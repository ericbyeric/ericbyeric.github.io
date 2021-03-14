---
title: "구현 (implementation)"
layout: post
date: 2021-03-12 02:00
tag: 
- codingTest

headerImage: true
codingTest: true
hidden: true # don't count this post in blog pagination
category: codingTest
author: Hyunsoo
externalLink: false
---

## 구현 (implementation)
-


### 예제1: 상하좌우
- 상하좌우를 dx, dy, move_type으로 정해놓고 들어오는 input값에 따라서 차례로 수행 (굳이아디어)
- 움직여 놓고 공간을 벗어나면 **continue** 안벗어나면 이동 하는 테크닉이 유용

### 예제2: 시각
- 시간을 input으로 받으면 단순하게 h, 60, 60만큼 각각 for loop를 돌고 매번 3 이라는 숫자가 있는지 파악해서 있으면 count를 올려줘

{% highlight python %}
if '3' in str(i) + str(j) + str(k):
    count += 1
{% endhighlight %}
<figcaption class="caption">숫자 3 체크</figcaption>
<br>

### 예제3: 왕실의 나이트
- input으로 char값이 들어왔을때 **ord()**로 unicode를 만들어 *ord('a')를 빼주는것

### 예제4: 게임 개발
- 갔던 위치를 기억하는 공간을 만들어 놓기
- turn하는 기능을 메소드로 따로 만들기
- 갈곳을 미리 계산하고 가도 되는지 안되는지를 파악 
    - 되면 이동하고 turn을 초기화
    - 안되면 이동 안하고 turn만 증가
- 4번을 돌았을때 뒤로갈곳을 미리 계싼
    - 뒤로 갈수 있으면 이동
    - 못가면 break