# 리팩토링 연습 01

---

> 다음 코드를 리팩토링 해보자.

```C#
void Refactoring()
{
    if (playerTeam.GetTeamHPRate() > enemyTeam.GetTeamHPRate())
    {
        BattleSequenceInfo.m_isPlayerWin = true;
    }
    else if (playerTeam.GetTeamHPRate() < enemyTeam.GetTeamHPRate())
    {
        BattleSequenceInfo.m_isPlayerWin = false;
    }
    else
    {
        if (playerTeam.ActorList.Length >= enemyTeam.ActorList.Length)
        {
            BattleSequenceInfo.m_isPlayerWin = true;
        }
        else
        {
            BattleSequenceInfo.m_isPlayerWin = false;
        }
    }
}
```

---

> 내가 리팩토링 해본 코드

```C#
void Refactoring()
    {
        bool isPlayerTeamBigger = playerTeam.ActorList.Length >= enemyTeam.ActorList.Length;
        
        if (playerTeam.GetTeamHPRate() > enemyTeam.GetTeamHPRate())
        {
            BattleSequenceInfo.m_isPlayerWin = true;
        }
        else if (playerTeam.GetTeamHPRate() < enemyTeam.GetTeamHPRate())
        {
            BattleSequenceInfo.m_isPlayerWin = false;
        }
        else
        {
            BattleSequenceInfo.m_isPlayerWin = isPlayerTeamBigger;
        }
    }
```

각 팀의 HPRate가 같을 때 팀의 크기를 비교하기 때문에 `isPlqyerTeamBigger` 변수를 만들어
`BattleSequenceInfo.m_isPlayerWin`에 다이렉트로 넣어 주었다.

---

> 올바른 리펙토링 예제 01

```C#
void Refactoring()
{
    if (playerTeam.GetTeamHPRate() == enemyTeam.GetTeamHPRate()) 
    {
        BattleSequenceInfo.m_isPlayerWin = (playerTeam.ActorList.Length >= enemyTeam.ActorList.Length);
    } 
    else 
    {
        BattleSequenceInfo.m_isPlayerWin = (playerTeam.GetTeamHPRate() > enemyTeam.GetTeamHPRate());
    }
}
```

각 팀의 HPRate가 같을 때와 아닐때만 구분하여 `BattleSequenceInfo.m_isPlayerWin` 에 각 팀의 크기와 HPRate를 비교 한 값을 다이렉트로 넣어 주는 것을 알 수 있다.
