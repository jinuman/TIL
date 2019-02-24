# Python
> _"Life is too short, you need python"_

## 파이썬으로 할 수 있는 일

1. 데이터 분석 (ex. Pandas)
2. 데이터베이스 프로그래밍
3. 웹 프로그래밍 (ex. Django)
4. C / C++ 과 결합
5. GUI 프로그래밍
6. 시스템 유틸리티 제작 

## 파이썬의 특징

- 0~100 같은 int형 숫자들은 많이 쓰이기 때문에 메모리에 미리 잡아두고 쓰이기 때문에 메모리 주소가 같다.

```Python
a = 5
b = 5
print(id(a) == id(b))   # True
# 레퍼런스 비교 연산자 is
print(a is b)   # True
```

- Python 3 에서는 큰 정수도 type int로 취급한다.

## print

```Python
print("a", "b")            # "a b"
print("a", "b", sep="-")   # "a-b"
# 한줄에 이어서 프린트하기
print("a", "b", sep="-", end="")
print("Hello")             # "a-bHello"
```

## String

- str[start:stop:step]
```Python
str = "Hello World"
a = str[:5]
print(a)    # "Hello"
b = str[6:]
print(b)    # "World"
# 거꾸로 출력하기
c = str[::-1]
print(c)    # "dlroW olleH" 
```

## Boolean

```Python
date = "5 Mar 2018"
check = "Mar" in date
print(check)    # True
```

## Random

- import random

```Python
import random

a = random.randrange(0, 10)
print(a) # 0~9 중 random 값 출력
b = random.randrange(0, 10, 2)
print(b) # 0~9 중 짝수만 출력
c = random.randint(0, 10)
print(c) # 0~10 중 random 값 출력
```


