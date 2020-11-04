---
layout: post
title:  "Data Collection 2"
date:   2020-11-04 01:30:37 -0400
categories: DataCollection2
---

### Long Term Viability Of Cards

Random Upgrade vs Random Card            |  Random vs Random Card | Random vs Random Upgrade |
:-------------------------:|:-------------------------:|:-------------------------:
![m](/Resources/RandUpgradeRandCardWinRatio.PNG)  |  ![m](/Resources/RandRandCardWinRatio.PNG) | ![m](/Resources/RandRandUpgradeWinRatio.PNG)
![m](/Resources/RandUpgradeRandCardCompetitiveness.PNG) |  ![m](/Resources/RandRandCardCompetitiveness.PNG) | ![m](/Resources/RandRandUpgradeCompetitiveness.PNG)

These 2 matchups show that hybrid use and card focus strategies are inferior to upgrade focus strategy in a randomized environment. Although, in theory card strategy can perform better if the player manages to pull of many well-timed cards in one game, I still wanted cards to have a better cost-benefit ratio in the long term by decreasing card costs and observing the results.

### Experimentation with Card Usage Ratio and Card Price

The table below shows the 200-game results when card usage ratio and card price change.

RandAI vs RandUpgrade  |Expensive Cards            | Cheap Cards
:-------------------------:|:-------------------------:|:-------------------------:
RandAI High Card Buy Ratio | ![m](/Resources/RandRandUpgrade00.PNG)  |  ![m](/Resources/RandRandUpgrade01.PNG)
RandAI Low Card Buy Ratio | ![m](/Resources/RandRandUpgrade10.PNG) |  ![m](/Resources/RandRandUpgrade11.PNG)

The table shows that when the card prices are halved, the hybrid strategy beats the upgrade-only strategy around when score to win within 5-50 range

### Adaptive AI: Mid Tracker vs Random and Mid Tracker vs Simple Scissor Lover

First to both test the MidTracker AI and to prove following a simple and boring strategy is not sufficient to win in Super Rock Paper Scissors, I made Mid Tracker play against Simple Scissor Lover.

#### Proving Adaptive Strategy Beats Simple 1-Weapon Strategy

![m](/Resources/TrackerScissorLover2WinRatio.PNG)

![m](/Resources/TrackerScissorLover2Competitiveness.PNG)

As seen from the results, Simple Scissor Lover stands no change against Mid Tracker

#### Tracker vs Random AI Problem

Although Mid Tracker's ability to form an opinion based on past actions by their opponent, its simple "choose the strong type of whatever I believe my opponent will choose" does not help enough against a random strategy.

![m](/Resources/TrackerRandWinRatio.PNG)

#### Better Tracker

The improved Mid Tracker2 has a weapon bias. It calculates probabilities of the opponent using a specific weapon similar to Mid Tracker, but unlike Mid Tracker the improved Mid Tracker 2 has a default prediction value for its biased weapon choice's strong type. For example let's assume Mid Tracker 2 keeps track of the past 3 weapons its opponent played which in order are: Rock, Scissor, Paper. This would mean that the prediction probabilities for Rock Paper and Scissor are, 0.33, 0.33, and 0.33. However, if Mid Tracker 2 has a bias for Paper with a random bias value of 0.37, then the tie would be broken and Mid Tracker 2 would choose Paper's strong type which is rock.

![m](/Resources/Tracker2RandWinRatio.PNG)

![m](/Resources/Tracker2RandCompetitiveness.PNG)

The Random AI gradually starts winning because Tracker AI relies too much on cards and not much on upgrades, so the switch at the 100 score to win mark can be ignored as the card/upgrade ratio wasn't well controlled in this experiment. This is something I am plan to fix next week.

### Next Week

Next week I want to look into game length. In theory, games can last forever if both players are following an identical deterministic strategy. I want to explore the implications of having a constraint that disallows such scenarios.


**Current Project Code Structure Of Super Rock Paper Scissors**

![m](/Resources/CodeAmount5.PNG)

**Weekly Time Spent on Project SeeSaw**

| Day  | Hours Spent |
|:-:|:-:|
| Thu | 0| 
| Fri | 0| 
| Sat | 2:00| 
| Sun | 2:00| 
| Mon | 0:00| 
| Tue | 0:00| 
| Wed | 4:00|
|TOTAL | 8:00| 