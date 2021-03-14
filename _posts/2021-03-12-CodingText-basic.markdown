---
title: "코딩테스트에 필요한 파이썬 문법"
layout: post
date: 2021-03-12 01:00
tag: 
- codingTest

headerImage: true
codingTest: true
hidden: true # don't count this post in blog pagination
category: codingTest
author: Hyunsoo
externalLink: false
---

## 자료형

### 수 자료형

#### 정수형
- 양의 정수
- 음의 정수
- 0

#### 실수형
- 소수부가 0이거나, 정수부가 0인 소수는 생략 가능

{% highlight python %}
a - 5.
print(a)

a = -.7
print(a)
{% endhighlight %}
<figcaption class="caption">0 생략 경우</figcaption>
<br>

- **e**나 **E**를 이용한 **지수 표현 방식**
    - 유효숫자e^(지수) = 유효숫자 x 10^(지수)
    - **1e9**: 10의 9제곱 (1,000,000,000)
        - 10억 대신 이렇게 쓰는게 유용 (최단 경로 파트 예제에서 나옴)
{% highlight python %}
a - 5.
print(a)

a = -.7
print(a)
{% endhighlight %}
<figcaption class="caption">Prime number</figcaption>
<br>

- 소수점을 비교하는 작업이 필요한 문제라면 실수 값을 비교하지 못해서 원하는 결과를 얻지 못할수도 있어 ㅠ
    - 그럴때는 **round()**를 써!
    - round(실수형 데이터, 반올림하고자 하는 위치 -1)
        ex) round(123.456, 2) => 123.46
<br>

#### 수 자료형의 연산
- 나누기: a/b
- 나머지: a%b
- 몫: a//b
- 거듭제곱: a ** b => a^b

---

### 리스트 자료형
- 파이썬의 리스트 자료형은 C나 자바와 같은 배열(Array) 기능을 포함
- 내부적으로 **연결 리스트**의 자료구조를 채택
    - 그래서 **append()**, **remove()**등의 메소드를 지원
- C++의 STL vector와 유사하며, 리스트 대신에 배열 혹은 테이블이라 부르기도 함.


#### 리스트 만들기

{% highlight python %}
a = [1,2,3,4]
print(a[4])

// 리스트 선언 1
a = list()

// 리스트 선언 2
a = []
{% endhighlight %}
<figcaption class="caption">리스트 선언 방식</figcaption>
<br>

- 리스트 초기화시 팁
{% highlight python %}
n = 10
a = [0]* n
print(a)    // [0,0,0,0,0,0,0,0,0,0]
{% endhighlight %}
<figcaption class="caption">리스트 초기화</figcaption>
<br>

#### 리스트의 인덱싱과 슬라이싱
- 인덱싱: 리스트의 특정한 원소에 인덱스로 접근
    - 인덱스: 양의 정수, 음의 정수(거꾸로) 둘다 가능
{% highlight python %}
a = [1,2,3,4,5,6]
a[-1]   // 6
a[-3]   // 4
{% endhighlight %}
<figcaption class="caption">리스트 초기화</figcaption>
<br>

- 슬라이싱: 리스트에서 **연속적인 위치를 갖는 원소들**을 가져와야 할 때 쓰임