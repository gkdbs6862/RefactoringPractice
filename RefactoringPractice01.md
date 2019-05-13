# 리팩토링 연습 01

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


> 내가 리팩토링 해본 코드

<pre> <code>
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
</code> </pre>
