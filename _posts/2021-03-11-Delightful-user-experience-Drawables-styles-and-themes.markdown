---
title: "(Android) Delightful user experience - Drawables, styles, and themes"
layout: post
date: 2021-03-11 09:00
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

## "ScoreKeeper" Example Overview 
- two sets of **Button** elements and two **TextView**
- keep track of the score for any point-based game
<br>


![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/app_overview.png)
<figcaption class="caption"> app overview </figcaption>
<br>

---

## Required Tasks
1. Create the Scorekeeper app
2. Create a drawable resource
3. Style your View elements
4. Themes and final touches

--- 

### Task 1: Create the Scorekeeper app

#### 1.1 Create the project

#### 1.2 Create the layout for MainActivity
- change **ConstraintLayout** (ViewGroup) to **LinearLayout**

{% highlight xml %}
// From
<android.support.constraint.ConstraintLayout ...>
// To
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android">

{% endhighlight %}
<figcaption class="caption">set LinearLayout </figcaption>
<br>

- Delete the following line of XML code
    - this is related to **ConstraintLayout**
{% highlight xml %}
xmlns:app="http://schemas.android.com/apk/res-auto"
{% endhighlight %}
<figcaption class="caption">removed line of XML code </figcaption>
<br>

- The XML code should look lie this:
{% highlight xml %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.android.scorekeeper.MainActivity">
{% endhighlight %}
<figcaption class="caption">activity_main XML code </figcaption>
<br>

- Add the following attributes
    - **android:orientation** = "**vertical**"
    - **android:padding** = "**16dp**"
<br>

#### 1.3 Create the score containers
위아래 각각 팀 점수 틀
- Add two **RelativeLayout**
    - attributes
        - **android:layout_width** = "match_parent"
        - **android:layout_height** = "0dp"
        - **android:layout_weight** = "1"
            - This determine how much space these **RelativeLayout** elements take up in the parent **LinearLayout**
            - 이것때문에 **layout_height**이 0dp여도 되는거야! proportional한 value라서~

<br>

점수 증가/감소 버튼
- Add two **ImageButton** element to each **RelativeLayout**
    - 총 4개의 버튼
        - decreasing button attributes
            - **android:layout_alignParentLeft** = "true"
            - **android:layout_alignParentStart** = "true"
            - **android:layout_centerVertical** = "true"
        - increasing button attributes
            - **android:layout_alignParentRight** = "true"
            - **android:layout_centerVertical** = "true"

<br>

점수표기
- Add a **TextView** to each **RelativeLayout** between **ImageButton** elements
    - attributes
        - **android:layout_centerHorizontal** = "true"
        - **android:layout_centerVertical** = "True"
        - android:text = "0"

<br>

팀이름
- Add a **TextView** to each **RelativeLayout**
    - attributes
        - **android:layout_alignParentTop** = "true"
        - android:layout_centerHorizontal = "true"
<br>

#### 1.4 Add vector assets
- **File > New > Vector Asset**
- size of the icon to 40dp x 40dp
- add the attributes to the **ImageButton**
- name of the icon: ic_plus, ic_minus

{% highlight xml %}
// to minus button
android:src="@drawable/ic_minus"
android:contentDescription="Minus Button"

// to plus button
{% endhighlight %}
<figcaption class="caption">add vector icon to the button </figcaption>
<br>

![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/layout_app.png)
<figcaption class="caption"> app overview </figcaption>
<br>


#### 1.5 Initialize the TextView elements and score count variable
- 점수를 올려줄려고 variable들을 미리 만들어놓기! 쉬우니까 여기는 쓱~

{% highlight java %}
// Member variables for holding the score
private int mScore1;
private int mScore2;
// Member variables for holding the score
private TextView mScoreText1;
private TextView mScoreText2;

@Override
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //Find the TextViews by ID
        mScoreText1 = (TextView)findViewById(R.id.score_1);
        mScoreText2 = (TextView)findViewById(R.id.score_2);
}
{% endhighlight %}
<figcaption class="caption">score count variable intialization </figcaption>
<br>


#### 1.6 Implement the click handlers for the ImageButton elements (중요!)
- add the **android:onClick** attribute to each **ImageButton**
- decreasing score button에 onClick걸어주기

{% highlight java %}
// to decreasing score button
android:onClick="decreasingScore"

// to increasing score button
android:onClick="increaseScore"
{% endhighlight %}
<figcaption class="caption">onClick methods</figcaption>
<br>

- 여기서 들수 있는 의문점이 두개의 decreasing button에 같은 onClick method를 걸어주면 어떻게 구분하냐지
    - 어떻게? -> **view.getID()**로 구분해! 알고나니까 당연한거지만 심플하고 좋네~

