---
layout: post
title:  "AI Players"
date:   2020-10-14 10:30:37 -0400
categories: AI
---


### AI Players For Balancing

Started designing AI that would play the game in unique ways. There are currently 4 available CPU players: **Simple Scissor Lover, Greedy Attacker, Greedy Defender, and Greedy Mixed**

I plan to make these AI play against each other while a logger script keeps track of game length, popular weapons, popular upgrades, popular cards, number of stalemates, 
win percentage per strategy, card and upgrade usefulness (does buying cards or upgrading weapons increase your chances of winning), defense vs attack based strategy win ratios and potentially more datapoints.

#### Base AI

All AI scripts extend the base AI class which has virtual functions for each phase (Buy, Action, Attack).

```cs
public abstract class BaseAI : MonoBehaviour
{
    protected Player player;
    protected PlayerController pc;
    protected float waitDurBetweenPhases = 0;

    protected static System.Random randObj = new System.Random();

    public bool active = false;

    public Subscription<CurrentPlayerChanged> CurrentPlayerChangedSubscription;
    public Subscription<PlayerWonRound> PlayerWonRoundSubscription;

    void Awake()
    {
        CurrentPlayerChangedSubscription = _EventBus.Subscribe<CurrentPlayerChanged>(_OnCurrentPlayerChanged);
        PlayerWonRoundSubscription = _EventBus.Subscribe<PlayerWonRound>(_OnPlayerWonRound);
    }

    public static Type GetAIType(PlayerType type)
    {
        switch (type)
        {
            case PlayerType.Simp_ScissorLover:
                return typeof(Simp_ScissorLover);
            case PlayerType.Mid_GreedyAttacker:
                return typeof(Mid_GreedyAttacker);
            case PlayerType.Mid_GreedyDefender:
                return typeof(Mid_GreedyDefender);
            case PlayerType.Mid_GreedyMixed:
                return typeof(Mid_GreedyMixed);
        }
        //Human
        return null;
    }

    public void InitializeBase(Player _player)
    {
        pc = GameController.instance.GetPlayer(_player);
        player = _player;
        active = true;
        Initialize();
    }

    void _OnCurrentPlayerChanged(CurrentPlayerChanged e)
    {
        if (!active)
        {
            return;
        }

        if (e.newCurr == player)
        {
            StartCoroutine(DoTurn());
        }
    }

    void _OnPlayerWonRound(PlayerWonRound e)
    {
        if (!active)
        {
            return;
        }

        if (e.player_t == player)
        {
            PostAttackPhase(true);
        }
        else
        {
            PostAttackPhase(false);
        }
    }

    protected IEnumerator DoTurn()
    {
        yield return 0;

        //=== BUY PHASE ===
        BuyPhase();
        if (waitDurBetweenPhases != 0)
        {
            yield return new WaitForSeconds(waitDurBetweenPhases);
        }

        //=== ACTION PHASE ===
        ActionPhase();
        if (waitDurBetweenPhases != 0)
        {
            yield return new WaitForSeconds(waitDurBetweenPhases);
        }

        //=== ATTACK PHASE ===
        AttackPhase();
        if (waitDurBetweenPhases != 0)
        {
            yield return new WaitForSeconds(waitDurBetweenPhases);
        }

        yield break;
    }

    protected virtual void Initialize()
    {
        return;
    }

    protected virtual void BuyPhase()
    {
        _EventBus.Publish<EndTurnPhase>(new EndTurnPhase(pc));
    }

    protected virtual void ActionPhase()
    {
        _EventBus.Publish<EndTurnPhase>(new EndTurnPhase(pc));
    }

    protected virtual void AttackPhase()
    {
        _EventBus.Publish<EndTurnPhase>(new EndTurnPhase(pc));
    }

    protected virtual void PostAttackPhase(bool isWinner)
    {
        return;
    }
}
```

#### Simple Scissor Lover

This AI only upgrades its scissor weapon and has an 80% chance of playing scissor in the attack phase.

```cs
public class Simp_ScissorLover : BaseAI
{
    protected override void Initialize()
    {
    }

    protected override void BuyPhase()
    {
        //Buys Nothing
        _EventBus.Publish<EndTurnPhase>(new EndTurnPhase(pc));
    }

    protected override void ActionPhase()
    {
        //If possible, always upgrades scissor's attack
        if (pc.CanUpgradeWeapon(WeaponType.Scissor, WeaponAttribute.Attack))
        {
            _EventBus.Publish<WeaponUpgraded>(new WeaponUpgraded(pc, WeaponType.Scissor, WeaponAttribute.Attack));
        }
    }

    protected override void AttackPhase()
    {
        //Has a 10% change of choosing rock and 10% chance of choosing paper, otherwise chooses scissor 
        int rand = randObj.Next(0, 100);
        if (rand < 10)
        {
            _EventBus.Publish<AttackWeaponPicked>(new AttackWeaponPicked(pc, WeaponType.Paper));
        }
        else if (10 <= rand && rand < 20)
        {
            _EventBus.Publish<AttackWeaponPicked>(new AttackWeaponPicked(pc, WeaponType.Rock));
        }
        else
        {
            _EventBus.Publish<AttackWeaponPicked>(new AttackWeaponPicked(pc, WeaponType.Scissor));
        }
    }

    protected override void PostAttackPhase(bool isWinner)
    {
    }
}
```

