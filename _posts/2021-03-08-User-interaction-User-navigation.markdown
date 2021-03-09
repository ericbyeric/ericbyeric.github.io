---
title: "(Android) User Interaction - User nagivation"
layout: post
date: 2021-03-08 09:00
tag: 
- Android 
- UserExperience

headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
category: project
author: Hyunsoo
externalLink: false
---

## User Navigation:

- Learn how to add **UP** button (left-facing arrow) to the app bar
    - used to navigate to the parent screen
- How to set up an app with **tab navigation** and swipe views
    - a popular way to create lateral(옆의) navigation from one child screen to a sibling child screen
<br>

![image](/assets/images/android-tutorial/4-4_user_navigation/navigation_bar_hierarchy.png)
<figcaption class="caption">hierarchy of navigation bar </figcaption>
<br>

---


## "Tap Experiment" Example Overview 
- App for tab navigation
- It shows three tabs below the app bar to navigate to sibling screens
- The user can swipe left and right to visit the content screens

![image](/assets/images/android-tutorial/4-4_user_navigation/app_overview.png)
<figcaption class="caption">Tab Experiment app overview </figcaption>
<br>

---

## Required Tasks
1. Add an Up button for ancestral navigation
2. Use tab navigation with swipe views


--- 

### Task 1: 상위 navigation으로 가는 UP button 더하기!

- **Up** button 만드는법
    1. Go to ***AndroidMAnifest.xml*** file
    2. change the **Activity** element for **OrderActivity** as below
<br>

{% highlight java %}
// From
<activity android:name=".OrderActivity"/>

// To
<activity android:name="com.example.android.droidcafeinput.OrderActivity"
    android:label="Order Activity"
    android:parentActivityName=".MainActivity">
    <meta-data android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity"/>
</activity>
{% endhighlight %}
<figcaption class="caption">Modify AndroidManifest.xml file to add UP button </figcaption>
<br>

![image](/assets/images/android-tutorial/4-4_user_navigation/upButton.png)
<figcaption class="caption">UP button in OrderActivity activity </figcaption>

--- 

### Task 2: Tab navigation with Swipe views (쓱~)

- patent화면으로 navigate필요없이 category(sibling) 간에 왔다갔다 할수있게 하는 기능
- child screen들을 left, right swipe으로 navigation가능
- **TabLayout** : the primary class used for diaplaying tabs
    - provides a **horizontal layout** to display tabs
- **PagerAdapter** class
    - generate the screens that the view shows
- **ViewPager**
    - a layout manager lets the user filp left and right through the screens.
    - Automatically handles user swipes to screens or **View** elements
    - used in conjunction with **Fragment**
        - With **Fragment**, it is a convenient way to manage the lifecycle of a screen "page"
        - standard adapters for using fragments with the ***ViewPager***
            - **FragmentPagerAdapter**
                - Designed for navigation between sibling screens (pages) representing a fixed, small number of screens.
            - **FragmentStatePagerAdapter**
                - Designed for paging across a collection of screen (pages) for which the number of screens is undetermined.
                - It destroys each ***Fragment*** as the user navigates to other screens, minimizing memory usage.
<br>


![image](/assets/images/android-tutorial/4-4_user_navigation/lateral_navigation.png)
<figcaption class="caption">Lateral navigation from one category (story) screen to another </figcaption>
<br>

#### 2. **Create the project and layout**
2-1 Edit the **build.gradle(Module:app) file
- add the following line
-  **TabLayout** 을 사용하기 위해서 필요해

{% highlight java %}

implementation 'com.android.support:design:26.1.0'

{% endhighlight %}
<figcaption class="caption">dependency required to use TabLayout </figcaption>
<br>


2-2. **Toolbar**를 사용하기 위해서 (no app bar and app title) ***res > values > styles.xml*** 에서 app bar와 app title을 없애줘
- (내 코드의 경우는 themes.xml에서 따로 지정해주넹)
{% highlight java %}

<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
   <!-- Other style attributes -->      
   <item name="windowActionBar">false</item>
   <item name="windowNoTitle">true</item>
</style>

{% endhighlight %}
<figcaption class="caption">dependency required to use TabLayout </figcaption>
<br>

2-3. activity_main.xml의 layout을 **ConstraintLayout**에서 **RelativeLayout**으로 바꿔

