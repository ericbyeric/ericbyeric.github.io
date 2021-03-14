---
title: "그리디 (Greedy)"
layout: post
date: 2021-03-12 03:00
tag: 
- codingTest

headerImage: true
codingTest: true
hidden: true # don't count this post in blog pagination
category: codingTest
author: Hyunsoo
externalLink: false
---

## 그리디 (Greedy)

- 현재 상황에서 **당장 좋은것만 고르는 방법**
    - 순간 좋은것만 선택!
    - 현재의 선택이 나중에 미칠 영향에 대해서는 생각 안해 ㅎㅎ
- 정렬 라이브러리의 사용법을 알고 있어야해
    - ex) 최단경로? -> 플로이드 워셜(Floyd-Warshall) or 다익스트라(Dijkstra)
        - 다익스트라는 그리디 알고리즘으로 분류되
- 그리디 알고리즘은 **정렬 알고리즘**과 짝을 이뤄 출제

### 예제: 거스름돈

[Code]()
- 만약 화페단위가 500원, 400원, 100원인 경우 800월을 거슬러 줄때 최적의 해는?
    - 위의 코드로는 500, 100, 100, 100 이렇게 4개
    - 하지만 최적의 해는 400, 400 이렇게 2개
        - 어떻게 구할수 있을까?
- 화페단위가 무작위로 주어진 문제는 그리디로는 해결할수 없어
    - **다이나믹 프로그래밍**으로 해결 할 수 있다.

### 예제: 큰 수의 법칙

[Code]()
- 가장 큰수를 K번 더하고 두번째 큰수를 한번 더 더하는걸 레알 그대로 구현했네 ;;
- 수학적으로 반복되는 규칙을 도식화 해서 구현해도 좋네

### 예제: 숫자 카드 게임

[code]()
- 각 라인을 돌때마다 min value를 찾아서 가장 큰 value만 유지

{% highlight python %}
n, m = map(int, input().split())

    result = 0
    for i in range(n):
        line = list(map(int, input().split()))
        if result < min(line):
            result = min(line)

    print(result)
{% endhighlight %}
<figcaption class="caption">my answer</figcaption>
<br>

### 에제: 1이 될 때까지
