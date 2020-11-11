---
layout: post
title:  "Machine Learning and Research"
date:   2020-11-11 01:30:37 -0400
categories: ML
---

### Machine Learning For AI Players Research

This week, I spent most my time researching about machine learning techniques, and writing the infrastructure for testing couple of these machine learning tools. 

#### **Unsupervised vs Supervised**

**Research**

[*Unsupervised vs Supervised ML*](https://towardsdatascience.com/supervised-vs-unsupervised-learning-14f68e32ea8d)

[*Big Data Bioinformatics*](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5219000/#:~:text=Supervised%20Versus%20Unsupervised%20Machine%20Learning,-Machine%20learning%20algorithms&text=A%20supervised%20learning%20algorithm%20analyzes,unlabeled%20examples%20without%20predefined%20classes.)

**Conclusion**

First question to tackle was what kind of machine learning would best fit a Super Rock Paper Scissors AI player.

Since the AI players will be able to accumulate information about their opponent's previous actions, the supervised approach seems more fitting.

#### **Classification vs Regression**

**Research**

[*Classification vs Regression ML*](https://machinelearningmastery.com/classification-versus-regression-in-machine-learning/)

**Conclusion**

We want the machine learning model to predict what the opponent is gonna do this round. The opponent's actions can be represented as a finite sized set of classifications. For example, the opponent can upgrade Scissor, play Defense Increase Card, and pick Scissor which can be represented by the classification (US,DIC,PS). 

In conclusion, it seems like the Supervised Classification Learning fits the bill the best.

#### **Feature Space**

**Research**

First I started looking into Convolutional Neural Networks

[*Predicting Professional Players' Chess Moves with Deep Learning*](https://towardsdatascience.com/predicting-professional-players-chess-moves-with-deep-learning-9de6e305109e)

[*Predicting Moves in Chess using Convolutional Neural Networks*](https://www.semanticscholar.org/paper/Predicting-Moves-in-Chess-using-Convolutional-Oshri/28a9fff7208256de548c273e96487d750137c31d?p2df)

Then I realized, the snapshots of my game state would depend on each other due to the importance of ordering and timing of the game state snapshots.
I then started looking into Recurrent Neural Nets

[*Convolutional Neural Network for time-dependent features*](https://stackoverflow.com/questions/31541251/convolutional-neural-network-for-time-dependent-features)

[*Recurrent Convolutional Neural Networks for Scene Labeling*](http://proceedings.mlr.press/v32/pinheiro14.pdf)

[*Attention, please: forget about Recurrent Neural Networks*](https://medium.com/swlh/attention-please-forget-about-recurrent-neural-networks-8d8c9047e117)

[*Deep Learning, CNN - RNN - Pytorch*](http://www2.ece.rochester.edu/~zduan/teaching/ece477/lectures/Christos%20-%20CNN-LSTM-PyTorch.pdf)

**Conclusion**

The dimensionality of the feature space and its time-dependence is a more intricate question than I anticipated. It seems like using an RNN model will be the best way to move forward, but before I pick a machine learning model I want to discuss it with a Machine Learning Professor.

A potential CNN solution I've came up with follows the model below.

```cs
# Insired from University of Michigan EECS 445 Project 2
class CNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = torch.nn.Conv2d(3,16,(5,5), stride=(2,2), padding=self.get_same_padding(5))
        self.conv2 = torch.nn.Conv2d(16,64,(5,5), stride=(2,2), padding=self.get_same_padding(5))
        self.conv3 = torch.nn.Conv2d(64,32,(5,5), stride=(2,2), padding=self.get_same_padding(5))
        self.fc1 = torch.nn.Linear(512,64)
        self.fc2 = torch.nn.Linear(64,32)
        self.fc3 = torch.nn.Linear(32,3)
        self.init_weights()

    def get_same_padding(self, filter_dim):
        return ( int((filter_dim - 1) / 2), int(( filter_dim - 1) / 2))

    def init_weights(self):
        for conv in [self.conv1, self.conv2, self.conv3]:
            C_in = conv.weight.size(1)
            nn.init.normal_(conv.weight, 0.0, 1 / sqrt(5*5*C_in))
            nn.init.constant_(conv.bias, 0.0)
        for fc in [self.fc1, self.fc2, self.fc3]:
            C_in = fc.weight.size(1)
            nn.init.normal_(fc.weight, 0.0, 1 / sqrt(C_in))
            nn.init.constant_(fc.bias, 0.0)

    def forward(self, x):
        N, C, H, W = x.shape
        print(x.shape)
        x1 = F.relu(self.conv1(x))
        x2 = F.relu(self.conv2(x1))
        x3 = F.relu(self.conv3(x2))
        x3 = torch.flatten(x3, 1)
        x4 = F.relu(self.fc1(x3))
        x5 = F.relu(self.fc2(x4))
        x6 = self.fc3(x5)

        return x6
```

### Potential Solutions to Fixing Indefinite Stalemates

**Why is Indefinite Stalemates a serious issue**

As I envisions Super Rock Paper Scissors to be a proper competitive game that can be played in a tournament structure, the game length is supposed to be reasonably predictable. It should not be possible to extend one game indefinitely, as that would make the tournament structure vulnerable to delaying schemes. In the case of unintentional stalemate chains, the game should have a predictable length to allow players to predict and plan a reasonable timeframe for playing Super Rock Paper Scissors. 

To show how long stalemates can exten the gamelength, I made Simple Scissor Lover AI which picks Scissor 80% of the time and the other weapons 20% of the time play against itself.

![m](/Resources/ScissorScissorCompetitiveness.PNG)

The image above shows the competitiveness score across the board are around double the competitiveness score of a regular game of Super Rock Paper Scissors (1.3 to 1.8). The graph shows, as the game gets longer, stalemates become more unlikely as the players gain resources to upgrade and buy cards which essentially break the ties when both players pick the same weapon. When the game is long enough; however, (Score To Win set to 100) we can see that once max upgrades is reached, the stalemate scenarios start to become likely again.

Fixing indefinite stalemates was a harder issue than I thought it would be. This is mainly because I did not like the idea of allowing games that end in a stalemate, since I think games with definite winners are more flexible in a competitive environment.

#### Fatigue System

Fatigue system tries to tackle the indefinite stalemates issue by punishing players for sticking to playing the same weapon or weapon pattern over and over again so that the players are demotivated to be predictable. It does this by debuffing stats of each weapon depending on how often they were used in the past X rounds. The premise that the players would be less likely to be caught in stalemate loops may be true but the fatigue system does not entirely eliminate the possibility of having unintentional stalemate chains, let alone intenional stalemates. One other problem with the fatigue system is that I think it is an intricate and tedious new mechanic only clutters the gameplay with uneccesary micromanagement.

#### Max Rounds

One idea is to limit max possible rounds that can be played in a single game. While limiting maximum possible rounds fix the issue of indefinite stalemate, it introduced couple new issues. First of all, it does not provide a solution to deciding a winner when the max possible rounds is reached. Second of all, enabling games to be concluded before at least ```Score To Win``` many rounds have been played makes it difficult to predict and control how much money and resources will be available to both players in an average game of Super Rock Paper Scissors. 

#### Max Rounds With Sudden Death

One improvement to the Max Rounds idea to implement a sudden death scenario in case of a stalemate. One idea for the sudden death scenario is to make both players play a best of 3 game of traditional Rock Paper Scissors (no upgrades and cards) against random AI players. Whoever player reaches First player to win 2 rounds against the random AI win the game. 

While Max Rounds With Sudden Death fixes the potential of having indefinite stalemates, it still does make balancing and predicting the amount of money and resources in circulation harder from a game balance standpoint. Max Rounds With the suggested sudden deatch scenario also introduces randomness as a deciding factor of the winner of a stalemate game.

#### Time Limit (With or Without Sudden Death)

Time limit is similar to Max Rounds as they share the same flaws and benefits. One characteristic of time limit that stands out over max rounds however is it makes game length more predictable which may allow for a better competitive tournament environment. Time limit; however, can be abused by players by taking as long as possible while winning. This can be partially mitigated by implementing a time limit for each round separate from the game time limit. 

#### Tie Breaker Trigger

The Tie Breaker Trigger prevents indefinite stalemate chains by detecing when a stalemate chain of X rounds occurs, and applying a tie breaking scenario to enforce a round winner on the third consecutive stalemate round. This limits max game length of a single game to 

```MAXGAMELENGTH = (ScoreToWin + (ScoreToWin - 1)) * ((MaxStalemateChainLength - 1) + 1)```

The tie breaker scenario could look compare total stats and/or coins of players to determine a winner. While this shrinks the possibility of having a truly random round to nearly zero, it does not completely eliminate the possibility of having indefinite stalemates. Since this scenario is rare enough, the round winner can be decided at random when compared stats of both players are equal. The introduction of randomness here is much better than the random nature of the Max Rounds approach with the example Sudden Death Scenario I talked about above. 

**Current Project Code Structure Of Super Rock Paper Scissors**

![m](/Resources/CodeAmount5.PNG)

**Weekly Time Spent on Project SeeSaw**

| Day  | Hours Spent |
|:-:|:-:|
| Thu | 0:00| 
| Fri | 0:00| 
| Sat | 2:00| 
| Sun | 0:00| 
| Mon | 4:00| 
| Tue | 4:00| 
| Wed | 2:00|
|TOTAL | 12:00| 