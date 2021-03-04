---
title: "User Interaction - Clickable images"
layout: post
date: 2021-03-04 12:00
tag: 
- Android 
- UserExperience
- ClickableImages
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
- Display a **Toast** message when the user tap an image
- Tap a shopping-cart button to proceed to the next Activity

<br>   
![image](/assets/images/android-tutorial/dessert-ordering-app.png)
<figcaption class="caption">Dessert ordering app preview </figcaption>


---


## Required Tasks

* **Task 1**: Add images to the layout
* **Task 2**: Add onClick methods for images
* **Task 3**: Change the floating action button 

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
![image](/assets/images/android-tutorial/constraint-layout-1.png)
<figcaption class="caption">Constraint Layout For ImageView </figcaption>
<br>

![image](/assets/images/android-tutorial/constraint-layout-2.png)
<figcaption class="caption">Constraint Layout For Text Descriptions </figcaption>


---

### Task 2: Add onClick methods for images

- Create a Toast method
    - Use string resources in **strings.xml**
    - create **displayToast()** method to the end of **MainActivity**

{% highlight html %}
    <string name="donut_order_message">You ordered a donut.</string>
    <string name="ice_cream_order_message">You ordered an ice cream sandwich.</string>
    <string name="froyo_order_message">You ordered a FroYo.</string>
{% endhighlight %}
<figcaption class="caption">String resources </figcaption>
<br>

{% highlight html %}
    public void displayToast(String message) {
        Toast.makeText(getApplicationContext(), message,
                            Toast.LENGTH_SHORT).show();
    } 
{% endhighlight %}
<figcaption class="caption">displayToast() </figcaption>
<br>

- Create click handlers
    - add **showDonutOrder()** 
