---
title: "DFS/BFS"
layout: post
date: 2021-03-14 00:00
tag: 
- codingTest

headerImage: true
codingTest: true
hidden: true # don't count this post in blog pagination
category: codingTest
author: Hyunsoo
externalLink: false
---

### 스택(Stack)

- **append()**: 리스트의 가장 뒤쪽에 데이터를 삽입
- **pop()**: 리스트의 가장 뒤쪽에서 데이터를 꺼내

{% highlight python %}
stack = []


stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()
stack.append(1)
stack.append(4)
stack.pop()

print(stack)        # 최하단 원소부터 출력
print(stack[::-1])  # 최상단 원소부터 출력

# [5, 2, 3, 1]
# [1, 3, 2, 5]
{% endhighlight %}
<figcaption class="caption">Stack Implementaion</figcaption>
<br>


### 큐(Queue)
- **collections**모듈에서 제공하는 **deque**자료구조를 활용해라
    - **append()**
    - **popend()**
- **queue**라이브러리를 이용하는 것보다 더 간단하다
- **deque**객체를 리스트 자료형으로 변경하고싶으면 **list()**메서드를 이용해
    - list(queue) 이렇게
{% highlight python %}
from collections import deque

queue = deque()

queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popend()
queue.append(1)
queue.append(4)
queue.popend()

print(queue)
queue.reverse()
print(queue)


{% endhighlight %}
<figcaption class="caption">Queue Implementaion</figcaption>
<br>


### 재귀함수
- 자기 자신을 다시 호출하는 함수
- 보통 파이썬 interpreter는 호출 횟수 제한이 있다

#### 재귀 함수의 종료 조건
- 문제에서 **재귀 함수**가 언제 끝날지, ***종료 조건***을 꼭 명시해야되
    - 그렇지 않으면 함수가 무한 호출
- 재귀 함수의 수행은 **Stack** 자료구조를 이용해
    - 그래서 스택 자료구조를 활용하는 상당수 알고리즘은 **재귀 함수**를 이용해
        - ex) **DFS**
<br>

- **factorial**(팩토리얼) 예시 구현
    - 반복적 구현
    - 재귀적 구현

{% highlight python %}
# 반복적으로 구현한 n!
def factorial_iterative(n):
    result = 1
    for i in range(1, n+1):
        result *= i
    return result

# 재귀적으로 구현한 n!
    def factorial_recursive(n):
        if n <= 1:
            return 1;
        # n! = n + (n - 1)!를 그대로 코드로 작성하기
        return n * factorial_recursive(n - 1)

print(factorial_iterative(5))   # 120
print(factorial_recursive(5))   # 120

{% endhighlight %}
<figcaption class="caption">Queue Implementaion</figcaption>
<br>

- 보다싶이 **재귀 함수**가 더 간결해
    - 수학의 **점화식**을 그대로 소스코드로 옮겼지 때문!
<br>


## DFS (Depth-First Search)
- 먼저 그래프의 기본 구조를 알아야해
    - **Node**: Node를 **Vertex**라고도해
    - **edge**
- 프로그래밍에서 Graph는 크게 2가지 방식으로 표현할 수 있어
    - **인접행렬(Adjacency Matric)**: 2차원 배열로 그래프의 연결 관계를 표현
    - **인접 리스트(Adjacency List)**: 리스트로 그래프의 연결 관계를 표현

#### 인접행렬
- 2차원 배열에 각 노드가 연결된 형태를 기록
- 연결되어 있지 않은 노드끼리는 **INF**무한 의 비용으로 작성

{% highlight python %}
INF = 999999999  # 무한의 비용 선언

graph = [
    [0, 7, 5],
    [7, 0, INF],
    [5, INF, 0]
]

{% endhighlight %}
<figcaption class="caption">Adjacency Matric</figcaption>
<br>

#### 인접 리스트
- 모든 노드에 연결된 노드에 대한 정보를 차례대로 연결하여 저장
- 단순히 2차원 리스트를 이용
{% highlight python %}
# 행(row)이 3개인 2차원 리스트로 인접 리스트 표현
graph [[] for _ in range(3)]

# 노드 0에 연결된 노드 정보 저장(노드, 거리)
graph[0].append((1,7))
graph[0].append((2,5))

# 노드 1에 연결된 노드 정보 저장
graph[1].append((0,7))

# 노드 2에 연결된 노드 정보 저장
graph[2].append((0,5))

print(graph)  # [[(1,7), (2,5)], [(0,7)], [(0,5)]]

{% endhighlight %}
<figcaption class="caption">Adjacency Matric</figcaption>
<br>

### 인접행렬과 인접리스트의 차이
- 메모리 측면
    - 인접 행렬: 모든 관계를 저장 -> 노드 개수가 많을수록 메모리 낭비
    - 인접 리스트: 연결된 정보만을 저장 -> 메모리 효율적
        - But, 인접 행렬에 비해 정보를 얻는 속도가 **느리다** why? 연결된 데이터를 하나씩 확인해야 하기 떄문에
    