2-4. **Toolbar**, **TabLayout** 과 **ViewPager**을 **RelativeLayout**속에 더해줘  
- ***app:popupTheme*** attribute for Toolbar가 Tab했을때 나오는 visual effect야

2-5. Add the following statement to **RelativeLayout**
{% highlight java %}
<RelativeLayout xmlns:app="http://schemas.android.com/apk/res-auto"
{% endhighlight %}
<figcaption class="caption">res-auto </figcaption>
<br>


#### 3. Create a class and layout for each fragment
3-1. create three blank **Fragment** in **com.example.android.tabexperiment**
- name as *TabFragment1.xml*, *TabFragment2.xml* and *TabFragment3.xml*
- Each fragment is created with its class definition set to extend **Fragment**
- Also, each **Fragment** inflates the layout associated with the screen

{% highlight java %}
public class TabFragment1 extends Fragment {

    public TabFragment1() {
        // Required empty public constructor
    }
    
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment.
        return inflater.inflate(R.layout.tab_fragment1, container, false);
    }

}
{% endhighlight %}
<figcaption class="caption">TabFragment1 looks like this</figcaption>
<br>


#### 4. Edit the fragment layout
4-1. in each **Fragment** layout XML, change the **FrameLayout** to **RelativeLayout**

4-2. Change the **TextView** text and set the **layout_width** and **layout_height** to wrap_content in each fragment xml file

#### 5. Add a PagerAdapter [중요!]

- **adapter-layout manager pattern** 이 여러가지 screen을 하나의 activity안에서 제공해줘
    - use an ***adapter*** to fill the content screen to show in the **Activity**
        - -> 컨텐츠 채우는놈
    - use a ***layout manager*** that changes the content screens depending on which tab is selected
        - -> 컨텐츠 tab에따라 바꾸는놈

5-1. create a new **PagerAdapter** java class extending **FragmentStatePagerAdapter** in *com.example.tabexperiment* folder
- implement ***getItem()*** and ***getCount()*** methods

5-2. create constructor matching ***super***

5-3. add an integer member variable **mNumOfTabs**, and change the constructor to use it

{% highlight java %}
public class PagerAdapter extends FragmentStatePagerAdapter {
    int mNumOfTabs;
    
    public PagerAdapter(FragmentManager fm, int NumOfTabs) {
        super(fm);
        this.mNumOfTabs = NumOfTabs;
    }

    /**
     * Return the Fragment associated with a specified position.
     *
     * @param position
     */
    @Override
    public Fragment getItem(int position) {
        return null;
    }

    /**
     * Return the number of views available.
     */
    @Override
    public int getCount() {
        return 0;
    }
}
{% endhighlight %}
<figcaption class="caption">code in PagerAdapter java class</figcaption>
<br>

5-4. change the newly added **getItem()** method
- return the **Fragment** to show based on which tab is clicked by using switch case

{% highlight java %}
@Override
public Fragment getItem(int position) {
        switch (position) {
            case 0: return new TabFragment1();
            case 1: return new TabFragment2();
            case 2: return new TabFragment3();
            default: return null;
        }
}
{% endhighlight %}
<figcaption class="caption">change in getItem() method</figcaption>
<br>

5-5. change getCount() method
- return the number of tabs

{% highlight java %}
@Override
public int getCount() {
        return mNumOfTabs;
}
{% endhighlight %}
<figcaption class="caption">change in getCount() method</figcaption>
<br>


#### 6. Inflate the Toolbar and TabLayout [더중요!!]
- 현재 이미 app bar와 Toolbar가 셋업 되어 있는 상태
- have to inflate the **Toolbar**
- create an instance of **TabLayout** to position the tabs


6-1. add the following code inside the **onCreate()** in MainActivity
- (내 코드에서는 Toolbar module을 지원을 안했음 ㅠㅠ 왜인지 한번 알아볼것..!)
    - 알아보니 **androidx.appcompat.widget.Toolbar** 모듈을 사용해야함
{% highlight java %}
protected void onCreate(Bundle savedInstanceState) {
    // ... Code inside onCreate() method
    // Inflate the toolbar
    android.support.v7.widget.Toolbar toolbar = 
                                  findViewById(R.id.toolbar);
    setSupportActionBar(toolbar);
    // Create an instance of the tab layout from the view.
}
{% endhighlight %}
<figcaption class="caption">Inflate the Toolbar</figcaption>
<br>

