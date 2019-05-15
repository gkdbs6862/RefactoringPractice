# 리팩토링 연습 03

### 2019/05/15

---

## 다음 코드를 리팩토링 해보자

---

>> 문제 1

```C#
bool IsBananas()
    {
        if ( isHungry() )
        {
            if ( bananas_are_ripe() )
            {
                return true;
            }
            else
            {
                return false;
            }
        }
        else
        {
            return false;
        }
    }    
```

---

>> 문제 2

```C#
    void Test()
    {
        bool a;
        int b;
        a = fn1();
        b = fn2();
        if (a)
            foo(10, b);
        else
            foo(5, b);
    }
```

---


## 내가 리팩토링 해본 코드

---

>> 문제 1

```C#
    bool IsBananas()
    {
        if (isHungry() && bananas_are_ripe())
        {
            return true;
        }
        else
        {
            return false;
        }
    }
```

if문 두개의 조건을 합하여 하나의 if문으로 수정 해 보았다.

---

>> 문제 2

```C#
    void Test()
    {
        bool a;
        int b;
        a = fn1();
        b = fn2();

        int fooValue = a ? 10 : 5;
        foo(fooValue, b);
    }
```

삼항연산자를 이용해 if문을 제거하는 방향으로 수정 해 보았다.

---

## 올바른 리팩토링 예제

---

>> 문제 1

```C#
    bool IsBananas()
    {
        return (isHungry() && bananas_are_ripe());
    }
```

계산한 값을 다이렉트로 리턴함으로써  
if문을 없앤 방향으로 수정되었다.

---

>> 문제 2_01

```C#
    void Test()
    {
        int fooValue = fn1() ? 10 : 5;
        foo(fooValue, fn2() );
    }
```

불필요한 변수를 제거하고 삼항연산자를 이용해 if문이 제거되었다.

---

>> 문제 2_02

```C#
    void Test()
    {
        foo(fn1() ? 10 : 5, fn2() );
    }
```

불필요한 변수를 제거하고  
삼항연산자를 이용한 계산식을 foo()에 다이렉트로 넣은 방향으로 수정된 예제이다.


---

불필요한 변수와 if문을 얼마나 더 많이 줄일 수 있는지 항상 생각해야 함을 깨닳은 연습이었다.
리팩토링을 할 때 항상 더 간결하게 수정할 수 있는지 확인해야겠다.
