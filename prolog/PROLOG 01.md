## PROLOG 01

[문제]

2~10 명 까지 앉을 수 있는 테이블이 있다.

총 인원수가 100 명 이고 테이블의 개수의 제한은 없다.

테이블에 나누어 앉는 패턴의 수는?

<br>

ex) _6 명 일 경우_ :  4 가지

- 2 + 2 + 2
- 2 + 4
- 3 + 3
- 6

---

## 재귀적(Recursive) 방법

재귀적 방법의 핵심은 반복되는 현상의 패턴과 탈출 조건을 정의하는 것이라고 생각한다.

이 문제의 경우, 총 인원수가 100명 이므로, 첫 번째 테이블에 2명을 앉히면 남은 인원은 98 이고 2 ~ 10 의 범위 내에서 앉힐 사람을 정한다. 이를 트리로 표현하면

![image](https://user-images.githubusercontent.com/79683414/140048629-8bdedd01-7c35-4147-803c-f4d35de679a3.png)

<br><br>

여기서 반복되는 패턴은

"남은 인원 수를 고려하여 2~10의 숫자를 정하는 행위" 이다. 세분화하면

1) 남은 인원 수 계산
2) 2 ~ 10 사이의 수 `i` 선택
3) 남은 인원수 - `i`

<br>

반복되는 것이 "남은 인원 수", "선택하는 수" 이다.

Python으로 전체적인 틀을 우선 잡아보면 아래와 같다. 

```python
M = 100

def decision(remain, select):
    # remain = 남은 인원 수
    # select = 선택하는 인원의 수
    decision(remain-i, i)
    
decision(M, 2)
```

<br>

반복되는 패턴을 구현했으니 탈출 조건을 설정하면 된다. i = 2 라고 가정 해보자.

테이블에 모든 사람을 앉히면 되니 remain = 0 인 경우 `return`하면 될 것 같다.

또, i 에 2 ~ 10의 수를 대입해야 하니 반복문을 사용하자.

<br>

```python
M = 100

def decision(remain, select):
    if remain == 0:
        return 
    for i in range(select, 11):
    	decision(remain-i, i)
    
decision(M, 2)
```

<br><br>

반복 패턴과 탈출 조건이 어느 정도 맞춰졌다. 어떤 값을 return 할 지는

remain = 0 인 경우를 생각해보면 된다. remain 이 0 인 경우 패턴을 1개 찾은 것이다.

따라서 아래와 같게 된다.

```python
M = 100

def decision(remain, select):
    if remain == 0:
        return 1
    cnt = 0
    for i in range(select, 11):
    	cnt += decision(remain-i, i)
    
decision(M, 2)
```

<br><br>

지금 까지의 로직을 검산해보자. 트리로 생각하면 깊이 우선 탐색 이다. 깊이 우선 탐색에서 최종 노드(leaf node)를 카운트 한다고 보면 된다.

decision 함수가 계속 실행되면서 100 - 2 패턴이 반복되고 remain 이 2 일 때, 반복문을 생각해보자.

remain 은 2 인데 반복문의 범위는 2 ~ 10 이다.

즉, decision() 의 첫 번째 매개변수로 음수가 전달되는 경우가 발생한다.

이런 경우 누적 연산에서 제외해야 한다.

<br>

또, 최종적으로 결과 값을 반환해야 한다. 완성된 형태는 아래와 같다.

```python
M = 100

def decision(remain, select):
    if remain < 0:
        return 0
    if remain == 0:
        return 1
    cnt = 0
    for i in range(select, 11):
    	cnt += decision(remain-i, i)
    return cnt


print(decision(M, 2))
```

<br>

재귀적인 방법을 요약하자면, 트리로 생각했을 때 최종 노드까지 가는 로직을 먼저 생각 하고 탈출 조건, 반환 형식을 생각하면 된다.

