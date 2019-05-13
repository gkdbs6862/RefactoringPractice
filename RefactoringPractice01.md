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

## 아래부터는 리펙토링 입니다.

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

각 팀의 HPRate가 같을 때 팀의 크기를 비교하기 때문에 `isPlayerTeamBigger` 변수를 만들어
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

---

> 올바른 리펙토링 예제 02

```C#
void Refactoring()
{
    bool isDraw = (playerTeam.GetTeamHPRate() == enemyTeam.GetTeamHPRate());
    if (isDraw) 
    {
        bool isPlayerTeamBigger = (playerTeam.ActorList.Length >= enemyTeam.ActorList.Length);
        BattleSequenceInfo.m_isPlayerWin = isPlayerTeamBigger;
    } 
    else 
    {
        bool hpBigger = playerTeam.GetTeamHPRate() > enemyTeam.GetTeamHPRate();
        BattleSequenceInfo.m_isPlayerWin = HPbigger;
    }
}
```

`isDraw` bool 변수를 만들어 각 팀의 HPRate가 같은지 비교하고
같다면 `isPlaterTeamBigger` bool 변수를 통해 각 팀의 크기를 비교한 값을 `BattleSequenceInfo.m_isPlayerWin` 에 넣어주고
다르다면 `hpBigger` bool 변수를 통해 각 팀의 HPRate를 비교한 값을 `BattleSequenceInfo.m_isPlayerWin` 에 넣어주는 것을 알 수 있다.

---

올바른 리펙토링 예제 2개를 통해 리팩토링 방법에 대해 여러가지 방법이 있음을 알게 되었다.
1번 리펙토링 예제가 더 깔끔하게 보이지만 2번 리펙토링 예제가 더 읽기 수월하였다.

이번 리펙토링 문제의 요점은 true/false 여부를 입력하는 용도로 if/else 를 사용하는 것에 대한 refactoring 이었다.
if문으로 true / false 만 확인하는 거라면 Direct로 넣어주는 것도 깔하다는 것을 알게 되었다.
