---
title: "(Android) User Interaction - Clickable images"
layout: post
date: 2021-03-04 12:00
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

## Clickable images:

- The user interface(UI) in android consists of objects called *views*.
- The **View** class represents the basic building blocks for all UI components.
- Store the image for the **ImageView** in the **drawables** folder of your project.

---


## Example Overview
- build a new dessert-ordering app
- Display a **Toast** message when the user taps an image
- Tap a shopping-cart button to proceed to the next Activity

<br>   
![image](/assets/images/android-tutorial/4-1_clickable_images/dessert-ordering-app.png)
<figcaption class="caption">Dessert ordering app preview </figcaption>


---


## Required Tasks

* **Task 1**: Add images to the layout
* **Task 2**: Add onClick methods for images
* **Task 3**: Change the floating action button 
* **Task 4**: Launch a second Activity called **OrderActivity**

--- 

### Task 1: Add images to the layout

1. Start the new project with **Basic Activity**
2. Add the images
    - Add the images into **drawable** folder by using the path: *project_name* **> app > src > main > res > drawable**
    - Drag an **ImageView** to the layout
3. **ImageView** attribute
    - **contentDescription**
        - It might have a warning of *hardcoded text*
        - Extract the string value
4. Add the text descriptions

#### Constraint Layout Figure
![image](/assets/images/android-tutorial/4-1_clickable_images/constraint-layout-1.png)
<figcaption class="caption">Constraint Layout For ImageView </figcaption>
<br>

![image](/assets/images/android-tutorial/4-1_clickable_images/constraint-layout-2.png)
<figcaption class="caption">Constraint Layout For Text Descriptions </figcaption>


---

### Task 2: Add onClick methods for images

1. Create a Toast method
    - Use string resources in **strings.xml**
    - create **displayToast()** method to the end of **MainActivity**

2. Create click handlers
    - add **showDonutOrder()** and use the previously created **displayToast()** to display **Toast** message
    - add **showIceCreamOrder()** and **showFroyoOrder()**

3. Add the onClick attribute
    - **android:onClick** to ***showDonutOrder()***, ***showIceCreamOrder()*** and ***showFroyoOrder()***
<br>

{% highlight html %}
    <string name="donut_order_message">You ordered a donut.</string>
    <string name="ice_cream_order_message">You ordered an ice cream sandwich.</string>
    <string name="froyo_order_message">You ordered a FroYo.</string>
{% endhighlight %}
<figcaption class="caption">String resources </figcaption>
<br>

{% highlight java %}
    public void displayToast(String message) {
        Toast.makeText(getApplicationContext(), 
                            message,
                            Toast.LENGTH_SHORT)
                            .show();
    } 
{% endhighlight %}
<figcaption class="caption">displayToast() </figcaption>
<br>


{% highlight java %}
    /**
    * Shows a message that the donut image was clicked.
    */
    public void showDonutOrder(View view) {
        displayToast(getString(R.string.donut_order_message));
    } 
{% endhighlight %}
<figcaption class="caption">showDonutOrder() </figcaption>
<br>

{% highlight java %}
    /**
    * Shows a message that the ice cream sandwich image was clicked.
    */
    public void showIceCreamOrder(View view) {
        displayToast(getString(R.string.ice_cream_order_message));
    }

    /**
    * Shows a message that the froyo image was clicked.
    */
    public void showFroyoOrder(View view) {
        displayToast(getString(R.string.froyo_order_message));
    }
{% endhighlight %}
<figcaption class="caption">showIceCreamOrder() and showFroyoOrder() </figcaption>
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
