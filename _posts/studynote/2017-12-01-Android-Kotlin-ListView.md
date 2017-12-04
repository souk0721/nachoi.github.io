---
layout: post
title:  "[Android][Kotlin] 코틀린 ListView 리스트뷰"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view]
---
ListView가 안드로이드 공부 하면서 특히 어려운 부분인 것 같다. 예제 따라하기만 하다보니 머리가 아파서, 직접 예제를 만들고 한 단계씩 정리해보았다.

## ListView
`ListView`는 스크롤 가능한 항목을 나타낼 때 사용되는 뷰 그룹이다. ListView에 먼저 View를 배치한 다음, 데이터가 저장된 곳에서 데이터를 View의 형식에 맞게 변환하여 가져온다.

안드로이드 리스트뷰를 사용하기 위해 준비해야 할 것을 정리하자면 다음과 같다.
1. ListView에서 사용할 데이터 정리 (클래스 파일 생성하는 것이 편리하다.)
2. ListView를 사용할 레이아웃의 xml 파일에 ListView Containers 추가
3. Custom ListView 레이아웃 파일 만들고 적용하기.
4. `Adapter` 만들고 설정하기.

위와 같이 순서를 정하고, 리스트뷰 레이아웃 예제는 간단한 채팅 리스트 화면으로 만들었다. ~~썰렁한 UI는 무시하자~~

<center>
<img src="/assets/post-img/171204-capture1.JPG" width="30%" height="30%">
</center>


### 1. ListView에 사용할 데이터 정리하기
우선은 하드코딩된 데이터로 연습해보기로 한다. Chat 이라는 `Class` 파일을 만들어서 Chat의 구성요소를 name, message, photo로 설정했다.

```kotlin
/* Chat.kt */

class Chat(val name: String, val message: String, val photo: String)
```

그리고, name-message-photo 형식을 띤 Chat 데이터를 여러개 가지고 있는 ArrayList가 필요했다. DataService 라는 `Object` 파일에 `ArrayList<Chat>` 변수 chatData 를 초기화했다.

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
<br>

### 2. ListView를 사용할 레이아웃의 xml 파일에 리스트뷰 만들기
메인 액티비티에서 작업했기 때문에 activity_main.xml에서 Palette - Containers - ListView 를 배치하고, id를 myListView로 지정했다.

<center>
<img src="/assets/post-img/171204-capture2.JPG" width="70%" height="70%">
</center>


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

  <ListView
      android:id="@+id/myListView"
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

### 3. Custom ListView 레이아웃 파일 만들고 적용하기.
다음으로 ListView 안에 들어가는 `item`을 만들었다. 이 예제의 경우, chat_list_item.xml 파일을 만들어서 왼쪽에 사진, 상단에 이름, 하단에 메세지가 오도록 커스텀했다.

<center>
<img src="/assets/post-img/171204-capture3.JPG" width="70%" height="70%">
</center>


```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:app="http://schemas.android.com/apk/res-auto"
  xmlns:tools="http://schemas.android.com/tools"
  android:layout_width="match_parent"
  android:layout_height="72dp">

  <ImageView
      android:id="@+id/chatPhotoImg"
      android:layout_width="56dp"
      android:layout_height="56dp"
      android:layout_marginBottom="8dp"
      android:layout_marginStart="16dp"
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
채팅창 하나의 높이는 ConstraintLayout의 height 영역에서 72dp로 설정했다. 그리고 각 View에 id를 부여하고, ImageView의 내용은 안드로이드 기본 제공 이미지를 사용했다.
<br>

### 4. Adapter 만들고 설정하기.
ListView, ListView의 각 item, 그리고 거기에 넣을 데이터까지 설정했으면 `Adapter`를 만들어야 한다. 사진, 이름, 채팅 내용 등 어느 요소를 어느 View에 넣을 것인지 연결해주는 것이 Adpater의 역할이다.

임의로 Custom한 Adpater Class는 `BaseAdapter()`의 확장(extend)이다. 그렇기에 기본적으로 4개의 메소드를 implement 해야한다. 아래와 같이 입력한 후에, 클래스 이름(ChatAdpater)에 커서를 놓고 Alt+Enter를 누르면 자동으로 필수 메소드를 Implement를 할 수 있다.

```Kotlin
class ChatAdapter (val context: Context, val chatData: ArrayList<Chat>) : BaseAdapter()
```
  - **getView(Int, View, ViewGroup)** : xml 파일의 View와 데이터를 연결하는 핵심 역할을 하는 메소드이다.
  - **getItem(Int)** : 해당 위치의 item을 메소드이다. Int 형식으로 된 position을 파라미터로 갖는다. 예를 들어 3번째 줄에서 했던 채팅을 선택하고 싶으면 코드에서 getItem(2)과 같이 쓸 수 있을 것이다.
  - **getItemId(Int)** : 해당 위치의 item id를 반환하는 메소드이다. 이 예제에서는 사용하지 않아서 0을 반환하도록 설정했다.
  - **getCount()** : ListView에 속한 item의 전체 수를 반환한다.

이외에도 데이터 추가, 삭제나 변경 등의 기능을 하는 메소드도 추가할 수 있지만, 기본적으로 override 하는 메소드는 위의 4개이다.

```Kotlin
class ChatAdapter (val context: Context, val chatData: ArrayList<Chat>) : BaseAdapter() {

    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
      val chatView: View = LayoutInflater.from(context).inflate(R.layout.chat_list_item, null)
      /* LayoutInflater는 item을 Adapter에서 사용할 View로 부풀려주는(inflate) 역할을 한다.
         채팅 한 줄에 해당하는 뷰를 chatView라는 이름의 View 변수에 담았다. */

      val chatName : TextView = chatView.findViewById(R.id.chatNameTv)
      val chatMessage : TextView = chatView.findViewById(R.id.chatMessageTv)
      val chatPhoto : ImageView = chatView.findViewById(R.id.chatPhotoImg)
      /* 위에서 생성된 chatView를 Custom Item과 연결하는 과정이다.
         2개의 TextView와 1개의 ImageView 변수를 만들어 각각의 View와 연결한다. */

      val chat = chatData[position]
      /* DataService 파일에서 만들었던 chatData 를 하나씩 담는 View 변수 chat을 초기화한다. */

      val resourceId = context.resources.getIdentifier(chat.photo, "drawable", context.packageName)
      chatPhoto.setImageResource(resourceId)
      chatName.text = chat.name
      chatMessage.text = chat.message
      /* drawable에 저장한 png 파일명을 가져오고, chatView의 각 View 요소를 실제 데이터로 채운다. */

      return chatView
    }

    override fun getItem(position: Int): Any {
        return chatData[position]
    }

    override fun getItemId(position: Int): Long {
        return 0
    }

    override fun getCount(): Int {
        return chatData.size
    }
}
```
<br>
정성들여 Adapter를 셋팅한 후, MainActivity.kt로 와서 Adapter를 초기화해야 한다. 그 후 ListView와 Adapter를 연결한다.

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        var chatAdapter = ChatAdapter(this, DataService.chatData)
        myListView.adapter = chatAdapter
    }
}
```

### ListView 의 단점
ListView는 Adapter를 통해 `getView` 메소드를 호출하여 View를 만든다. 최초로 화면을 로딩한 후에도 스크롤을 움직이는 등 액션을 취하면 그 때마다 `findViewById`를 통해 convertView에 들어갈 요소를 찾는다. 스크롤 할 때마다 View를 찾으면 리소스를 많이 사용하게 되고, 속도가 느려진다. 그 이유 때문에, `ViewHolder`와 `RecyclerView`를 사용하는 것이 권장된다.  
