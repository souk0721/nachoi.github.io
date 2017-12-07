---
layout: post
title:  "[Android][Kotlin] 코틀린 RecyclerView(1)"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view]
---

## ListView와 RecyclerView
`ListView`에서는 모든 데이터에 대한 View를 만들고, View가 사라졌다가 나타날 때마다 리소스를 불러와야 했다. 이 방법은 많은 메모리와 저장 공간을 사용하므로, 대용량의 데이터를 이용하면 앱이 느려지거나 충돌할 가능성이 있었다.

`RecyclerView`는 ListView의 단점을 보완하기 위해 만들어졌다. `ViewHolder`를 필수적으로 사용해야 하고 `LayoutManager`를 설정하는 등 조금 더 복잡할 수 있지만, 앱에서 불필요하게 메모리를 사용하는 일은 줄어들 것이다.

안드로이드 리사이클러뷰를 사용하기 위해 준비해야 할 것을 정리해보았다. (이전의 [ListView 포스트](/studynote/2017/12/01/Android-Kotlin-ListView.html)에서 정리했던 것과 유사하다.)

1. RecyclerView에서 사용할 데이터 정리 (클래스 정의 & 데이터 파일 생성)
2. Gradle에서 RecyclerView 컴파일 추가
3. RecyclerView를 사용할 레이아웃의 xml 파일에 RecyclerView 추가
4. Custom RecyclerView item 레이아웃 파일 생성
5. Adapter 만들고, 어댑터와 레이아웃 매니저 설정
6. LayoutManager 설정

이 예제는 다음과 같은 Chat App의 화면을 만드는 것을 목표로 했다.

<center>
<img src="/assets/post-img/171206-capture1.JPG" width="40%" height="40%">
</center>
<br>

### 1. RecyclerView에서 사용할 데이터 정리
Chat 이라는 `Class` 파일을 만들어서 Chat의 구성요소를 name, message, photo로 설정한다.

```kotlin
/* Chat.kt */

class Chat(val name: String, val message: String, val photo: String)
/* photo 파라미터는 우선 String으로 선언하고, 이후에 코드를 통해 실제 이미지 파일과 데이터를 연동했다. */
```
<br>

그리고, 데이터를 하드코딩 하기 때문에 name-message-photo 형식을 띤 Chat 데이터를 여러개 가지고 있는 ArrayList 가 필요하다. DataService 라는 `Object` 파일에 `ArrayList<Chat>` 변수 chatData 생성한다.

```Kotlin
/* DataService.kt */

object DataService {
    val chatData = arrayListOf(
        Chat("카드1", "0월 0일 999원 사용.", "cardimg1"),
        Chat("영화관1", "영화 예매완료.", "movieimg1"),
        Chat("친구1", "안녕?", "friendimg1"),
        Chat("택배1", "배송이 완료되었습니다.", "deliveryimg1"),
        Chat("카드2", "0월 0일 999원 사용.", "cardimg1"),
        Chat("영화관2", "영화 예매완료.", "movieimg1"),
        Chat("친구2", "안녕?", "friendimg1"),
        Chat("택배2", "배송이 완료되었습니다.", "deliveryimg1")
    )
}
```
클래스를 만든 후, drawable 폴더에는 cardimg1.png 등 photo에 사용할 이미지 파일을 추가했다.
<br>

### 2. Gradle에서 RecyclerView 컴파일 추가
RecyclerView는 기본 API에 제공되어 있지 않기 때문에, Support Library 추가를 해야 사용할 수 있다. 좌측의 `Gradle Scrpits` - `build.gradle (Module: app)`경로로 파일을 열어서 `dependencies` 안에 컴파일을 추가해야 한다.

```
compile 'com.android.support:recyclerview-v7:26.1.0'
```

직접 위의 코드를 입력해서 컴파일 할 수도 있고, **Alt + Enter** 를 통해 library dependency를 찾아서 추가할 수도 있다.

