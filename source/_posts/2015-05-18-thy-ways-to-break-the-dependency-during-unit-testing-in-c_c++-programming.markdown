---
layout: post
title: "The ways to break the dependency during unit testing in C/C++ programming"
date: 2015-05-15 23:01:06 +0800
comments: true
categories: Others
author: Eric Zhang
---

We encourage people writes unit testing shall follow [F.I.R.S.T principle](http://agileinaflash.blogspot.de/2009/02/first.html "Fast, Isolated, Repeatable, Self-Validating and Timely"), but dependency gets in the way. Especially you make unit testing under legacy code, that's mess. That's the excuse we plan to ignore unit testing during coding.

For example, there is SUT code is `game.c`:


```c++
bool is_win() {
	printf("is_win is invoked!\n");
	if (dice_points() > 3) {
		return true;
	}
	return false;
}
```

function `is_win()` relies on the return value of function `dice_points()` which is defined in another file called `dice.c`:

```c++
int dice_points() {
	printf("dice_points is invoked!\n");
	return rand() % 6 + 1;
}
```

If we would like to test `is_win()`, see the test case like this:

```c++
TEST(VerifyGame, test_the_game_is_win) {
    CHECK(is_win());
}
```
The problem is you don't know the case will be pass or not, since the dependent function `dice_points()` return value is volatile, this test case is unrepeatable. So we need define a test double called `stub_dice_points()` to replace real implementation of `dice_points()`, and return a value which is configurable, like this:

```c++
static int m_points = 0;

void set_stub_dice_points(int point){
	m_points = point;
}

int stub_dice_points() {
	return m_points;
}
```

 And during you execute `is_win()` function, use `stub_dice_points()` instead of `dice_points()`, when you implement the unit testing like this:
```c++
 TEST (VerifyGame, test_bigger_than_three_points_shall_be_win) {
	 set_stub_dice_points(3 + 2);
	 CHECK(is_win());
}
```
But problem is how to replace the `dice_points()` with `stub_dice_points()`? There are some ways here:

## 1. Pre-compile macro
Macro `#ifdef`,  `#else` and `#endif` can be used during pre-compile phase, to select which source code statement can be compiled. So the SUT source code `game.c` can be modified like this:

```c++
bool is_win() {
	printf("is_win is invoked!\n");
#ifndef UNIT_TEST
	if (dice_points() > 3) {
#else
	if (stub_dice_points() > 3) {
#endif
		return true;
	}
	return false;
}
```

And during compile the source code for unit testing purpose, give the `-DUNIT_TEST` compilation option, to enable the replacement. But this way requires you modify productive source code, and there will be a lot of macro like this, too ugly, and the source code readability will be getting worse.

## 2. Function pointer replacement

Another way is define a function pointer, in production code, the pointer is points a real depended function, and during unit testing, the function pointer will point the stub function, but it requires programmer change the production code like this， for example, the `dice.h` change like this:

```c++
external int (*dice_pionts)();

```
and the `dice.c` change to this:

```c++
int dice_points_imp() {
	printf("dice_points is invoked!\n");
	return rand() % 6 + 1;
}

int (*dice_pionts)() = dice_points_imp;
```

After that, in the test case, you can use the function pointer re-assignment like below (in cpputest framework):

```c++
TEST(VerifyGame, test_the_dice_points_is_bigger_than_3_points_shall_be_win) {
	UT_PTR_SET(dice_points, stub_dice_points);
	set_stub_dice_points(3 + 2)
    CHECK(is_win());
}
```
The obvious disadvantage is production code changing is needed. You can image that, every function dependency shall be replaced by function pointer way, too ugly.

## 3. Linker links replaced object

Above two ways require to change the production code, is there any can avoid the production code change to complete the function replacement.
We assume the `game.c` includes `dice.h`, and `dice.c` is to implement the function `dice_ponits()` implementation. If we define another `stub.c` to implement the function `dice_points()` implementation in stub way. During compile the unit testing binary, don't compile the `dice.c`, just compile `game.c` and `stub.c`, and link them together to build a executable binary file. 

But there will be another problem occurs: How many `*.c` files under testing, there will be how many unit testing executable binary files. But, if the function `is_win()` and `dice_points()` in the same file, how to replace it? It will be difficult.

## 4. Dynamically replace 

Is there any way to replace the dependency without do any production code change? The answer is yes, here we would like to introduce a tricky way to replace the dependency: During the function invoking, we let the invoking jump to our defined stub function.
For example, during `isWin()` invoke `dice_points()`, we change the memory stack information, leads the invoking to jump to `stub_dice_points()`. The principle is first make the code page where `dice_points()` located writable using `mprotect()` in Linux or `VirtualProect()` in Win32. Then overwrite a `JMP` instructor which jump to `stub_dice_poinots()` at the begin of `dice_points()` binary code.

For example, there is a assistant source code `assistant.c` which includes `set_stup()` and `reset_stub()` implementation. The two are used to the dependency replacement:

```c++
//assistant.h
#ifndef __ASSISTANT_H__
#define __ASSISTANT_H__

#ifdef __cplusplus
extern "C" {
#endif

#define JUMP_CODE_MAX   0x05
#define JUMP_CODE_CMD   0xE9
#define JUMP_CODE_RET   0xC3

typedef struct stubInfo {
  void *funcAddr;
  unsigned char byteCode[5];
} stubInfo;

extern void set_stub(void *funcAddr, void *stubAddr, stubInfo *si);
extern void reset_stub(stubInfo *si);
#ifdef __cplusplus
}
#endif
#endif /* __ASSISTANT_H__ */
```

And the `assistant.c` is here:

```c++
#include <sys/mman.h>
#include <unistd.h>
#include "assistant.h"

static void set_jump_code(void *codeAddr, char jumpCode[JUMP_CODE_MAX]) {
  int pagesize = sysconf(_SC_PAGE_SIZE);
  if (mprotect((void*) ((unsigned long) codeAddr & (~(pagesize - 1))), pagesize, PROT_READ | PROT_WRITE | PROT_EXEC) != 0) {
    return;
  }
  memcpy(codeAddr, jumpCode, JUMP_CODE_MAX);
}

void set_stub(void *funcAddr, void *stubAddr, stubInfo *si) {
    char jumpCode[JUMP_CODE_MAX] = {JUMP_CODE_CMD};
    int  dist = stubAddr - funcAddr - 5;

    memcpy((void *)&jumpCode[1], (void *)&dist, sizeof(void *));
    si->funcAddr = funcAddr;
    memcpy((void *)&si->byteCode[0], (void *)funcAddr, JUMP_CODE_MAX);

    set_jump_code(funcAddr, jumpCode);
}

void reset_stub(stubInfo *si)
{
    char   jumpCode[JUMP_CODE_MAX];
    memcpy((void *)&jumpCode, (void *)&si->byteCode[0], JUMP_CODE_MAX);
    set_jump_code(si->funcAddr, jumpCode);
}
```
And then in the unit test file, you can use the assistant function for the dependency replacement:
```
TEST(VerifyGame, test_the_dice_points_is_bigger_than_3_points_shall_be_win) {
    stubInfo si;
    set_stup(dice_points, stub_dice_points, &si);
    set_stub_dice_points(3 + 2)
    CHECK(is_win());
}
```
## 5. Inherit from abstract class, replaced in dependency injection way

As we know, in OOP, there is interface concept, in C++, there is abstract class which used for interface purpose. If above the functions are defined as class way, there will be like this:

```c++
//game.h
class Game {
public:
    bool isWin() {
        Dice dice;
        if (dice.points() > 3) {
            return true;
        }
        return false;
    }
};
```
and the `dice` class is like this:
```
class Dice{
public:
    int points(){
        return rand() % 6 + 1;
    }
};
```
If we would like to make the replacement, we have to define an abstract class called `Player` like this:

```c++
class Player{
public:
    int points() = 0;
};
```
And the class `Dice` inherits from `Player` like this:

```c++
class Dice: public Player {
public:
    int points() {
		return rand() % 6 + 1;
	}
};
```
And the `Game` class shall make some change as well:

```c++
class Game {
private:
	Player m_opponent;
public:
    Game(Player player) {
		m_opponent = player;
	}
	
	bool isWin(){
		if (m_opponent.points() > 3){
			return true;
		}	
		return false;
    }
};
```
The constructor function is used to [dependency injection](http://martinfowler.com/articles/injection.html "Inject"). In test purpose, we will inherit a new class called `StubDice` from abstract class `Player`, and use the `StubDice` replace the real `Dice` in test case:

```c++
//stub_dice.h
class StubDice: public Player {
private:
	int m_points = 0;
public:
    void set_points(int points) {
		m_points = points;
	}
	int points() {
		return m_points;
	}
}
```
In the test case, the implementation as below:

```c++
TEST(VerifyGame, test_the_dice_points_less_3_points_shall_be_lose) {
	StubDice stub;
	Game *game = new Game(stub);
	stub.set_points( 3 - 1);
	CHECK(!game->isWin());
}
```

## Summary
We list the 5 ways of dependency breaking, it doesn't mean that we suggest you use the ways in unit testing. During unit testing, if we find that it's hard to make unit testing, we have to look back, check why it's too hard to implement unit testing. Is it the design problem? Is it too many dependency, why? Is there any other way to implement same requirements?

Treat the unit testing as design tool, helps you make better design; treat unit testing as safety tools, facilitates you make source code change.

-EOF-
