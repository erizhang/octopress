---
layout: post
title: "How to successfully host and facilitate coderetreat"
date: 2015-11-12 17:06:08 +0800
comments: true
categories: programming
author: Eric Zhang
---


![Alt text](/images/2015-11-12-how-to-successfully-host-and-facilitate-coderetreat/pair-work.png  "coderetreat pair work")

## 1. What's coderetreat
Coderetreat is a day-long, intensive practice event, focusing on the fundamentals of software development and design. By providing developers the opportunity to take part in focused practice away from the pressures of 'getting things done', the coderetreat format has proven itself to be a highly effective means of skill improvement. You can visit [coderetreat.org](http://www.coderetreat.org/ "coderetreat") for details.

## 2. What consists of  the coderetreat day?
The day-long activity including: beginning, coding sessions and ending. Consider the time arrangement, there will be at least 5 sessions during that day. Beginning is reserved to introduce the coderetreat activity, sponsor, and the rules etc. The coding sessions are the major part of coderetreat day, the details will be described below. And ending part is used to retrospective of day-long whole coderetreat activity.

## 3. What's consists of the session?

In each session, there basically including below steps:

### Step 1. Delete the source code what is written in previous sessions

I often was asked why we programmer shall delete the source code what is written in previous session by them, they treated the source code as asset, programmer always makes the increase the source code size, but has less opportunity to delete source code.  And remove the source code permanently helps programmers enter in new session with brand new status, new session with new constraint will make programmer has new idea.

### Step 2. Add new constraint in constraints-list

The most interested part in coderetreat is coding under constraints. Image that, if your hands are tied, you will consider use other parts of your body. Before prepare coderetreat, facilitator shall prepare enough coding constraints carefully. Add one constraint in one session, current session shall be follow previous all constraints and new added in current session. For example, in the first session, the constraint is "TDD is required", and in the second session, there is new constraint called "No local/global variable used", it means in the second session, programmers shall follow two constraints.

Add constraints can help programmer programming in different solution, one constraint would help programmer to realize his/her potential well.

Some constraint examples:

- **TDD is required**
- **The LoC of each function shall be less than 6**
- **No conditional statement used**, for example, "`if`, `if else`, `?: etc`" shall be forbidden, of cause, it does not allow use `for` or `while` loop to instead of conditional statement
- **Follow "**Tell, don't ask" principle**
- **No loop**
- **Keyboard only**, no mouse, no touchpad
- **Silent**: all of the programmer shall be silent, no talking, it helps programmers use the source code and unit test case to make communication, test case and source code shall be readable

### Step 3. Coding to solve the problem

Actually, in coderetreat, we suggest every two programmers pair programming together, and in different sessions, the pair partner shall be rotated. Alice pair programming with Denial in session 1, but Alice shall pair programming with someone else. It helps programmers get new idea, and help them how to work with new partner. During coding, as facilitator shall walk through all of pair, to get to know what's the situation, is there any problem, if someone stuck there, facilitator can give a clue to help them keep going forward.

### Step 4. Debrief and share each other

In each session, shall reserve at least 5~10 minutes to collect the feedback from participants. As facilitator, you can ask "What's your feeling in this session?", "Is there any problem you encounter, and how do you resolve it?", ask programmers write down the answers on the post-it, and paste them on the wall. and share each other. To facilitator, frequency quick feedback can help to adjust coming session's plan. To participants, it's a easy way to learn other's idea well.

![Alt text](/images/2015-11-12-how-to-successfully-host-and-facilitate-coderetreat/session-type.jpg =400x "session sharing")
![Alt text](/images/2015-11-12-how-to-successfully-host-and-facilitate-coderetreat/session-sharing.jpg =400x "share the idea")

## 4. As a host, what's shall be prepared before the day?

As a host, you shall provide a comfortable workplace for programmers, so some logistics including:

- A good place
- Nice table and chair
- Food, snack and drinking

Since in the activity, there are a lot of sharing to make, so below shall be prepare as well:

- Projector
- Whiteboard
- White paper
- Marker pen
- Post-it (more color)

If you consider interact with other coderetreat place:

- Camera for live recorder
- Audio device for live broadcast play

Enroll you coderetreat on [coderetreat website](https://www.coderetreat.org "coderetreat.org") , if you're going to organize a GDCR (Global Day of Coderetreat), please don't forget register it on [gdcr website](http://gdcr.coderetreat.org/ "gdcr"), and create you owned registration, and publish it for participants enroll. Of cause, if you registered your activity in the gdcr website, you will get support from administrator. Post your activity on twitter with tag "#gdcr", "#gdcr15" or "#coderetreat" etc. for great promotion.

## 5. As a facilitator, what's shall be focused on the sessions?

Remember that: YOU ARE FACILITATOR. This is important role during the activity, your passion can inspire people enjoy the activity well.

- First of all, you shall understand the whole process of coderetreat,
- Then understand the constraints well, you can explain every constraint,
- Know the problem well, and solve the problem under the constraint before the activity start.
- Before session, you shall clear delivery the requirements to participants, and inspire people
- In in the session, you shall walk through the pair, and getting to know the status, and facilitate the programmers in the right way
- In the end of the session, you shall use right tools to get enough feedback, and help participants share the idea each other.
- In the coderetreat ending part (the last activity of whole day), you shall make whole day retrospective.

![Alt text](/images/2015-11-12-how-to-successfully-host-and-facilitate-coderetreat/ending-retrospective.jpg "retrospective in ending")

### How to make whole day retrospective?
 Actually, there are three questions facilitator shall ask:

 1. What's your feeling today?
  1. What's you learned today?
   1. What are you going to do related coderetreat?

You can split the participants in different groups, each group has 10 persons, give everyone three green color post-it, and write down the first question's answer, write down 3 feelings, and paste them on the white paper external circle, and then give each member 30 seconds to share his/her feeling one by one in the group.

Then give everyone two yellow post-it, and write down the second question's answer, write down 3 learning, and paste them on the white paper middle circle, share his/her learning one by one in the group.

In the end, give each person one red post-it, and write down the third question's answer, write down the action plan, and paste it on the white paper inner circle, share the action plan one by one in the group.

![Alt text](/images/2015-11-12-how-to-successfully-host-and-facilitate-coderetreat/retrospective-way.jpg "retrospective way")

-EOF-