{% highlight java %}
/**
* Method that handles the onClick of both the decrement buttons
* @param view The button view that was clicked
*/
public void decreaseScore(View view) {
   // Get the ID of the button that was clicked
   int viewID = view.getId();
   switch (viewID){
       //If it was on Team 1
       case R.id.decreaseTeam1:
           //Decrement the score and update the TextView
           mScore1--;
           mScoreText1.setText(String.valueOf(mScore1));
           break;
       //If it was Team 2
       case R.id.decreaseTeam2:
           //Decrement the score and update the TextView
           mScore2--;
           mScoreText2.setText(String.valueOf(mScore2));
   }
}

/**
* Method that handles the onClick of both the increment buttons
* @param view The button view that was clicked
*/
public void increaseScore(View view) {
   //Get the ID of the button that was clicked
   int viewID = view.getId();
   switch (viewID){
       //If it was on Team 1
       case R.id.increaseTeam1:
           //Increment the score and update the TextView
           mScore1++;
           mScoreText1.setText(String.valueOf(mScore1));
           break;
       //If it was Team 2
       case R.id.increaseTeam2:
           //Increment the score and update the TextView
           mScore2++;
           mScoreText2.setText(String.valueOf(mScore2));
   }
}
{% endhighlight %}
<figcaption class="caption">onClick method details </figcaption>
<br>
--- 

### Task 2: Create a drawable resource
- Android, graphics are often handled by a resource called a **Drawable**
- 우리는 **Drawable**의 타입 중 하나인, **ShapeDrawable**을 만드어서 **ImageButton**에 적용시킬꺼야
    - 버튼의 둥그런 모양이 우리가 만들것! 어떻게 만들지 여기보자 저기보자 알아보자~
    
#### 2.1 Create a ShapeDrawable
- **ShapeDrawable** is a primitive geometric shape defined in an XML file
    - by attributes including color, shape, padding and more
- it defines a ***vector graphic***
- do right-click on the **drawable** folder
    - **New > Drawable resource file**
        - create *button_background.xml*

- add following code to create **oval** shape
{% highlight xml %}
<shape 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
        <stroke 
            android:width="2dp"
            android:color="@color/colorPrimary"/>
</shape>
{% endhighlight %}
<figcaption class="caption">onClick methods</figcaption>
<br>


#### 2.2 Apply the ShapeDrawable as a background
- 위에서 만든 Oval모양을 나타내는 ShapeDrawable을 쓰는방법!
- Add the **Drawable** as the background
{% highlight xml %}
android:background="@drawable/button_background"
{% endhighlight %}
<figcaption class="caption">set ShapeDrawable</figcaption>
<br>

- change the button height & width layout
    - 그냥 이렇게 layout사이즈를 조정하면되
{% highlight xml %}
// size is 70dp
android:layout_width="@dimen/button_size"
android:layout_height="@dimen/button_size"
{% endhighlight %}
<figcaption class="caption">set the size of shape</figcaption>
<br>

![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/drawable_resource.png)
<figcaption class="caption"> app overview applied with drawable style </figcaption>
<br>

---

### Task 3: Style your View elements
- 앞에서 **View** element들을 더하면서 code가 점점 커지고 반복되.. 특히 같은 attribute를 적용할때 귀찮고..
    - 이럴때 **style**을 공통된 attribute들에 사용하면 편하당~
- **style**은 padding, font color, font size, and background color같은 공통된 property들을 지정하는데 좋아

#### 3.1 Create Button styles
- In Android, styles can ***inherit*** properties from other styles.
- **parent** parameter는 option이야!
    - 어떤 style이던간에 parent의 style로 define된건 child에 전부 포함되
    - child가 parent의 property들을 override할수있어
- 모든 **ImageButton**들은 default properties of an **ImageButton**을 포함
    - minus **ImageButton**은 위의 특성을 포함하고 minus 이미지 포함
    - plus **ImageButton**은 위의 특성을 포함하고 plus 이미지 포함

![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/style_composition.png)
<figcaption class="caption"> style composition </figcaption>
<br>

- 이제 어떻게 style을 만들지 알아보자 (중요)
    - in **res > values** create **styles.xml**
        - **name** attribute: the name of the style
        - **parent** attribute: the style inherits this. 



{% highlight xml %}
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
{% endhighlight %}
<figcaption class="caption">style tag</figcaption>
<br>

- 위의 그림에서 1번부분
- In between the **<resource>**, add a new style with the following attributes
- default attributes of a **Button**을 **Widget.AppCompat.Button**으로부터 얻는것. 

{% highlight xml %}
<style name="ScoreButtons" parent="Widget.AppCompat.Button">
    <item name="android:background">@drawable/button_background</item>
</style>
{% endhighlight %}
<figcaption class="caption">style for ScoreButtons</figcaption>
<br>

