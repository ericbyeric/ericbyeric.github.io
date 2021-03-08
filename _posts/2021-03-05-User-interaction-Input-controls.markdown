---
title: "(Android) User Interaction - Input controls"
layout: post
date: 2021-03-04 10:00
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


## Input Controls:
### Main points

- **android:inputType** attribute for **EditText** elements
    - name, address, single-line, multiple-line, a numeric keypad and etc..
- add radio buttons and offer a spinner input control
- use attributes to control the on-screen keyboard appearance, and to set the type of data entry for an **EditText**
- default check on a radio button

---


## "Droid Cafe" Example Overview 

- how to use **android:inputType** attribute for **EditText** elements
    - person's name, address, single-line, multiple-line, a numeric keypad and etc..
- spinner input control for selecting the label (*Home*, *Work*, *Other*, *Custom*) for the phone number

<br>   
![image](/assets/images/android-tutorial/4-2_input_controls/droid-cafe-app.png)
<figcaption class="caption">Droid Cafe app preview </figcaption>


---


## Required Tasks

* **Task 1**: Experiment with text entry attributes
* **Task 2**: Use radio buttons
* **Task 3**: Use a spinner for user choices
* **Task 4**: 

--- 

### Task 1: Experiment with text entry attributes

1. Add an EditText for entering a name
    - **android:inputType** attribute is set to **textPersonName**
2. Add a multiple-line EditText
    - TextView와 EditText의 라인을 맞추려면 baseline들끼리 맞춰주면되
    - **android:inputType** is set to **textMultiLine**
3. Use a keypad for phone numbers
     -**android:inputType** is set to **phone**
        - this enable to show the numeric keypad
4. Combine input types in one EditText
    - ex) android:inputType = "textCapSentences | textMultiLine"

#### Layout Figure
![image](/assets/images/android-tutorial/4-2_input_controls/constraint_layout_inputType_phone.png)
<figcaption class="caption">Constraint Layout For phone inputType </figcaption>
<br>

![image](/assets/images/android-tutorial/4-2_input_controls/layout_screen.png)
<figcaption class="caption">Screen layout </figcaption>


---

### Task 2: Use radio buttons

- 우선 radio button을 왜 사용할까를 생각해야해
    - Why? -> 유저가 모든 옵션을 나란히 보기를 원할때!
        - 만약 위의 경우가 아니면, **Spinner**를 사용하면 좋아!

1. Add a RadioGroup and radio buttons
2. Add the radio button click handler
    1. create onRadioButtonClicked() method
    2. 먼저 **displayToast** method 
{% highlight Java %}
<RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="24dp"
        android:layout_marginStart="24dp"
        android:orientation="vertical"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/delivery_label">

        <RadioButton
            android:id="@+id/sameday"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="onRadioButtonClicked"
            android:text="Same day messenger service" />

        <RadioButton
            android:id="@+id/nextday"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="onRadioButtonClicked"
            android:text="Next day ground delivery" />

        <RadioButton
            android:id="@+id/pickup"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="onRadioButtonClicked"
            android:text="Pick up" />
</RadioGroup>
{% endhighlight %}
<figcaption class="caption">Adding radio button</figcaption>
<br>



---

### Task 3: Change the floating action button

<span class="evidence">FloatingActionButton</span> is implemented.

As an example of **FloatingActionButton**, the Gmail app provives a **floating action button** to create a new email message.

1. Add a new icon
    1. right-click the **drawable** folder
    2. choose **New > Image Asset**
    3. choose **Action Bar and Tab Icons**
    4. change the name
    5. click the clip art image  
<br>

2. Add an Activity
    - add another activity called <span class="evidence">OrderActivity.java</span> 
        - Right-click the *com.example.android.droidcafe* folder and choose **New > Activity > Empty Activity**
<br>

3. Change the action
    - make an explicit intent to start **OrderActivity**
    - (we do not use **Snackbar** in this example)
<br>

{% highlight java %}
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton fab = findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
//                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
//                        .setAction("Action", null).show();
                Intent intent = new Intent(MainActivity.this, OrderActivity.class);
                intent.putExtra(EXTRA_MESSAGE, mOrderMessage);
                startActivity(intent);
            }
        });
    }
{% endhighlight %}
<figcaption class="caption">Change onClick(View view) method </figcaption>

---

### Task 4: Launch a second Activity called **OrderActivity**

1. add a **TextView** (in OrderActivity)
2. assign the message string **mOrderMessage** with the **donut_order_message** string (in MainActivity)
3. define the key for an **intent.putExtra** (in MainActivity)
4. Change the **onClick()** method to include the **intent.putExtra** statement (in MainActivity)
5. extract the string message from intent (in OrderActivity)
<br>

{% highlight java %}
    mOrderMessage = getString(R.string.donut_order_message);
    displayToast(mOrderMessage);
{% endhighlight %}
<figcaption class="caption">assign mOrderMessage </figcaption>
<br>

{% highlight java %}
    public static final String EXTRA_MESSAGE = 
                      "com.example.android.droidcafe.extra.MESSAGE";
{% endhighlight %}
<figcaption class="caption">assign the key of the intent </figcaption>
<br>

{% highlight java %}
    public void onClick(View view) {
        Intent intent = 
                new Intent(MainActivity.this, OrderActivity.class);
        intent.putExtra(EXTRA_MESSAGE, mOrderMessage);
        startActivity(intent);
    }
{% endhighlight %}
<figcaption class="caption">assign the key of the intent </figcaption>
<br>

{% highlight java %}
Intent intent = getIntent();
    String message = "Order: " + 
                    intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
    TextView textView = findViewById(R.id.order_textview);
    textView.setText(message);
{% endhighlight %}
<figcaption class="caption">set the message in OrderActivity</figcaption>
<br>

![image](/assets/images/android-tutorial/4-1_clickable_images/dessert-ordering-app-2.png)
<figcaption class="caption">Constraint Layout For Text Descriptions </figcaption>

---

#### Reference

1. **Android Developer Guide by Google**
    - Android Developer Fundamentals
        - Unit2: User experience
            - Lesson4: User interaction
                - 4.1: [Clickable images](https://developer.android.com/codelabs/android-training-clickable-images?index=..%2F..%2Fandroid-training#4/)
2. **Github Source Code** :  [source](https://github.com/ericbyeric/android-fundamentals-apps-v2/tree/master/DroidCafe/) 
