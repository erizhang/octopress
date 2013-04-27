---
layout: post
title: "Take care of your integer variables"
date: 2013-04-27 13:19
comments: true
categories: programming
author: Eric Zhang
---

We produce telecom equipment, the equipments will be deployed as infrastructure of telecommunication, internet etc., so it required product high stability, high security. For these reason, we have to review our code time and time again, test and test again, we worry about there will be a 911 phone call cannot be reachable since our software bug, or financial exchange be stuck since networking problem which is caused by our software bug. We shall understand that we are making the infrastructure of society, government, financial, security, even national defense. 

I recall that one day, Erno Jeges (who is secure coding expert) told us: you have to care your code as care your baby, otherwise you don't know what your baby will be. Even so, we still found lots of problems regard software security: buffer overflow, print formatting, and integer overflow etc. I'm not expert in this area, but I notice that at least we have to take care of our integer variable first since C/C++ languages handle integers in a very dangerous manner since there are:

* No overflow exception
* No run-time detection if a negative integer is converted to an unsigned value
* No checks whether a larger integer value is put into a shorter variable

That's why in our source code hides one integer number multiply by one integer number, and leads software fiery crash. Of cause, sometimes, we can rely one static analysis tool such as Klocwork, cppcheck, but not all of problems can be filtered by tool, human shall participate in the process of software creation. As these reason, programmer who is programming in C/C++ language shall be carefully deal with the calculation or convention of integer variable, and it shall be basic knowledge of C/C++ programmer. That's why I act Longevity Monk who from [A Chinese Odyssey](http://en.wikipedia.org/wiki/A_Chinese_Odyssey "A Chinese Odyssey in Wikipedia") here.

![Alt text](/images/2013-04-27/tangseng.jpeg "Longevity Monk")

let's look at this code snippet:
{% codeblock lang:c %}
int copy(char *src, int len)
{
    char dest[50];
    if (len > 50) {
        return -1;
    }
    memcpy(dest, src, len);
	//TODO: use dest here

	return 0;
}
{% endcodeblock %}
Could you please tell me is there any problem if you stop here and do not plan to go through this post? Of cause, there is a big problem once assign a negative value to parameter <code>len</code>, for example, we assign the <code>len</code> is <code>-1</code>, the program can be executed in <code>memcpy(dest, src, len)</code>, and <code>-1</code> will be casted to unsigned value <code>0xffffffff</code>, and lead the program Segmentation fault, this problem is called **Signedness bug**.

Okay, let's see another code snippet here:
{% codeblock lang:c %}
int append(char *str1, char *str2, unsigned int len1, unsigned int len2)
{
    char dest_buf[64];
    if ((len1 + len2) > 64) {
        return -1;
    }
    memcpy(dest_buf, str1, len1);
    memcpy(dest_buf + len1, str2, len2);
    //TODO: dest_buf usage can be added here
    
    return 0;
}
{% endcodeblock %}
How about this one, is there any problem? In case of len1 is huge number, and <code>len1+len2</code> can be overflow to a small number. For example, <code>len1 = 4294967198</code>, and <code>len2 = 100</code>, and then <code>len1 + len2 = 4294967298 (0x100000002)</code>. We call this problem is **Arithmetical overflow**.

one more:

{% codeblock lang:c %}
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[])
{
    unsigned short s;
    int i;
    char buf[80];
    if (argc < 3){
        return -1;
    }
    i = atoi(argv[1]);
    s = i;
    if (s >= 80) {
        printf("string is too long!\n");
        return -1;
    }
    memcpy(buf, argv[2], i);
    buf[i] = '\0';
    return 0;
}
{% endcodeblock %}
If the <code>i</code> is <code>"65535"</code>, the <code>s</code> will be <code>0</code>, and lead segmentation fault during memcpy, we call it **Widthness integer overflow**. So here we would like to highlight the ranges of integer, we believe professional programmer can deal it well.

![Alt text](/images/2013-04-27/integer-ranges.png "integer ranges
")

And now it's time to review you owned source code, is there any same problem? Have you group the integer calculation invoking in some centralized place rather than spread everywhere of your program? Have you clearly understand using variable is 2byte, 4bytes or 8bytes? signed or unsigned? 

Be careful! :\

*(Knowledge I learned from [Computer Systems: A programmer's perspective](http://www.amazon.com/Computer-Systems-A-Programmers-Perspective/dp/013034074X "Amazon link"), and emphasize the mindset in secure coding course)*
