---
layout: post
title: "Misunderstanding of Unit Testing"
date: 2015-02-03 22:01:06 +0800
comments: true
categories: Programming
author: Eric Zhang
---

In software development, unit testing is used for method/function verification. Most of people agree that unit testing helps on software quality improvement, but no many people deep dive. Have you really understand the relationship between unit testing and quality? 

Most of people knows while coding a method/function, the corresponding unit testing shall be added as well. But, why? And a lot of people has misunderstanding about unit testing, I'd like to list some ones here.

## Misunderstandings

### 1. Ask newbie to write unit testing

Who shall be the author of unit testing? definitely shall be the coder himself/herself. But the situation likes this: once you finish the function/method implementation, and use a practice called "debug" to verify your method/function works well, then you will think the task is done, any else (unit testing) is optional. If you are key person, another task will be assigned to you, and the unit testing always will be the low prioirty task, and finally, it's assigned to a newbie who has the competence to take the "low level" tasks.

But unit testing purpose is to facilitate source code change, and acts as safety during coding, in TDD pratice, unit testing is used to drive development. Do you think your coding shall be driven by a newbie, how can you trust the newbie understand the requirement clearly. If you are rock climber, do you trust a newbie install a safety tool for your climbing?

So, the correct pratice shall be experieced person do requirement analysis and write unit testing, let the newbie to implement the function/method, your unit testing case to verify the implementation is okay or not, you take the responsibility to facilitate the source change, you are the safety officer.

### 2. Expect to find bug through unit testing

As we know, there are two types of testing called: exploratory testing and verification testing. Unit testing is verification testing, it's used to verify the method/function does fulfill the requirements or not. Unit testing the executable requirement specification of the product method/function.

The correct pratice shall be write the executable requirement as unit testing case, and implement the product method/function untill all of related unit testing cases are passed.

If you would like to find more bug, please make exploratory testing manually, it requires experienced tester, and be more creative.

### 3. Pursue high coverage rate

I know, there might be a KPI in your team, the unit testing line coverage rate shall be higher 80%, function coverage rate shall be 100%, and branch coverage rate, conditional coverage rate. If you can reach 100% coverage for all of above four kinds of coverage, your manager will thumb up and say "well done". We spend a lot of time to pursue high coverage rate, did you think that does it worth? 

Let's look below source code
```c
int foo(int x, int y)
{
    int z = 0;
    if ((x > 0) && (y > 0)) {
        z = x;
    }
    return z;
}
```
We use `assertEquals(2, foo(2, 2))` can touch all line of the code, it means the line coverage rate is 100%, but does it mean the source code has been approven okay, I don't think so.

Source code metrics somehow likes the weather forecast report indicates, single metric does not make sense, you have to combine all of the metrics, and give you insight. Metrics just tell you there may be something wrong, you need to spend time deep dive in the source code.

Some managers are used to challenge team with metrics, because they read excel daily, but never read your source code.

Josphal has written a blog to introduce the [test coverage rate role](http://www.infoq.com/cn/articles/test-coverage-rate-role "test coverage rate role"), above example is from his blog.

### 4. Write unit testing for legacy code

I really appreciate people starts to care about the legacy code, start to make refactoring, that's good. But only add unit testing to legacy code without refactoring legacy code is dangerous. There is a manager required his team soruce code (including legacy code) unit testing coverage rate shall reach higher 80%, so team member spent a lot of time to write the unit testing for the legacy code, and tried their best time to cover each line of code, branch, and conditions. The unit testing totally was implemented in white-box, it means if the source code logic change, there will be unit testing case failed.

My question is:

- Is your legacy code testable?
- Is your new added unit testing case maintainable?

Your unit testing case will be technical debts as well as the legacy code, trust me. Several years ago, I read a post which is written by Steven Sanderson, called [Selective Unit Testing](http://blog.stevensanderson.com/2009/11/04/selective-unit-testing-costs-and-benefits/ "selective unit testing") gave a great strategy to handle legacy code unit testing according to the cost and benefit balance. The article shows the legacy code can be identfied four types according to the benefit and cost. As we know, the unit testing implementation cost depends on the source code dependancy, high code dependancy means high unit testing cost:

![Alt text](http://blog.stevensanderson.com/wp-content/uploads/2009/11/image.png "selective unit testing")

- **Algorithm**: This kind of code has complex calculation logic, but lack dependancy, all calculation is depends on the argument input value, and then give out an output. This code is exterem important, any error will lead the program computing failed. So we called it's high benefit and low cost, the unit testing for this kind of code is required.
- **Trivial**: This kind of code has simple logic, sometimes, the method just several LoC, and there is more dependancy with other source code, we call it low benefit and low cost. So we think the unit testing for this code is optional.
- **Coordinator**: This code acts as coordinator role, it coordinate methods/functions work, it helps the methods integration, and used to describe a specific functional scenario, for example, the initialization, main etc. It has a lot of dependancy with other source code, because it needs to invoke other functions/methods. We call it high cost, but low benefit, because the logic is simple. A lot of dependancies means you need to stub or mock, that's high cost. For legacy code, we won't make unit testing for it. Instead, we use functional testing or module testing to cover the sceniro requirements.
- **Overcomplicated**: This kind of code has the characters of Algorithm and Coodinator code, complex logic, and has a lot of dependancies with other code. For this kind of code, we have to refactor it to Algorithm and Coordinator codes, and then based on above strategy to decide to make unit testing or not.


### 5. Treat unit testing as white-box testing

 As academism, the unit testing is white-box testing, but I'm hard to agree that, I suggest people write unit testing as black-box rather than white-box.

If we write unit testing as black-box, in another words, the test cases describe the method/function requirement, as we know, there are kinds of implementations for same one requirement. If the implementation has been changed, the unit testing case still can be used to verify the method/function correct or not. But, if you implement the unit testing in white-box way, once the production code implementation changes (method/function signature is not changed), you have to update your test case as well. How bad.

Unit testing implemention shall be align the requirement, not the implementation logic.

## Reference:

- [Selective Unit Testing - Cost and Benefits](http://blog.stevensanderson.com/2009/11/04/selective-unit-testing-costs-and-benefits/ "selective unit testing")
- [Test Coverage Rate Role](http://www.infoq.com/cn/articles/test-coverage-rate-role "test coverage rate role")

-EOF- 




