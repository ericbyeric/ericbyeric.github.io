---
title: "(Android) User Interaction - Menus and pickers"
layout: post
date: 2021-03-05 09:00
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

## Menus and pickers:

- the native **ActionBar** behaves differently depending on the version of Android running on the device. This is the reason why we should use the **v7 appcompat** support library's **Toobal** as an app bar
- **Toolbar** includes the most recent features, and works for any device that can use the support library
- Seperatioon of layout by using **include layout**
- resource for **adding the app bar** : https://developer.android.com/training/appbar/index.html
- 

---


## "Droid Cafe" Example Overview 
- add *action overflow button*
- 

---


## Required Tasks



--- 

### Task 1:

- **<item>** within **<menu>**


--- 

### Task 2:

- add icon images in **drawable** folder
- item attributes
    - **android:icon** : icon image
    - **app:showAsAction** : always | ifRoom | never
- how to create a **option menu**

---

### Task 3:

- **Option menu**
    - onCreateOptionsMenu() : inflator
    - onOptionsItemSelected()
- The **onOptionsItemSelected()** method handles selections from the options menu.
- **Context menu**
    - onCreateContextMenu() : inflator
    - onContextItemSelected()


---


### Task 4:
example: "Dialog for Alert"
- **Builder design pattern** 
- use **AlertDialog.Builder**
    - onClickShowAlert() : handler for the **Alert** button
    - setTitle()
    - setMessage()
    - setPositiveButton()
    - setNegativeButton()

---

### Task 5:

- ***picker***: ready-to-use dialogs, for picking a time or a date
- **Fragment**: a behavior or a portion of a UI within an **Activity**. like a mini-**Activity**
    - benefit?
        - can isolate the code sections
- instance of **DialogFragment**, a subclass of **Fragment**

### What I have learned

- **option menu** 와 **context menu**의 차이와 어떻게 만드는지를 알아야되
- **Dialog**를 만드는 방식 파악
- **Picker**를 사용하는 방식 파악
- 각각의 요소들이 왜 필요하고 어떻게 쓰이는지에 집중

#### Reference

1. **Android Developer Guide by Google**
    - Android Developer Fundamentals
        - Unit2: User experience
            - Lesson4: User interaction
                - 4.1: [Clickable images](https://developer.android.com/codelabs/android-training-clickable-images?index=..%2F..%2Fandroid-training#4/)
2. **Github Source Code** :  [source](https://github.com/ericbyeric/android-fundamentals-apps-v2/tree/master/DroidCafe/) 
