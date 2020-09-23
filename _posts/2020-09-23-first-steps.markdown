---
layout: post
title:  "First Steps"
date:   2020-09-23 12:30:37 -0400
categories: Experimentation
---

## Initial Experimentation

As I was experimenting with game balance, I realized that I wouldn't be getting solid results and data this week since I needed to have a medium where I can test game balance first.So I've come up with 2 initial game projects to conduct experiments on in the following weeks. This initial work over-head has shaken my confidence a bit, because I realized I would have to first create a solid medium to conduct my research on.

### Turn-based Rock Paper Scissors with upgrades and action cards. 

![m](/Resources/RPS1.PNG)

This is a game of Super Rock Paper Scissors made in Unity using C#.

#### How does it work?

**Round Structure**

Each round has 2 stages, manage & attack. In the manage stage the player can play an action card, upgrade the attack or defense power of one of their weapons (Rock, Paper, Scissor), or buy an action card. Upgrading weapons and buying action cards consume coins. 

**Money System**

Coins are based on the damage you deal to your opponent. For example if you attack opponent with your Scissor that has an attack score of 2.0 and your opponent chooses Paper that has a defense score of 1.0, you will gain 2.0-1.0 = 1.0 coin.

**Score System**

Score is calculated similar to coins, with two differences:
* You only earn 1 point for each interaction you win
* Score points cannot be spent, the game ends when one player reacher a certain score

**Action Cards**
There are many action cards you can buy and play. 

Some examples are:

Add 0.5 attack score to one of your weapons

Add 0.5 defense score to one of your weapons

Decrease one of opponent's weapon's defense score by 0.5 defense next turn

Decrease one of opponent's weapon's defense score to 50% next turn

Stun the enemy next turn if you win the interaction

Cleanse any effects you may be under next turn


You also gain a 1.5 attack score card whenever you reach a score multiple of 5.

#### Why Rock, Paper, Scissors?

Balancing a turn-based strategy requires many iterationgs of playtesting; however, it is one of the easier genres of video games to be tested using automated tools. Turn based strategy games allow for restricted parameter tuning, where an AI can play the game for you and answer some isolated questions such as:

**Is Starting Conditions Fair?**

Is the playing field symmetric? As in, does one player benefit from starting on one side or with one set of preferences. (eg Black and White pieces in chess)

**Does the game reward long-term or short-term resource management?**
A greedy agent that focuses on short term benefit analysis on a single score function can help with answering this question.

**Is the outcome known long before the game's end?**

Are there states in the game where the winnder is determined long before the game ends. This can be tested with agents that play the game from a certain point into the gameplay. The win rates and strategies can then be analyzed.

**How powerful is a given action or combination of actions?**

Is there an overpowered or underpowered strategy that completely breaks the game dynamics?

### Shoot-em-up Platformer

![m2](/Resources/Platformer1.png)

This is a Metroid-like platformer game made in Unity using C#.

#### Why a platformer game?

Platformer games, or single-player games in general, are tricker to balance since the fun factor less depends on power balances of certain moves and interactions with other players and more depends on arbitrarily defined "fun" parameters. 

For example, it is hard to quantify so-called the "goodness" of the location of an obstacle in a platformer game.

## Conclusion

I aimed to work on these two games in parallel, because I think they make a good contrast of limitations of automation and playtesting. 

In the following weeks, I plan to polish these games and make them debugging-friendly, so that the games can be started from any stated and controlled and tweaked by bots easily.