- 위의 그림에서 3번부분
- **PlusButtons**이 **ScoreButtons**를 inherit하는걸 알수있어
- vector이미지도 여기서 설정
{% highlight xml %}
<style name="PlusButtons" parent="ScoreButtons">
    <item name="android:src">@drawable/ic_plus</item>
    <item name=
        "android:contentDescription">@string/plus_button_description</item>
</style>
{% endhighlight %}
<figcaption class="caption">style for PlusButton</figcaption>
<br>

- 위의 그림에서 2번부분
- **PlusButtons**이 **ScoreButtons**를 inherit하는걸 알수있어
{% highlight xml %}
<style name="MinusButtons" parent="ScoreButtons">
    <item name="android:src">@drawable/ic_minus</item>
    <item name=
        "android:contentDescription">@string/minus_button_description</item>
</style>
{% endhighlight %}
<figcaption class="caption">style for MinusButton</figcaption>
<br>

- 이제 저렇게 만든 **style**을 어떻게 쓰냐?
    - 밑에처럼

{% highlight xml %}
// remove those attributes
android:src
android:contentDescription
android:background

// add this one
style="@style/MinusButtons
{% endhighlight %}
<figcaption class="caption">how to add style</figcaption>
<br>

#### 3.2 Create TextView styles (이지~)
{% highlight xml %}
android:textAppearance="@style/TextAppearance.AppCompat.Headline"
{% endhighlight %}
<figcaption class="caption">textAppearance</figcaption>
<br>


![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/after_applying_style.png)
<figcaption class="caption"> after applying style </figcaption>
<br>


#### 3.3 Updating the styles
- textAppearance도 style에 넣어버려~
{% highlight xml %}
<style name="TeamText">
   <item name=
    "android:textAppearance">@style/TextAppearance.AppCompat.Display1
   </item>
</style>
{% endhighlight %}
<figcaption class="caption">textAppearance in style</figcaption>
<br>

{% highlight xml %}
<style name="TeamText">
   style="@style/TeamText" 
</style>
{% endhighlight %}
<figcaption class="caption">use textAppearance style</figcaption>
<br>


![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/after_updating_style.png)
<figcaption class="caption"> after applying style </figcaption>
<br>


### Task 4: Themes and final touches
- 지금까지는 비슷한 특성을 지민 **View** element들을 style에서 설정해줬어
- 근데 entire activity에 style을 define하고 싶으면??
    - 여기서 **theme**이라는 컨셉이 두두등장!
- night mode를 구현할꺼야
    - add an option menu. it will toggle the app between two theme (day-mode and night-mode)

#### 4.1 Explore themes
- open the **AndroidManifest.xml** file, fine the **<application>** tag, and change the **android:theme**
- 밑의 코드처럼 하면 앱에서 ActionBar가 사라져
{% highlight xml %}
// test
android:theme="@style/Theme.AppCompat.Light.NoActionBar"

// back to AppTheme
android:theme="@style/AppTheme"
{% endhighlight %}
<figcaption class="caption">NoActionBare</figcaption>
<br>

- 만약 특정한 activity에만 theme을 적용하고 싶으면
    - In **AndroidManifest.xml**에서 **<application>**말고 **<Activity>**tag에 theme을 적용시키면되
<br>

#### 4.2 Add theme button to the menu
- open **strings.xml** and create two string resources
{% highlight xml %}
<string name="night_mode">Night Mode</string>
<string name="day_mode">Day Mode</string>
{% endhighlight %}
<figcaption class="caption">NoActionBare</figcaption>
<br>

- In **res** directory에서 right-click **New > Android resource file**
    - Name the file **main_menu**, change the **Resource Type** to **Menu**
    - add a menu item
<item 
    android:id="@+id/night_mode"
    android:title="@string/night_mode"/>
<figcaption class="caption">NoActionBare</figcaption>
<br>

- In **MainActivity**, override a method, **onCreateOptionsMenu(menu:Menu):boolean** located under the **android.app.Activity** category
{% highlight java %}
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        return super.onCreateOptionsMenu(menu)
    }
{% endhighlight %}
<figcaption class="caption">Initial method</figcaption>
<br>

- Add the code below to inflate the menu
{% highlight java %}
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflator().inflate(R.menu.main_menu, menu);
        return super.onCreateOptionsMenu(menu)
    }
{% endhighlight %}
<figcaption class="caption">Add Menu Inflator</figcaption>
<br>

