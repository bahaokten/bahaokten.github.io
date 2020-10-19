---
layout: post
title:  "AI Players"
date:   2020-10-14 10:30:37 -0400
categories: AI
---

### First Game Balance Change Before Logging tools and Advanced AI

**Observation** 

AI that focuses on upgrading Defense and/or using Defense-buff cards tend to lose more often than attack-centric AI.

**Deduction** 

Defense buffs never give you a winning edge over your opponent. Defense is only useful when you lose the round and you want to minimize your opponent's earnings. Defense buffs are either too expensive or defense is a redundant attribute.

**Solution**
To give defense attribue a purpose, I decided to make defense have an impact on picking the round winner.

**Balance Changes**

When both players pick the same weapon, their weapon's Attack and Defense attributes are summed up. Whichever player has the greatest sum win the round. Coins are calculated as winners ```sum - losers sum + 1```

**Conclusion**

Since defense is also cheaper to upgrade, this means it's crucial to have high defense in Weapon X vs Weapon X rounds to both win the round and earn large amount of coins that you would not be able to win in any other X vs Y round.

So now defense has a role in deciding the round winner, as well as acting as a trump card to win a lot of coins in one round.

I am amazed that even in the early stages of creating AI and Logging tools to balance the game, the tools are already helping me find out glaring issues with the game design.

### First Bug Discovered By Simple AI

**Expectation**

When a card is used, its effect is active only for the next round.

**Observation**

When greedy attacker uses a card, its effect sometimes lasts for multiple rounds

**Solution**

Fixed the code where the child card class was wrongly calling its parent's disable function.


https://www.reddit.com/r/Unity3D/comments/6qg197/readingwriting_csvs_in_unity/
https://forum.unity.com/threads/using-external-libraries-in-unity.410741/
https://gamedev.stackexchange.com/questions/166401/use-a-different-target-framework-version-in-a-unity-c-project-other-than-4-6


### Links

**Github repo for Super Rock Paper Scissors: [Click Here](https://github.com/bahaokten/Research_RPC)**

**Webbuild: [Click Here]({{ site.baseurl }}{% link Builds/Week5/index.html %})**


**Current Project Code Structure Of Super Rock Paper Scissors**

![m](/Resources/CodeAmount2.PNG)

**Weekly Time Spent On Project SeeSaw**

| Day  | Hours Spent |
|:-:|:-:|
| Wed | 3:00| 
| Thu | 0| 
| Fri | 0| 
| Sat | 0| 
| Sun | 4:00| 
| Mon | 4:00| 
| Tue | 0| 
| Wed | 3:00|
|TOTAL | 14:00| 