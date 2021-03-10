---
title: "(Android) User Interaction - Recycler View"
layout: post
date: 2021-03-10 09:00
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

## RecyclerView:

- Using **ScrollView** to scroll **View** or **ViewGroup** is easy to use, but not recommended for long, scrollable lists
- a subclass of **ViewGroup**
- resource-efficient way to display scrollable lists
- It creates a limited number of list items and reuses them for visible content
- add a click handler to each list item
- add items to the list using a **floating action button (FAB)**
<br>


---


## "RecyclerView" Example Overview 
- demonstrates how to use a **RecyclerView** to display a long scrollable list of words
- Tap a word makes it clicked
- Tap the **Floating action button (FAB)** adds a new word

![image](/assets/images/android-tutorial/4-5_recyclerView/app_overview.png)
<figcaption class="caption">RecyclerView app overview </figcaption>
<br>

---

## Required Tasks
1. Create a new project and dataset
2. Create a RecyclerView
3. Make the list interactive

--- 

### Task 1: Create a new project and dataset (심플)

#### 1.1 Create the project and layout

{% highlight java %}
public class MainActivity extends AppCompatActivity {
    private final LinkedList<String> mWordList = new LinkedList<>();
    // ... Rest of MainActivity code ...
}
{% endhighlight %}
<figcaption class="caption">mWordList linked list </figcaption>
<br>

#### 1.2 Add code to create data
- add a private member variable for the **mWordList** linked list
- populates **mWordList** with words

{% highlight java %}
// Put initial data into the word list.
for (int i = 0; i < 20; i++) {
            mWordList.addLast("Word " + i);
}
{% endhighlight %}
<figcaption class="caption">initialize data in mWordList </figcaption>
<br>

#### 1.3 Change the FAB icon
- create a new Image Asset in *drawable* folder
<br>

--- 

### Task 2: Create a RecyclerView

- Data to display: Use the **mWordList**
- **RecyclerView**: for scrolling list that contains the list items
    - part of **android.support.v7.widget.RecyclerView** Support Library
- **RecyclerView.LayoutManager**: handles the hierarchy and layout of **View** elements.
    - the layout options: ***vertical***, ***horizontal***, or a ***grid***
    - (we use a vertical **LinearLayoutManager**)
- **RecyclerView.Adapter**: connects your data to the **RecyclerView**. 자바클래스로 따로 생성할꺼야
    - It prepare the data in a **RecyclerView.ViewHolder**.
- **ViewHolder**: contains the **View** information for displaying one item from the item's layout
<br>


![image](/assets/images/android-tutorial/4-5_recyclerView/relationship_in_recyclerView.png)
<figcaption class="caption">Relationship between the data, the adapter, the ViewHolder, and the layout manager </figcaption>
<br>

#### 2.1 Modify the layout in content_main.xml
- Replace the entire **TextView** element with the following in *content_main.xml*


{% highlight java %}
<android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
</android.support.v7.widget.RecyclerView>
{% endhighlight %}
<figcaption class="caption">RecyclerView in activity_main or content_main </figcaption>
<br>


#### 2.2 Create the layout for one list item
- create a new *Layout resource file* in **app > res > layout** folder named as **wordlist_item**
- change the **ConstraintLayout** to **LinearLayout**
- set the detailed attributes
<br>

#### 2.3 Create a style from the TextView attributes
- Right-click the **TextView**, and choose **Refactor > Extract > Style**
    - (문제는 내 컴퓨터에 이 Extract이라는 옵션이 안나와.. 중요한건 아니지만..)
<br>

#### 2.4 Create an adapter (중요!)
- **Adapter**를 사용하는 이유는 list안의  data에 **View** item 으로 연결할려고!
- **View** item으로 data에 접근하려면, 먼저 **View** item에 대해 알아야되
    - 이 **Adapter**는 **ViewHolder**를 사용해
        - **ViewHolder** has info about: 
            1. **View** item
            2. its position in **RecyclerView**
<br>

- craete a new Java class named as **WordListAdapter**
- and give the following signature
    - **WordListAdapter** extends a generic adapter for **RecyclerView** to use a **View** holder
    - **View** holder is defined inside **WordListAdapter.WordViewHolder**

{% highlight java %}
public class WordListAdapter extends
    RecyclerView.Adapter<WordListAdapter.WordViewHolder>  {}
{% endhighlight %}
<figcaption class="caption">Adapter Class </figcaption>
<br>

- **onCreateViewHolder**, **onBindViewHolder** 메소드들은 **WordViewHolder** inner class를 reference하고있으며 아직 implement안됨.
- **getItemCount**도 아직 implement안됨
<br>

#### 2.5 Create the ViewHolder for the adapter
- **adapter**안에 **ViewHolder** inner class를 만들어!

