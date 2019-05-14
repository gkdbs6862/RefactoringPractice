# 리팩토링 연습 02
### 2019/05/14


---

> 다음 코드를 리팩토링 해보자

```C#
public void SetSliderExpBar(UISlider slider, UILabel lblGaugeText, int Value)
    {
        // LevelExpDataTbl = 경험치 테이블, 경험치를 기준으로 Level을 계산
        List<LevelExpData> levelExpTblList = TableManager.Instance.GetTbl<LevelExpDataTbl>().GetDataList();
        
        float nowLvReqExp = 0;
        float nextLvReqExp = 0;
        int level = 1;
        for (int i = 0; i < levelExpTblList.Count; ++i)
        {
            if (expValue >= levelExpTblList[i].reqexp)
            {
                level = levelExpTblList[i].level;
                nowLvReqExp = levelExpTblList[i].reqexp;
            }
            else
            {
                nextLvReqExp = levelExpTblList[i].reqexp;
                break;
            }
        }
        if (level == Definition.LEVEL_MAX)
        {
            slider.value = 1;
        }
        else
        {
            float expValue = (Value - nowLvReqExp) / (nextLvReqExp - nowLvReqExp);
            slider.value = expValue;
        }
        lblGaugeText.text = string.Format("{0:F1}%", slider.value * 100f);
    }
    ```
    
    ---
    
    ## 아래부터는 리팩토링 예제입니다.
    
    ---
    
    > 내가 리팩토링 해본 코드
    
    ```C#
    
public void SetSliderExpBar(UISlider slider, UILabel lblGaugeText, int Value)
    {
        // LevelExpDataTbl = 경험치 테이블, 경험치를 기준으로 Level을 계산
        List<LevelExpData> levelExpTblList = TableManager.Instance.GetTbl<LevelExpDataTbl>().GetDataList();

        float nowLvReqExp = 0;
        float nextLvReqExp = 0;
        int level = 1;
                
        if (level == Definition.LEVEL_MAX)
        {
            slider.value = 1;
        }
        else
        {
            for (int i = 0; i < levelExpTblList.Count; ++i)
            {
                if (expValue >= levelExpTblList[i].reqexp)
                {
                    level = levelExpTblList[i].level;
                    nowLvReqExp = levelExpTblList[i].reqexp;
                }
                else
                {
                    nextLvReqExp = levelExpTblList[i].reqexp;
                    slider.value = (Value - nowLvReqExp) / (nextLvReqExp - nowLvReqExp);
                    break;
                }
            }
        }
        lblGaugeText.text = string.Format("{0:F1}%", slider.value * 100f);
    }
    ```
    
   for문의 동작이 매번 이루어지기 보다 업데이트 해야 할 상황에만 동작하도록 해 보려고 하였다.
   하지만 위의 코드는 level을 먼저 계산 후 if문이 진행되어야 하므로 제대로 동작하지 않는다는 것을 알 수 있었다.
   
   ---
   
   > 올바른 리팩토링 예제
   
   ```C#
   public void SetSliderExpBar(UISlider slider, UILabel lblGaugeText, int value)
    {
        // LevelExpDataTbl = 경험치 테이블, 경험치를 기준으로 Level을 계산
        List<LevelExpData> levelExpTblList = TableManager.Instance.GetTbl<LevelExpDataTbl>().GetDataList();
        
        float nowLvReqExp = 0;
        float nextLvReqExp = 0;
        int level = 1;
        for (int i = 0; i < levelExpTblList.Count; ++i)
        {
            if (value < levelExpTblList[i].reqexp)
            {
                nextLvReqExp = levelExpTblList[i].reqexp;
                break;
            }
            level = levelExpTblList[i].level;
            nowLvReqExp = levelExpTblList[i].reqexp;
        }
        float expValue = (value - nowLvReqExp) / (nextLvReqExp - nowLvReqExp);
        slider.value = (level == Definition.LEVEL_MAX) ? 1 : expValue;
        lblGaugeText.text = string.Format("{0:F1}%", slider.value * 100f);
    }
    ```
    
    두가지 측면에서 리팩토링 한 예제이다.
    
    첫번째는 for문 안에서의 if else 구문이다.
    else에 break가 들어가는 조건문을 뒤집어 if만 사용해도 되도록 수정되었고
    
    두번째는 slider.value를 삼항연산자를 이용해 slider.value를 계산할 때 사용된 if문이 제거되었다.
    
    ---
    
    퍼포먼스만을 신경쓰고 제대로 동작하지 않는 코드를 만든 오늘의 연습을 반성하고 더 많이 연습해야겠다.
    if문을 뒤집음으로써 else 구문을 제거 할 수 있
    삼항연산자를 통해 if문을 제거하여 깔끔하게 리팩토링 하는 방법을 알게 되었다.