gradle 파일을 수정하고 나면 우측 상단의 **Sync Now** 를 눌러서 설정을 업데이트 한다.
<br>

### 3. RecyclerView를 사용할 레이아웃의 xml 파일에 RecyclerView 추가
Gradle에 제대로 추가했다면, `<android.support.v7.widget.RecyclerView>` 경로를 통해 RecyclerView를 레이아웃에 추가할 수 있다. 레이아웃 미리보기에서는 RecyclerView 안에 Item 0, Item 1 과 같은 텍스트 리스트가 수직으로 나열된다.

여기서는 activity_main.xml 에 RecyclerView를 추가했다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.yenachoi.study_something.MainActivity">

    <TextView
        android:id="@+id/mainTopText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:text="@string/chat_list"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <android.support.v7.widget.RecyclerView
        android:id="@+id/myRecyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/mainTopText" />

</android.support.constraint.ConstraintLayout>
```
<br>

### 4. Custom RecyclerView item 레이아웃 파일 생성
다음으로 RecyclerView 안에 들어가는 `item`을 만든다. 이 예제의 경우, chat_list_item.xml 파일을 만들어서 왼쪽에 사진, 상단에 이름, 하단에 메세지가 오도록 커스텀했다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:app="http://schemas.android.com/apk/res-auto"
  xmlns:tools="http://schemas.android.com/tools"
  android:layout_width="match_parent"
  android:layout_height="72dp">

  <ImageView
      android:id="@+id/chatPhotoImg"
      android:layout_width="56dp"
      android:layout_height="56dp"
      android:layout_marginBottom="8dp"
      android:layout_marginStart="8dp"
      android:layout_marginTop="8dp"
      android:contentDescription="@string/chatphotoimg"
      app:layout_constraintBottom_toBottomOf="parent"
      app:layout_constraintStart_toStartOf="parent"
      app:layout_constraintTop_toTopOf="parent"
      app:srcCompat="@mipmap/ic_launcher_round" />

  <TextView
      android:id="@+id/chatNameTv"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_marginStart="16dp"
      android:textSize="20sp"
      android:textStyle="bold"
      app:layout_constraintStart_toEndOf="@+id/chatPhotoImg"
      app:layout_constraintTop_toTopOf="@+id/chatPhotoImg"
      tools:text="Name" />

  <TextView
      android:id="@+id/chatMessageTv"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:textSize="16sp"
      app:layout_constraintBottom_toBottomOf="@+id/chatPhotoImg"
      app:layout_constraintStart_toStartOf="@+id/chatNameTv"
      tools:text="This is contents of chat." />
</android.support.constraint.ConstraintLayout>
```
채팅창 하나의 높이는 ConstraintLayout의 height 영역에서 72dp로 설정했다. 그리고 각 View에 id를 부여하고, ImageView의 기본값으로는 안드로이드 기본 제공 이미지를 사용했다.
<br>

### 5. Adapter 만들고, 어댑터 설정
RecyclerView와 그곳에 들어갈 각각의 item, 연동할 데이터까지 설정했으면 `Adapter`를 만들어야 한다. 사진, 이름, 채팅 내용 등 어느 요소를 어느 View에 넣을 것인지 연결해주는 것이 Adpater의 역할이다. 리사이클러뷰의 어댑터는 기본 ~~BaseAdapter()~~가 아닌, RecyclerView.Adapter 를 extend 한다.