6-2. create an instance of the tab layout
- add this code at the end of the **onCreate()** method
{% highlight java %}
// Create an instance of the tab layout from the view.
TabLayout tabLayout = findViewById(R.id.tab_layout);
// Set the text for each tab.
tabLayout.addTab(tabLayout.newTab().setText(R.string.tab_label1));
tabLayout.addTab(tabLayout.newTab().setText(R.string.tab_label2));
tabLayout.addTab(tabLayout.newTab().setText(R.string.tab_label3));
// Set the tabs to fill the entire layout.
tabLayout.setTabGravity(TabLayout.GRAVITY_FILL);
}
{% endhighlight %}
<figcaption class="caption">Create a tabLayout instance</figcaption>
<br>

#### 7. Use PagerAdapter to manage screen views
- add this code at the end of the **onCreate()** method in MainActivity

7-1. use **PagerAdapter** to manage screen (page) views in the fragments

{% highlight java %}
// Use PagerAdapter to manage page views in fragments.
// Each page is represented by its own fragment.
final ViewPager viewPager = findViewById(R.id.pager);
final PagerAdapter adapter = new PagerAdapter
              (getSupportFragmentManager(), tabLayout.getTabCount());
viewPager.setAdapter(adapter);
{% endhighlight %}
<figcaption class="caption">Create a tabLayout instance</figcaption>
<br>

7-2. set a listener (**TabLayoutOnPageChangeListener**) to detect if a tab is clicked, and create the **onTabSelected()** method
- **onTabSelected()** method set the **ViewPager** to the appropriate tabbed screen.

{% highlight java %}
// Setting a listener for clicks.
viewPager.addOnPageChangeListener(new
                TabLayout.TabLayoutOnPageChangeListener(tabLayout));
tabLayout.addOnTabSelectedListener(new 
                TabLayout.OnTabSelectedListener() {
    @Override
    public void onTabSelected(TabLayout.Tab tab) {
        viewPager.setCurrentItem(tab.getPosition());
    }

    @Override
    public void onTabUnselected(TabLayout.Tab tab) {
    }

    @Override
    public void onTabReselected(TabLayout.Tab tab) {
    }
});
{% endhighlight %}
<figcaption class="caption">Create a tabLayout instance</figcaption>
<br>

- Run the app. Tap each tab to see each "page" (screen)
- You should be able to swipe left and right to visit the different "pages"



---

#### 중요하다고 느끼는 부분
- **Up**Button을 app bar에 더해줄때 AndroidManifest.xml 파일을 수정 해야한다는 점.
- Tab Navigation을 만들때 구성요소가 **Toolbar**, **TabLayout**, **ViewPager**로 구성되어 있다는점
- **PagerAdapter**는 컨텐츠 채우는놈!
    - 개별적인 Java class로서 작동
    - 탭을 클릭함에 따라 해당 탭의 Fragment를 return해줘
- **ViewPager**는 컨텐츠를 tab에따라 바꾸는놈
- 각각의 tab에 해당되는 *Fragment*를 각각 생성해 줘야한다는 점
    - 이 각각의 *Fragment*가 screen을 inflate하는거야
- **Toolbar**를 inflate해주고 **TabLayout**의 instance를 생성해 줘야해
    - tabLayout의 label을 여기서 set해주고 tab에 띄워준다
- MainActivity에서 **ViewPager**에 **PageAdapter**를 set해줘
    - tab이 클릭 되었는지 **TabLayoutOnPageChangeListener** listener를 set해줘
        - 해당 listener속의 **onTabSelected()**에서 **ViewPager에 tabbed된 스크린을 보여줘


#### Reference

1. **Android Developer Guide by Google**
    - Android Developer Fundamentals
        - Unit2: User experience
            - Lesson4: User interaction
                - 4.4: [User navigation](https://developer.android.com/codelabs/android-training-provide-user-navigation?index=..%2F..%2Fandroid-training#0)
2. **Github Source Code** :  [source](https://github.com/ericbyeric/android-fundamentals-apps-v2/tree/master/TabExperiment/) 