{% highlight java %}
class WordViewHolder extends RecyclerView.ViewHolder {}
{% endhighlight %}
<figcaption class="caption">Adapter Class </figcaption>
<br>

- **WordViewHolder**안에 variable들을 더해줘

{% highlight java %}
public final TextView wordItemView;
final WordListAdapter mAdapter;
{% endhighlight %}
<figcaption class="caption">Adapter Class </figcaption>
<br>

- add a constructor that initializes the **ViewHolder TextView** from the **word** XML resource
    - 여기서 **word**는 **wordlist_item.xml**의 textView!
- this sets its adapter

{% highlight java %}
public WordViewHolder(View itemView, WordListAdapter adapter) {
   super(itemView);
   wordItemView = itemView.findViewById(R.id.word);
   this.mAdapter = adapter;
}
{% endhighlight %}
<figcaption class="caption">ViewHolder detail </figcaption>
<br>

- 여기까지보면 아직 adapter를 ***RecyclerView*** 에는 안붙인거야


#### 2.6 Storing your data in the adapter
- **adapter**가 데이터들을 가지고 있게 만들어 줘야해
    - **WordListAdapter**의 constructor에서 word list를 기존 데이터 (MainActivity.java의 mWordlist Linkedlist)를 initialize해줘야되
<br>

- **adapter**가 데이터를 가지고 있으려고 *private linked list*를 **WordListAdapter**에 만들어줘
{% highlight java %}
private final LinkedList<String> mWordList;
{% endhighlight %}
<figcaption class="caption">mWordList in WordListAdapter </figcaption>
<br>

- **getItemCount()를 수정**
    - it returns the size of **mWordList**

{% highlight java %}
@Override
public int getItemCount() {
   return mWordList.size();
}
{% endhighlight %}
<figcaption class="caption">getItemCount() in WordListAdapter </figcaption>
<br>

- 여기서 ***layout inflator***가 필요해
    - 위에서 말했들이 **WordListAdapter**는 데이터로부터 word list를 받아야해.
    - list item을 위한 **View**를 생성하기 위해서 "XML을 inflate해야되"
- **LayoutInflator**는 layout XML description을 읽고 해당하는 **View** item으로 convert해줘
- **LayoutInflator**를 **WordListAdapter**에 더해줘

{% highlight java %}
private LayoutInflater mInflater;
{% endhighlight %}
<figcaption class="caption">layoutInflater in WordListAdapter </figcaption>
<br>

- 이제 **WordListAdapter**의 constructor를 만들어줘
    - The constructor는 두가지 parameter가 있어
        1. contect parameter
        2. a linked list of words with the app's data
    - 이 constructor의 역할
        1. instantiate a **LayoutInflator** for **mInflater**
        2. set **mWordList** to the passed in data

{% highlight java %}
public WordListAdapter(Context context, 
                               LinkedList<String> wordList) {
   mInflater = LayoutInflater.from(context);
   this.mWordList = wordList;
}
{% endhighlight %}
<figcaption class="caption">constructor in WordListAdapter </figcaption>
<br>

- **onCreateViewHolder()**는 **onCreate()**과 비슷해
    - It inflates the item layout, and returns a **ViewHolder** with the layout and the adapter

{% highlight java %}
@Override
public WordViewHolder onCreateViewHolder(ViewGroup parent, 
                                                     int viewType) {
   View mItemView = mInflater.inflate(R.layout.wordlist_item, 
                                                     parent, false);
   return new WordViewHolder(mItemView, this);
}
{% endhighlight %}
<figcaption class="caption">onCreateViewHolder in WordListAdapter </figcaption>
<br>

- **onBindViewHolder()**는 데이터를 view holder에 연결해줘

{% highlight java %}
@Override
public void onBindViewHolder(WordViewHolder holder, int position) {
   String mCurrent = mWordList.get(position);
   holder.wordItemView.setText(mCurrent);
}
{% endhighlight %}
<figcaption class="caption">onBindViewHolder in WordListAdapter </figcaption>
<br>

#### 2.7 Create the RecyclerView in the Activity
- 지금까지 우리는 **adapter**속에 **ViewHolder**를 만들었고 이제 **RecyclerView**를 만들어서 연결해주면 data가 화면에 보여질꺼야!
<br>

- add member variables for the **RecyclerView** and the adapter
{% highlight java %}
private RecyclerView mRecyclerView;
private WordListAdapter mAdapter;
{% endhighlight %}
<figcaption class="caption">RecyclerView and Adapter in MainActivity </figcaption>
<br>

