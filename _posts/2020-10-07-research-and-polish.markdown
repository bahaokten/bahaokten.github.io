---
layout: post
title:  "Designing a Game With Balance In Mind Part 2"
date:   2020-10-07 10:30:37 -0400
categories: Experimentation
---

This week has been a busy week, so the update will be brief

### Polishing the Super Rock Paper Scissors Code

I've spent some time converting all internal state changes to be done through the pub/sub system, so that more internal actions can be controlled by external tools.

Card system is now semi-functional, players and AI can play and buy cards alike.

### Turn-Based AI Research

Along with the Super Rock Paper Scissor's development, I looked into how I can best write Turn-Based AI for the game.

### Turn-Based RPG AI

<iframe width="560" height="315" src="https://www.youtube.com/embed/B8f16Iew8DY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Gives a good idea of turn-based game structure and a brief example of simple Turn-Based RPG AI.

#### Designing AI Algorithms For Video Games

[Link](https://www.gamasutra.com/view/feature/129959/designing_ai_algorithms_for_.php)

Talks about a scoring system can be used to prioritize certain actions that AI may choose. This is especially useful since I want to be able to alter how much the AI values an action over the others.  

#### Beginner's Guide To Game AI

[Link](https://www.gamedev.net/tutorials/programming/artificial-intelligence/the-total-beginners-guide-to-game-ai-r4942/)

![m](/Resources/AIStates.PNG)

Explains what Game AI is and what it is not and goes over some common design patterns for creating Game AI.

It mentions that the Game AI has 3 steps: Sense, Think, and Act.
Mentions that the AI is supposed to provide entertainment and challange rather than be optimal, and that the AI should make human-like decisions and feel natural.

While I will be writing AI that stress-tests the game by following extreme strategies, I also want to make sure I have AI that makes human-like decisions to monitor the game-play without needing to playtest with real players.

Explains AI decision making from several design patterns. Goes over decision trees, finite state machines, hierarchical state machines, behavior trees, utility-based systems, planning and pathfinding AI options, weight-based adaptation, markov models, and n-grams.


### Citations

1) https://homes.cs.washington.edu/~zoran/jaffe2012ecg.pdf

2) https://github.com/bahaokten/Research_RPC)https://www.gamasutra.com/view/feature/129959/designing_ai_algorithms_for_.php

3) https://www.gamedev.net/tutorials/programming/artificial-intelligence/the-total-beginners-guide-to-game-ai-r4942/



**Weekly Time Spent On Project SeeSaw**

| Day  | Hours Spent |
|:-:|:-:|
| Thu | 4:00| 
| Fri | 0| 
| Sat | 0| 
| Sun | 0:00| 
| Mon | 2:00| 
| Tue | 0:00| 
| Wed | 3:00|
|TOTAL | 9:00| 