#### Base Greedy AI

All greedy AI extend the base greedy AI (Mid_GreedyAttacker, Mid_GreedyDefender, Mid_GreedyMixed)

```cs
public abstract class BaseMid_Greedy : BaseAI
{
    protected PlayerController opponentPc;
    protected bool canBuyUpgrade = false;
    protected WeaponType upgradableWeaponType;

    protected bool cardUsed = false;

    protected WeaponAttribute attrOfInterest;
    protected CardType cardOfInterest;

    protected virtual void SetInterest()
    {

    }

    protected override void Initialize()
    {
        opponentPc = GameController.instance.GetOpponentPlayer(pc);
        SetInterest();
    }

    protected override void BuyPhase()
    {
        float bestUpgradePrice = 10000;
        print(canBuyUpgrade);
        foreach (WeaponType type in Enum.GetValues(typeof(WeaponType)))
        {
            if (pc.CanUpgradeWeapon(type, attrOfInterest))
            {
                float upgradePrice = pc.weapons[type].GetUpgradePrice(attrOfInterest);
                if (upgradePrice < bestUpgradePrice)
                {
                    bestUpgradePrice = upgradePrice;
                    canBuyUpgrade = true;
                    upgradableWeaponType = type;
                }
            }
        }
        if (!canBuyUpgrade && pc.coins > 4)
        {
            _EventBus.Publish<CardPurchased>(new CardPurchased(pc, cardOfInterest));
        }
    }

    protected override void ActionPhase()
    {
        if (canBuyUpgrade)
        {
            _EventBus.Publish<WeaponUpgraded>(new WeaponUpgraded(pc, upgradableWeaponType, attrOfInterest));
        }
        else
        {
            if (pc.cards.Count != 0)
            {
                _EventBus.Publish<CardUsed>(new CardUsed(pc, cardOfInterest));
                cardUsed = true;
            }
        }
    }

    protected override void AttackPhase()
    {
        float highestDamage = -1000;
        float cardDamageAdd = 0;
        WeaponType attackType = WeaponType.Scissor;

        if (cardUsed)
        {
            cardDamageAdd = 0.5f;
        }

        foreach (WeaponType type in Enum.GetValues(typeof(WeaponType)))
        {
            float currDamage = pc.GetWeapon(type).baseAttack + cardDamageAdd - opponentPc.GetWeapon(pc.GetWeapon(type).GetStrongType()).baseDefense;
            if (currDamage > highestDamage)
            {
                highestDamage = currDamage;
                attackType = type;
            }
        }
        _EventBus.Publish<AttackWeaponPicked>(new AttackWeaponPicked(pc, attackType));
    }

    protected override void PostAttackPhase(bool isWinner)
    {
        canBuyUpgrade = false;
        cardUsed = false;
    }
}
```

**Greedy Attacker (Extends BaseGreedy)**
```cs
public class Mid_GreedyAttacker : BaseMid_Greedy
{
    protected override void SetInterest()
    {
        attrOfInterest = WeaponAttribute.Attack;
        cardOfInterest = CardType.SelfAttackIncreaseAdditiveCurrent1;
    }
}
```

**Greedy Mixed (Extends BaseGreedy)**

```cs
public class Mid_GreedyMixed : BaseMid_Greedy
{
    protected override void SetInterest()
    {
        return;
    }

    protected override void BuyPhase()
    {
        RandomizeInterest();
        base.BuyPhase();
    }

    private void RandomizeInterest()
    {
        int type = randObj.Next(0, 2);
        if (type == 0)
        {
            attrOfInterest = WeaponAttribute.Defense;
            cardOfInterest = CardType.SelfDefenseIncreaseAdditiveCurrent1;
        } else
        {
            attrOfInterest = WeaponAttribute.Attack;
            cardOfInterest = CardType.SelfAttackIncreaseAdditiveCurrent1;
        }
    }
}
```
### More Changes

Super Rock Paper Scissors now uses the Pub/Sub system to allow more flexibility for logging, probing, and AI scripting.

Cards can now be bought and used in UI view.

Added a menu scene where player types can be changed and animations can be turned off.

Webbuild is now available (Check the Links section below).

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