---
layout: post
title: "Exploding Fake in Unit Test"
date: 2016-10-24 21:22:49 +0800
comments: true
categories: Programming
author: Eric Zhang
---

## What's **exploding fake**?
let's see below source code firstly.

```c
int a_stub(){
	fail("Boom!!! I shouldn't have been called!!!");
	return 0;
}
```
Even more popular:
```c
int a_stub(){
	fail("Hey!!! I'm actually called! Now you can think about "\
	"what behaviour do you want me to have. Do you want me to "\
	"return a specific value?");
	return 0;
}
```

So where this **exploding fake** will be used? For example, you would like to add unit test case for legacy code, in order to pass the compilation, you have to add hundreds of stubs, but you do NOT have enough time to check what's the stub purpose and return value, so you can make the stub be self exploding first. And then you can pass the compilation and add unit test case. Once there is stub exploding, then to check whether the stub shall be called, shall the stub have behaviour, and what's return value.

## Automatically generate exploding fake?
As we know, lots of stubs are needed during unit testing for legacy code, is there any easy way to generate exploding fake automatically?  

Because the stub is the dependency's function signature, actually, so, if you can get the dependancies' signature, that will be easy to generate, for example, tool [lizard](https://github.com/terryyin/lizard.git "lizard repo") could read all the functions signature, it will be good way to write a script to generate **exploding fake**.

Actually, James Grenning has already implement such script for the **exploding fake** generation: [source>>](https://github.com/jwgrenning/cpputest-starter-project/tree/master/tests/exploding-fakes "exploding fakes")
## Does make sense to making exploding fake for dynamic programming language?
No, because the dynamic programming language is not complied and linked, it will be naturally failed once the dependency missing. In another words, it's self-exploding type, :p

-- EOF --

