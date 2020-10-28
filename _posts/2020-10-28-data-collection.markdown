---
layout: post
title:  "Data Collection"
date:   2020-10-28 01:30:37 -0400
categories: DataCollection
---
### Excel For Data Analysis

I decided to use excel to summarize and analyze the logs. I created an excel file which creates a summary table and graphs for generated csv logs

[Example_Excel_Summary_Sheet]({{ site.url }}/Download/ExampleExcel.xlsx)

[Example_Excel_Graph_Sheet]({{ site.url }}/Download/ExampleExcel2.xlsx)

### Bug Discovery

Finally having the Game, AI Bots, and logging working properly, I was ready to start collecting data.
Before I did anything fancy, I wanted to prove that a random AI can be beaten by some simple strategy, hence proving that I succeeded at decoupling chance factor from winning a game of Rock Paper Scissors.

On paper, it seemed like SimpleScissorLover should be able to take down a random strategy since focusing on upgrading and buffing 1 weapon usually means you win 2 of the 3 possible matchups given enough rounds to play.

However, when I actually ran the tests, I couldn't make sense of the results.

![m](/Resources/StatsRandBug2.PNG)

![m](/Resources/StatsRandBug1.PNG)

As can be seen by the results, random strategy was winning against all the revisions of SimpleScissorLover AI. I calculated the theoretical probability of random strategy winning, and the math did not align with the results. I suspected there might be a bug in the winner determination code. Debugging the code revealed there, in fact, was a bug.

![m](/Resources/StatsRandBug3.PNG)

The code above handles determining the winner when both players pick the same weapon. As can be seen in the code, the Left player had a huge advantage since the second else clause that is suppose to check if player R won the round had it a faulty conditional expression that would never let player R win.

### Proving Random Strategy Can Be Beaten

Once I fixed the bug, I moved on to proving that the random strategy was not optimal. I think this is an important first step, because proving random strategy to be subpar to other strategies hints that the game can have interesting competitive decisions, which from a game balance standpoint is a neccessity for a 1v1 game to have.

Before I show you the data though, I want to introduce the competetiveness score I've come up with to better interpret the data.

#### Competitiveness Metric

I've come up with a competitiveness metric to better make sense of the data.

**It's calculated as such:**

* Sum up round wins for each round outcome (player L wins, player R wins, stalemate) across all logged games

* Find the average round wins per game for each round outcome

* Divide the averages by the score needed to win

* Add up all these averages for each round outcome

* The total score represents game length (competitiveness) and each individual averages for round
outcomes represent individual performace (performance of players and stalemate).

#### **Simp_Random Vs Simp_ScissorLover2**

I've made these 2 AI play against each other 5 games with differing score to win values (1,5,20,50,100). Score to win represents the score which a player needs to reach to win the game, score is earned by winning rounds.

#### Win Ratio

![m](/Resources/RandScissorWinRatio.PNG)

As the score to win increases, random strategy becomes less viable. This is because during the early stages of the game, no player has the resources to utilize cards and upgrades yet, thus the early-game of Super Rock Paper Scissors resembles the original Rock Paper Scissors where a random strategy is viable.

#### Competitiveness

![m](/Resources/RandScissorCompetitiveness.PNG)

The competitiveness score follows a similar trend where ScissorLover's performance increases and Random's performance decreases as score to win increases. However, what is interesting is seing the competitiveness score increase while the winning strategy is switching from Random to ScissorLover. The competitiveness is highest when score to win is set to 5. 

#### Observations and Conclusion

This experiment not only proves that the game potentially consists of interesting competitive decisions, but also gives insight to what may be an optimal value for score to win where random strategy is deemed unviable.

### More AI and Using Python in Unity to Enable Creating Machine Learning Models

**I am currently working on getting these AI ready for data collection:** 

* **Greedy AI:** The previous implementation of greedy AI had many setbacks which needed fixing

* **Mid Tracker AI:** An AI that tracks the opponents last N played weapons and predicts what the opponent is going to play this round and acts accordingly

* **Random AI with Card or Upgrade focus:** These AI were created out of neccessity to measure the usefulness of cards and upgrades in a vacuum. 

* **Base MLAI and its implementations:** MLAI stands for Machine Learning AI which every now and then trains a model on their opponent's previous picks and tries to predict their next weapon

**Base MLAI**

![m](/Resources/BaseMLAI.PNG)

The implementation for base MLAI requires the use of Python as Python has much better support for machine learning endeavors. I had to look into how Python code can be run through Csharp and Unity. I found couple of different approaches including a client-server approach which seemed super neat but lacked the comprehensive documentation I needed. I decided to go with a simple solution which uses the Python interpreter in the current execution environment.

**The code for calling Python code:**

![m](/Resources/PythonExecutor.PNG)


### Links

**Github repo for Super Rock Paper Scissors: [Click Here](https://github.com/bahaokten/Research_RPC)**

**Webbuild: [Click Here]({{ site.baseurl }}{% link Builds/Week5/index.html %})**

### Citations & Resources

1) https://github.com/gilcoder/AI4U/tree/master/doc

2) https://docs.unity3d.com/Packages/com.unity.scripting.python@2.0/manual/index.html

3) https://pythonpedia.com/en/tutorial/10759/call-python-from-csharp

4) https://www.softwarepronto.com/2016/09/calling-python-from-netc.html

5) https://support.microsoft.com/en-us/office/use-autosum-to-sum-numbers-543941e7-e783-44ef-8317-7d1bb85fe706#:~:text=If%20you%20need%20to%20sum,Here's%20an%20example.

6) https://www.excel-easy.com/examples/average.html

7) https://exceljet.net/formula/sum-if-cells-contain-specific-text

8) https://smallbusiness.chron.com/add-cells-across-multiple-spreadsheets-44275.html

**Current Project Code Structure Of Super Rock Paper Scissors**

![m](/Resources/CodeAmount4.PNG)

**Weekly Time Spent on Project SeeSaw**

| Day  | Hours Spent |
|:-:|:-:|
| Thu | 0| 
| Fri | 0| 
| Sat | 0| 
| Sun | 4:30| 
| Mon | 5:15| 
| Tue | 4:15| 
| Wed | 0|
|TOTAL | 14:00| 