#### 4.3 Change the theme from the menu
- The **DayNight** theme uses the **AppCompatDelegate** class to get the *night mode* options in your **Activity**.
    - 더 알아보고싶으면 [여기](https://medium.com/androiddevelopers/appcompat-v23-2-daynight-d10f90c83e94#.ub2ptykn9) 블로그로 가봐~
<br>

- In the **styles.xml** file, **AppTheme**의 parent를 **Theme.AppCompat.DayNight.DarkActionBar**로 바꿔!
- Override the **onOntionsItemSelected()** method in **MainActivity**
    - ***night mode***가 enable되어있는지 체크!
{% highlight java %}
@Override
public boolean onOptionsItemSelected(MenuItem item) {
   //Check if the correct item was clicked
   if(item.getItemId()==R.id.night_mode){}
       // TODO: Get the night mode state of the app.
   return true;
}
{% endhighlight %}
<figcaption class="caption">Night Mode check function 초기</figcaption>
<br>

- Replace the **TODO** about with code below
- ***night mode***가 enable되어있는지 체크!
        - 만약 enable하면 night mode를 disabled state으로 바꿔, 아니면 그냥 맞다고 하고
{% highlight java %}
if(item.getItemId()==R.id.night_mode){
    // Get the night mode state of the app.
    int nightMode = AppCompatDelegate.getDefaultNightMode();
    //Set the theme mode for the restarted activity
    if (nightMode == AppCompatDelegate.MODE_NIGHT_YES) {
        AppCompatDelegate.setDefaultNightMode
                          (AppCompatDelegate.MODE_NIGHT_NO);
} else {
   AppCompatDelegate.setDefaultNightMode
                         (AppCompatDelegate.MODE_NIGHT_YES);
}
// Recreate the activity for the theme change to take effect.
recreate();
{% endhighlight %}
<figcaption class="caption">Night Mode state check</figcaption>
<br>

- 그러면 코드는 현재의 night mode setting을 어떻게 아냐?
    - **AppCompatDelegate.GetDefaultNightMode()**를 call 해서 알아!

- 중요 포인트중 하나! **Theme**은 오직 **Activity**가 생성(create) 됐을때만 바꿀수있어
    - 그래서 **recreate()**을 해주는거야
- 여기서 문제는 현재 Night mode인데도 Menu의 string이 *night mode*라고 뜨는것 ㅠ
    - 아까 override한 **onCreateOptionsMenu()**에서 **return super.onCreateOptionsMenu(menu)**대신에 밑의 코드를 넣어
{% highlight java %}
// in onCreateOptionsMenu() method
int nightMode = AppCompatDelegate.getDefaultNightMode();
if(nightMode == AppCompatDelegate.MODE_NIGHT_YES){
    menu.findItem(R.id.night_mode).setTitle(R.string.day_mode);
} else{
    menu.findItem(R.id.night_mode).setTitle(R.string.night_mode);
}
return true;
{% endhighlight %}
<figcaption class="caption">String change in Night mode</figcaption>
<br>


![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/dark_mode_apply.png)
<figcaption class="caption"> after applying style </figcaption>
<br>

#### 4.4 SaveInstanceState
- 이전의 내용들중에서(어딘지 까먹음 찾아서 표기해놓겠음..) 우리가 화면을 회전시킬때 **Activity**가 예상치못한 시간에 destroy되고 recreate되는걸 미리 방지해야했어
    - 예들들어, 화면 돌릴떄 **TextView**가 가지고 있는 값이 **0** 으로 리셋되는경우. 이걸 고쳐야되!
<br>

- open **MainActivity** and add *tags* under the member variable
{% highlight java %}
static final String STATE_SCORE_1 = "Team 1 Score";
static final String STATE_SCORE_2 = "Team 2 Score";
return true;
{% endhighlight %}
<figcaption class="caption">add tags</figcaption>
<br>

- override the **onSaveInstanceState()** method at the end of **MainActivity**
{% highlight java %}
@Override
protected void onSaveInstanceState(Bundle outState) {
   // Save the scores.
   outState.putInt(STATE_SCORE_1, mScore1);
   outState.putInt(STATE_SCORE_2, mScore2);
   super.onSaveInstanceState(outState);
}
{% endhighlight %}
<figcaption class="caption">overrice onSaveInstanceState()</figcaption>
<br>

- At the end of the **onCreate()**, **savedInstanceStae**이 있나 없나 체크하는 코드 추가
    - 만약 있으면, score을 restore!
{% highlight java %}
if (savedInstanceState != null) {
    mScore1 = savedInstanceState.getInt(STATE_SCORE_1);
    mScore2 = savedInstanceState.getInt(STATE_SCORE_2);

    //Set the score text views
    mScoreText1.setText(String.valueOf(mScore1));
    mScoreText2.setText(String.valueOf(mScore2));
}
{% endhighlight %}
<figcaption class="caption">overrice onSaveInstanceState()</figcaption>
<br>

![image](/assets/images/android-tutorial/5-1_drawable_styles_themes/rotation_enabled.png)
<figcaption class="caption"> final </figcaption>
<br>
--- 





#### 직면했던 문제들
- 
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
