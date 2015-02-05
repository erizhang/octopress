---
layout: post
title: "Misunderstanding of Unit Testing"
date: 2015-02-03 22:01:06 +0800
comments: true
categories: Programming
author: Eric Zhang
---

In software development, unit testing is used for method/function verification, and most of people agree that, unit testing helps software quality is common understanding, but rarely deep dive. I prefer any discussion shall be based on expclit concept definition.

But corresponding unit testing during coding is common sense in software development, so here I'd like to introduce some misuderstanding area of unit testing here.

## Misunderstandings

### 1. Ask newbie to write unit testing

Who shall be the author of unit testing? definitely shall be the coder himself/herself. But the situation likes this: once you finish the function/method implementation, and use a method called "debug" to verify your method/function works well, then you will think the task is done, any unit testing else is optional. If you are key person, another task will be assigned to you, and the unit testing will be low prioirty task, and be assigned to newbie.

But unit testing purpose is to facilitate source code change, and safety tools during coding, and in TDD pratice, unit testing is used to drive development. Do you think your coding shall be driven by a newbie, how can you trust the newbie understand the requirement clearly. If you are rock climber, do you trust a newbie install a safety tool for your climbing?

So, the correct pratice shall be experieced person do requirement analysis and write unit testing, let the newbie to implement the function/method, you verify the implementation is okay or not, you take the responsibility to facilitate the source change, you are the safety officer.

### 2. Expect to find bug through unit testing

As we know, there are two type of testing called: exploratory testing and verification testing. Unit testing is verification testing, it's used to verify the method fulfill the requirements or not. Unit testing is the method/function requirement implementation in test case way.

The correct pratice shall be write the executable requirement as unit testing case, and implement the method/function untill all of corresponding unit testing cases are passed.

If you would like to find more bug, please make exploratory testing manually, it requires experienced tester, and be more creative.

### 3. Pursue high coverage

I know, there might be a KPI for your team, the unit testing line coverage shall be 80+%, function coverage shall be 100%, and branch coverage, conditional coverage. If you can reach 100% coverage for all of above four kinds of coverage, your manager will thumb up and say "well done". We spend a lot of time to pursue high coverage, did you think that is it worth? 

Let's check below source code
```
int foo(int x, int y)
{
    int z = 0;
    if ((x > 0) && (y > 0)) {
        z = x;
    }
    return z;
}
```
We use `assertEquals(2, foo(2, 2))` can tough all line of the code, it means the line coverage rate is 100%, but does it mean the source code has been approven okay, I don't think so.

Source code metrics somehow likes the weather forecast report indicates, single metric does not make sense, you have to combine all of the metrics, and give you insight. Metrics just tell you there may be something wrong, you need to spend time deep dive in the source code.

Some managers are used to challenge team with metrics, because they read excel daily, but never read your source code.

Josphal has written a blog to introduce the [test coverage rate role](http://www.infoq.com/cn/articles/test-coverage-rate-role "test coverage rate role"), above example is from his blog.

### 4. Write unit testing for legacy code

I really appreciate people starts to care about the legacy code, start to make refactoring, that's good. But only add unit testing to legacy code without refactoring legacy code is dangerous. There is a manager require his team soruce code (including legacy code) unit testing coverage rate shall reach above 80%, so team member spend a lot of time to write the unit testing for the legacy code, and try their best time to cover each line of code, branch, and conditions. The unit testing totally is implemented in white-box, it means if the source code logic change, there will be unit testing case failed.

My question is:

- Is your legacy code testable?
- Is your new added unit testing case maintainable?

Your unit testing case will be technical debts as well as the legacy code, trust me. Several years ago, I read a post which is written by Steven Sanderson, called [Selective Unit Testing](http://blog.stevensanderson.com/2009/11/04/selective-unit-testing-costs-and-benefits/ "selective unit testing") gave a great strategy to handle legacy code unit testing.

### 5. Treat unit testing as white-box testing

If we write unit testing as black-box, in another words, the test cases describe the method/function requirement, as we know, there are kinds way of implementation for same requirement. If the implementation has been changed, the unit testing case still can be used to verify the method/function correct or not. But, if you implement the unit testing in white-box way, once the production code implementation changes (method/function signature is not changed), you have to update your test case as well. How bad.

## Reference:

- [Selective Unit Testing - Cost and Benefits](http://blog.stevensanderson.com/2009/11/04/selective-unit-testing-costs-and-benefits/ "selective unit testing")
- [Test Coverage Rate Role](http://www.infoq.com/cn/articles/test-coverage-rate-role "test coverage rate role")

-EOF- 




