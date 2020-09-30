---
layout: post
title:  "Designing a Game With Balance In Mind"
date:   2020-09-23 12:30:37 -0400
categories: Experimentation
---

### What it takes to make a "balancable" game

Researching the tool, techniques and design patters used for game balance revealed that before any of these methods are used, the game first has to be designed to be "balancable".

On that note, while designing **Super Rock Paper Scissors**, I had to think carefully on how I can make the game's infrastucture probable by the tools, and techniques I will be using to balance the game. 

#### Concise and Independent Variables
Variables critical to gameplay had to be well-defined and each variable ideally had to affect the gameplay in a single independent factor (as in critical variables should not depend on each other's value too much). I predict that keeping variables concise and indepent will make it easier to isolate certain gameplay mechanics and focus the game balance tools on each mechanic one by one. (Something similar to what is suggested in [1])

#### A Secondary User Interface
For balancing **Super Rock Paper Scissors** I wanted the game to be fully playable without a GUI, so that the game can also be played by the AI. I created an API that allows one or both players to be controlled by AI. 

**This means that the game can be played in couple of states:**
* AI vs AI, No GUI (Work In Progress, I plan on creating a logging/spectating service to dump the log game progress in to a file and analyze game balance changes)
* AI vs AI, Animations On (Spectate mode, watch 2 AIs compete in a game of Super Rock Paper Scissors)
* AI vs User, Animations On (Hybrid mode, animations on means the transitions are slow and animated)
* AI vs User, Animations OFF (Hybrid mode, animations off means the transitions are fast)
* User vs User, (This mode doesn't make much sense in a competetive setting since both players can see each others cards and actions. The mode exists solely for testing purposes.) 

AI plays the game using the PlayerAPI. The PlayerAPI has similar capabilities to pressing buttons on game's UI.

![m](/Resources/APICard.PNG)

**Current Project Code Structure**

![m](/Resources/CodeAmount1.PNG)

### How to Play? (Updated)

#### **Round Structure**

You start the game with 5 coins and 0 score.

**Each player round consists of 3 phases:**
* Buy: In this phase you are allowed to buy as many cards as you want as long as you have the money for it.
* Action: In this phase you can either use a single card or upgrade one of your weapons' defense or attack attributes.
* Attack: In this phase you pick a weapon to attack your opponent. The attack initiates one both players finish the 3 phases.

#### **Winning Rounds and Score System**

You win the round if you correctly pick the weapon that counters your opponent's weapon. For example if you picked scissor and your opponent picked rock, you win the round and gain +1 score.
However, if you both pick the same weapon, then the player with the highest attack attribute on their weapon wins.

The first person to reach a certain score wins the game.

#### **Coins System**

Coins are earned based on the damage you deal to your opponent on a winning round. Damage is calculated as `your opponent's weapon's defense attribute - your weapon's attack attribute + 1`. If the damage dealt is less than 1, it is rounded up to 1.

#### **Action Cards**
There are many action cards you can buy and play. 

**Some examples include:**

* Add 0.5 attack score to one of your weapons

* Add 0.5 defense score to one of your weapons

* Decrease one of opponent's weapon's defense score by 0.5 defense next turn

* Decrease one of opponent's weapon's defense score to 50% next turn

* Stun the enemy next turn if you win the interaction

* Cleanse any effects you may be under next turn

* If you win the next turn, gain 50% extra coins


You gain a +0.5 attack score card whenever you reach a score value multiple of 5.

#### **How it differs from Rock Paper Scissors**

In tradinitional rock paper scissors, the biggest factor in winning the game is having good luck. 
A good rock paper scissors will aim to have a perfectly randomized patters while trying to guess the other person's habits.
Super Rock Paper Scissors aims to amplify the guessing stage by altering the relative power dynamics within rock, paper, scissor.
For example, when a player invests coins to upgrade their rock, they will have a winning matchup against your rock. They might have a bigger incentive to play rock more often since their rock now has 2 winning matchups out of 3 possible. 

### Links

**Github repo for Super Rock Paper Scissors: [Click Here](https://github.com/bahaokten/Research_RPC)**

**Webbuild: COMING SOON**

### Citations

1) https://homes.cs.washington.edu/~zoran/jaffe2012ecg.pdf