---
title: "(android) Delightful user experience - Cards and colors"
layout: post
date: 2021-03-12 01:00
tag: 
- Android 
- DelightfulUserExperience

headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
category: project
author: Hyunsoo
externalLink: false
---

## "MaterialMe-Starter" Example Overview 
- start a mock sports-news app with poor design implementation 
- fix the design to meet the **design guidelines** to have delightful user experience
- 시작전 **RecycleView**를 미리 만들어서 데이터를 화면에 불러 놓아야되
- 간단히 말해서 이쁘게 만들어보자
- visual effect가 있어서 그런지 지금까지 배운것중에 가장 재밌음 :)
<br>

![image](/assets/images/android-tutorial/5-2_cards_and_colors/app_overview.png)
<figcaption class="caption"> app overview </figcaption>
<br>

---

## Required Tasks
1. Complete starter code
2. Add a CardView and images
3. Make your CardView swipeable, movable, and clickable
4. Add the FAB and choose a Material Design color palette

--- 

### Task 1: Complete starter code

#### 1.1 Create the project
- [MaterialMe-Starter](https://github.com/google-developer-training/android-fundamentals-starter-apps-v2/tree/master/MaterialMe-Starter) 
- 위의 코드 그렇게 안기니까 직접 타이핑해봄
- functional적인 부분은 잘 돌아감

#### 1.2 Explore the app
- **sport.java**: data model for each row of data in the **RecyclerView**
- **SportsAdapter.java**: adapter for the **RecyclerView**
- **MainActivity.java**: initialize the **RecyclerView** and adapter
- **strings.xml**: all of the data for the app
- **list_item.xml**: define each row of the **RecyclerView**

---

### Task 2: Add a CardView and images
- User experience를 어떻게 향상시킬수 있을까?
    - **RecyclerView** list item을 이용해서 다이나믹하고 이목을 끌게하는 시작은 좋아. 그 이상은?
        - 보통 bold imagery를 이용하면 좋아
- 그래서 **Material Design**에서 추천하는게 **CardView**야!
    - a **FrameLayout**이며 elevation과 둥근 모서리같은 특성을 가지고 있음
    - **Android Support Libraries**에 속해있는 UI component야
<br>

#### 2.1 Add the CardView
- 이 예제에서는 **build.gradle (Module: app)**에서 **dependencies**를 추가해줘야되

{% highlight xml %}
// previous version
implementation 'com.android.support:cardview-v7:26.1.0'

// Current version. 
implementation 'androidx.appcompat:appcompat:1.2.0'

{% endhighlight %}
<figcaption class="caption">CardView dependency </figcaption>
<br>

- **list_item.xml**에서 LinearLayout을 **CardView**로 감싸줘
- 여기까지하면 **strings.xml**에있는 정보들이 각각 CardView에 담겨서 수직으로 쭉 나열되
{% highlight xml %}
<androidx.cardview.widget.CardView
     xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <!-- Rest of LinearLayout -->
        
        <!-- TextView elements -->
    </LinearLayout>
</androidx.cardview.widget.CardView>
{% endhighlight %}
<figcaption class="caption">CardView in list_item.xml </figcaption>
<br>


#### 2.2 Download the images
- **CardView**는 일반 텍스트보다 여러가지 content를 한번에 보여주는데 사용하는데 효과적이야
- 이미지는 메모리 자원을 많이 잡아먹어.. :(
    - **Glide**라이브러리가 큰 용량의 이미지를 draining이나 app crashing같은 Out of memory상태를 피할수 있게 도와줘
- Image들은 **MaterialME > app > src > main > res**에 넣어줘
- 위의 Image들을 사용하려먼 path를 알야야되! 어떻게?
    - **string.xml**에 array를 define해놔!
{% highlight xml %}
<array name="sports_images">
   <item>@drawable/img_baseball</item>
   <item>@drawable/img_badminton</item>
   <item>@drawable/img_basketball</item>
   <item>@drawable/img_bowling</item>
   <item>@drawable/img_cycling</item>
   <item>@drawable/img_golf</item>
   <item>@drawable/img_running</item>
   <item>@drawable/img_soccer</item>
   <item>@drawable/img_swimming</item>
   <item>@drawable/img_tabletennis</item>
   <item>@drawable/img_tennis</item>
</array>
{% endhighlight %}
<figcaption class="caption">Image paths in strings.xml </figcaption>
<br>

#### 2.3 Modify the Sport object
- 기존의 **Sport**객체에 Image를 담을 변수를 만들어줘.
    - 변수가 추가되나 Constructor도 수정되어야 겠지?

{% highlight java %}
// in Sport class
// add imageResource variable
private final int imageResource;

// modify constructor
public Sport(String title, String info, int imageResource) {
   this.title = title;
   this.info = info;
   this.imageResource = imageResource;
}

// getter for imageResource variable
public int getImageResource() {
   return imageResource;
}
{% endhighlight %}
<figcaption class="caption">modify the Sport object</figcaption>
<br>


#### 2.4 Fix the initializeData() method
- **TypedArray** 자료구조로 XML resource의 array를 저장할 수 있어.
- **getResources().obtainTypedArray()**로 해당 **TypedArray**의 resource id를 가져올 수 있어

{% highlight java %}
TypedArray sportsImageResources = 
        getResources().obtainTypedArray(R.array.sports_images);
{% endhighlight %}
<figcaption class="caption">modify the Sport object</figcaption>
<br>

- 해당 **TypedArray**의 resource id를 가져왔으니 **mSportData** ArrayList에 Sport객체를 제대로 담을 수 있어
{% highlight java %}
for(int i=0;i<sportsList.length;i++){
   mSportsData.add(new Sport(sportsList[i],sportsInfo[i], 
       sportsImageResources.getResourceId(i,0)));
}
{% endhighlight %}
<figcaption class="caption">modify the Sport object</figcaption>
<br>

- 사용했던 **sportsImageResources**를 clean up 해줘야되
{% highlight java %}
sportsImageResources.recycle();
{% endhighlight %}
<figcaption class="caption">clean up TypedArray</figcaption>
<br>





CardView에 넣기만 해도 위로 볼록 튀어나오네!


#### 직면했던 문제들
- Cardview module version problem
    - 이전버젼
        - implementation 'com.android.support:cardview-v7:26.1.0'
    - 최신버젼
        - androidx.cardview.widget.CardView

---

#### 중요하다고 느끼는 부분
- 


#### Reference

1. **Android Developer Guide by Google**
    - Android Developer Fundamentals
        - Unit2: User experience
            - Lesson5: Delightful user experience
                - 5.1: [Drawables, styles, and themes](https://developer.android.com/codelabs/android-training-drawables-styles-and-themes?index=..%2F..%2Fandroid-training#0)
2. **Github Source Code** :  [source](https://github.com/google-developer-training/android-fundamentals-apps-v2/tree/master/Scorekeeper) 