- creates the **RecyclerView** and connects it with an adapter and the data in the **onCreate()** (간단히 말하면 전부 연결해주는거~)
{% highlight java %}
// Get a handle to the RecyclerView.
mRecyclerView = findViewById(R.id.recyclerview);
// Create an adapter and supply the data to be displayed.
mAdapter = new WordListAdapter(this, mWordList);
// Connect the adapter with the RecyclerView.
mRecyclerView.setAdapter(mAdapter);
// Give the RecyclerView a default layout manager.
mRecyclerView.setLayoutManager(new LinearLayoutManager(this));
{% endhighlight %}
<figcaption class="caption">connect RecyclerView and adapter </figcaption>
<br>


![image](/assets/images/android-tutorial/4-5_recyclerView/create_recyclerView.png)
<figcaption class="caption">RecyclerView app overview </figcaption>
<br>


---

### Task 3: Make the list interactive

#### 3.1 Make item respond to clicks
- **adapter**안의 **WordViewHolder** class signature가 **View.onClicklistener**를 implement하게 만들어
{% highlight java %}
class WordViewHolder extends RecyclerView.ViewHolder 
                             implements View.OnClickListener {
{% endhighlight %}
<figcaption class="caption">connect RecyclerView and adapter </figcaption>
<br>

- 위의 listner를 implement하면 **onClick()**이 required method라고 나타나
    - 해당 data를 click하면 그 위치의 data가 "Clicked!"라는 단어가 붙어서 update되
{% highlight java %}
// Get the position of the item that was clicked.
int mPosition = getLayoutPosition();
// Use that to access the affected item in mWordList.
String element = mWordList.get(mPosition);
// Change the word in the mWordList.
mWordList.set(mPosition, "Clicked! " + element);
// Notify the adapter, that the data has changed so it can 
// update the RecyclerView to display the data.
mAdapter.notifyDataSetChanged();
{% endhighlight %}
<figcaption class="caption">connect RecyclerView and adapter </figcaption>
<br>

- 위의 listner의 내부작동을 설정 했으니 이제 **View**에 더해줘야되
- 이건 **WordViewHolder**안에서 더해줘!
{% highlight java %}
public WordViewHolder(View itemView, WordListAdapter adapter) {
   super(itemView);
   wordItemView = itemView.findViewById(R.id.word);
   this.mAdapter = adapter;

   // 이부분!
   itemView.setOnClickListener(this);
}
{% endhighlight %}
<figcaption class="caption">set the OnClickListener to the corresponding view </figcaption>
<br>

#### 3.2 Add behavior to the FAB
- list of words의 뒷부분에 새로운 단어를 추가해주고
- 데이터가 바뀐걸 adapter한데 알려주고
- 새로 추가된 item쪽으로 scroll을 내려줄꺼야
<br>

- 기존 mainActivity.java안의 **onCreate()**에 **FAB**이 define되어있는데 이 버튼의 **onClick()**에 새로운 단어를 더해줄 액션을 추가해줘
{% highlight java %}
@Override
public void onClick(View view) {
    int wordListSize = mWordList.size();
    // Add a new word to the wordList.
    mWordList.addLast("+ Word " + wordListSize);
    // Notify the adapter, that the data has changed.
    mRecyclerView.getAdapter().notifyItemInserted(wordListSize);
    // Scroll to the bottom.
    mRecyclerView.smoothScrollToPosition(wordListSize);
}
{% endhighlight %}
<figcaption class="caption">update onClick() in FAB</figcaption>
<br>

![image](/assets/images/android-tutorial/4-5_recyclerView/interactive_action.png)
<figcaption class="caption">interactive event in RecyclerView </figcaption>
<br>

---




#### 직면했던 문제들
- android.support.v7.widget.RecyclerView 가 지금은 안되고 androidx.recyclerview.widget.RecyclerView 가 되네
- style attribute style.xml로 extract하는게 내 컴퓨터에서는 안되네 ㅠㅠ. Right-click시 "Extract"이라는 아이콘 자체가 안뜸

---

#### 중요하다고 느끼는 부분
- data가 adapter속의 ViewHolder에 담겨서 해당 adapter가 RecyclerView에 속해지면 그때서야 data를 눈으로 확인 할 수 있다는점
    - data flow와 각 hierarchy가 어떻게 구성되어 있는지가 중요
- View에 onClick같은 이벤트를 추가시킬때 **ViewHolder**단에서 통제해줘야되
- Layout inflator가 XML에 표기된 레이아웃들을 메모리에 객체화 시켜줘
    - 좋은점? -> XML에 배치했던 UI 요소들을 마음껏 끌어와 쓸 수 있어
- data가 update되면 **adapter**에게 알려줘야되


#### Reference

1. **Android Developer Guide by Google**
    - Android Developer Fundamentals
        - Unit2: User experience
            - Lesson4: User interaction
                - 4.5: [RecyclerView](https://developer.android.com/codelabs/android-training-create-recycler-view?index=..%2F..%2Fandroid-training#0)
2. **Github Source Code** :  [source](https://github.com/google-developer-training/android-fundamentals-apps-v2/tree/master/RecyclerView) 
