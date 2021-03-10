---
title: "(Android) tutorial - UI Basics"
layout: post
date: 2021-03-10 10:00
tag: 
- AndroidTutorial

headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
category: project
author: Hyunsoo
externalLink: false
---

### TextView attribute
- *android:layout_centerHorizontal="true"*
- *android:Layout_layout_centerVertical="true"*
- ***android:Layout_centerInParent="true"***
- same as two codes above
- *android:textStype="bold|italic"*
- *android:onClick="func_name"*

### Method
- findViewById(R.id.txt)
- setText("Text is updated")

### Way to call Event Listener
- Set onClickListener() on button
    - button click시 작동
{% highlight java %}
// in onCreate()
Button btnHello = findViewById(R.id.btnHello);
btnHello.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        System.out.println("Hello");
    }
})
{% endhighlight %}
<figcaption class="caption">OnClickListener on button </figcaption>
<br>

- Implement View.OnClickListener interface directly to MainActivity
    - User가 버튼을 클릭하면 onClick()가 triggered 됨
{% highlight java %}
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    public void onClick(View v) {
        switch (v.getId()) {    // swtich depending on the id of the View object
            case R.id.btnHello:
                System.out.println("Hello");
                break;
            default:
                break;
        }
    }

    // in onCreate()
    Button btnHello = findViewById(R.id.btnHello);
    btnHello.setOnClickListener(this);
}
{% endhighlight %}
<figcaption class="caption">Implements View.OnClickListener  </figcaption>
<br>

- setOnLongClickListener()
    - button을 press & hold하면 이 이벤트가 작동!
    - 여기서 Toast를 사용시 this라고하면 View.OnLongClickListener interface를 말하는거라 안되
    - MainActivity.this를 쓸것
{% highlight java %}
// in onCreate()
Button btnHello = findViewById(R.id.btnHello);
btnHello.setOnLongClickListener(new View.OnLongClickListener() {
    @Override
    public boolean onLongClick(View v) {
        // Error
        Toast.makeText(this, "Hello Button Clicked", Toast.LENGTH_SHORT).show();
        // Correct
        Toast.makeText(MainActivity.this, "Long Press", Toast.LENGTH_LONG).show();
        return true;
    }
})
{% endhighlight %}
<figcaption class="caption">OnClickListener on button </figcaption>
<br>

### Toast
{% highlight java %}
// makeText(): static method
// this: context (every activity in Android is context)
Toast.makeText(this, "Hello Button Clicked", Toast.LENGTH_SHORT).show();
{% endhighlight %}
<figcaption class="caption">OnClickListener on button </figcaption>
<br>

### Button
// Button is View since this is inheritance
public class Button extends TextView