```Kotlin
/* ChatRecyclerAdapter.kt */

class ChatRecyclerAdapter(val context: Context, val chatData: ArrayList<Chat>) : RecyclerView.Adapter<ChatRecyclerAdapter.Holder>() {

    override fun onCreateViewHolder(parent: ViewGroup?, viewType: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.chat_list_item, parent, false)
        return Holder(view)
        /* 화면을 최초 로딩하여 만들어진 View가 없는 경우, xml파일을 inflate하여 ViewHolder를 생성한다. */
    }

    override fun getItemCount(): Int {
        return chatData.size
        /* RecyclerView로 만들어지는 item의 총 개수를 반환한다. */
    }

    override fun onBindViewHolder(holder: Holder?, position: Int) {
        holder?.bind(chatData[position], context)
        /*  아래에서 정의한 Holder 클래스에서 임의의 함수 bind를 만들고, 이곳에서 불러와 사용한다.
         *  위의 onCreateViewHolder에서 만들어둔 holder에 chatData 의 데이터를 각각 넣는 역할. */
    }


    inner class Holder(itemView: View?) : RecyclerView.ViewHolder(itemView) {
        val chatName = itemView?.findViewById<TextView>(R.id.chatNameTv)
        val chatMessage = itemView?.findViewById<TextView>(R.id.chatMessageTv)
        val chatPhoto = itemView?.findViewById<ImageView>(R.id.chatPhotoImg)
        /* Chat App의 한 줄에 해당하는 Holder를 만들고, findViewById로 각각의 View 참조를 한다. */

        fun bind (chat: Chat, context: Context) {
            val resourceId = context.resources.getIdentifier(chat.photo, "drawable", context.packageName)
            chatPhoto?.setImageResource(resourceId)
            chatName?.text = chat.name
            chatMessage?.text = chat.message
            /* bind 함수는 ViewHolder와 실제 데이터를 연동하는 역할을 한다. onBindViewHolder에서 사용. */
        }
    }
}
```
<br>

Adapter를 만들었으면, RecyclerView를 불러올 액티비티로 돌아가서 Adapter 연동을 한다.

```kotlin
/* MainActivity.kt */

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        var chatRecyclerAdapter = ChatRecyclerAdapter(this, DataService.chatData)
        myRecyclerView.adapter = chatRecyclerAdapter
    }
}
```
<br>

### 6. LayoutManager 설정
ListView Adapter와는 다르게, RecyclerView Adapter에서는 **레이아웃 매니저 (LayoutManager)** 를 추가로 설정해주어야 한다.

LayoutManager는 RecyclerView의 각 item들을 배치하고, item이 더이상 보이지 않을 때 재사용할 것인지 결정하는 역할을 한다. item을 재사용할 떄, LayoutManager는 Adapter에게 view의 요소를 다른 데이터로 대체할 것인지 물어본다. LayoutManager 사용을 통해 불필요한 findViewById를 수행하지 않아도 되고, 앱 성능을 향상시킬 수 있다.

기본적으로 안드로이드에서 3가지의 LayoutManager 라이브러리를 지원한다.
  - LinearLayoutManager
  - GridLayoutManager
  - StaggeredGridLayoutManager

<br>

이 외에도 사용자가 추상 클래스를 확장하여 임의의 레이아웃을 지정할 수도 있다.
이 예제에서는 LinearLayoutManager를 사용했다. RecyclerView를 불러올 액티비티에 LayoutManager를 추가한다.

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        var chatRecyclerAdapter = ChatRecyclerAdapter(this, DataService.chatData)
        myRecyclerView.adapter = chatRecyclerAdapter

        val myLayoutManager = LinearLayoutManager(this)
        myRecyclerView.layoutManager = myLayoutManager
        myRecyclerView.setHasFixedSize(true)
    }
}
```
<br>

마지막으로 recyclerView에 `setHasFixedSize` 옵션에 true 값을 준다. 그 이유는 item이 추가되거나 삭제될 때 RecyclerView의 크기가 변경될 수도 있고, 그렇게 되면 계층 구조의 다른 View 크기가 변경될 가능성이 있기 때문이다. 특히 item이 자주 추가/삭제되면 오류가 날 수도 있기에 setHasFixedSize true를 설정한다.
<br><br>

### References
- https://developer.android.com/guide/topics/ui/layout/recyclerview.html#structure
- https://stackoverflow.com/questions/28709220/understanding-recyclerview-sethasfixedsize
