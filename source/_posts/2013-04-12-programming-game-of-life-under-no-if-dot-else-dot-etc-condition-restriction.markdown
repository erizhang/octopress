---
layout: post
title: "Coderetreat: programming game of life under 'no conditional expression' restriction"
date: 2013-04-12 23:51
comments: true
categories: programming
---
If you search this topic with "coderetreat", "no if" or "no conditional expression" keywords, and if you are in coderetreat event right now, I suggest you close this topic, and after you finish the session, than open it again, and compare with your implementation, or comments your better implementation here.

I have facilitated several coderetreat events, not master it, but experienced. When in the session with rule ***No conditional expression allowed***, most of people will blame me why we have such restriction, or tell me WTF it's impossiable if we programming in C language. Minutes later, someone would ask me, shall we use "while loop" or "for loop"instead of "if", but I always say no. And later, someone told me that programming in OO polymorphism can be done, but how in C?

So how, is there any trick, I can only say don't know, but here I can give a sample of how to resolve the major logic of ["Conway's Game of Life"](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life "Game of Life"), rules of the game is:
 
 > 1. Any live cell with fewer than two live neighbours dies, as if caused by under-population.
 > 2. Any live cell with two or three live neighbours lives on to the next generation.
 > 3. Any live cell with more than three live neighbours dies, as if by overcrowding.
 > 4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
 
 So we can understand the next generation state will be based on two factors: current generation state (live or dead) and live neighours amount. We can define a two-dimensional array like below diagram illustrates:
 
 ![Alt text](/images/2013-04-12/rule_map.png "Rule Map")
 
The first row means current generation state is dead (0), for corresponding live neighbours amount, the next generation state of this cell shall be what (The value of array defined); similar, the second row means current generation state is live (1), for correspding live neighbours amount, the next generation state is defined as the value of array. The formula is: _**rule_map[current state][live neighbours amount - 1] = next generation state**_, for example, if current cell's state is dead, and it has 3 live neighbours, the result will be rule_map[0][3-1] = 1. The pseudocode shall be like this:
 {% codeblock lang:c %}
 /*0 means dead, 1 means live*/
 int rule_map[][] = {
     {0, 0, 1, 0, 0, 0, 0, 0},
     {0, 1, 1, 0, 0, 0, 0, 0},
 };
 
 int next_state(int cur_state, int neighbours)
 {
     return rule_map[cur_state][neighbours - 1];
 }
 {% endcodeblock %}


So how to calculate the live neighbours amount of specific cell? Please you make a post for it. I'm not going to clarify everything here, or you can comment below. Have fun. :\
