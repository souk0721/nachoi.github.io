---
layout: post
title:  "[Android][Kotlin] 코틀린 RecyclerView(2)"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view, lambda]
---

[RecyclerView(1) 포스트](/studynote/2017/12/06/Android-Kotlin-RecyclerView1.html)에서는 리사이클러뷰를 통해 데이터를 출력하는 과정까지 정리했다. 이번 포스트에서는 코틀린 프로젝트에서 각 item을 클릭했을 때에 이벤트를 처리하는 방법을 정리했다.

## itemClick Listener
ListView에서는 메인 액티비티 코드에서 `setOnClickListener`를 통해 각 item별 클릭 처리를 할 수 있었지만, RecyclerView에서는 별도로 클릭 리스너를 생성해주어야 한다. 이 처리는 `Adapter`에서 람다를 통해 설정해주었다.

지난 포스트에 이어서, 예제는 마찬가지로 채팅 수신함 목록을 나타내는 화면이다.

<center>
<img src="/assets/post-img/171206-capture1.JPG" width="40%" height="40%">
</center>
<br>

아래 코드는 Chat 목록 화면을 나타낼 RecyclerView의 Adapter이다. 맨 윗줄에서 어댑터에 대한 constructor로 **context** 와, 커스텀클래스 Chat의 변수를 모아둔 ArrayList인 **chatData** 을 사용한다.


```kotlin
class ChatRecyclerAdapter(val context: Context, val chatData: ArrayList<Chat>) : RecyclerView.Adapter<ChatRecyclerAdapter.Holder>() {

    override fun onCreateViewHolder(parent: ViewGroup?, viewType: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.chat_list_item, parent, false)
        return Holder(view)
    }

    override fun getItemCount(): Int {
        return chatData.size
    }

    override fun onBindViewHolder(holder: Holder?, position: Int) {
        holder?.bind(chatData[position], context)
    }


    inner class Holder(itemView: View?) : RecyclerView.ViewHolder(itemView) {
        val chatName = itemView?.findViewById<TextView>(R.id.chatNameTv)
        val chatMessage = itemView?.findViewById<TextView>(R.id.chatMessageTv)
        val chatPhoto = itemView?.findViewById<ImageView>(R.id.chatPhotoImg)

        fun bind (chat: Chat, context: Context) {
            val resourceId = context.resources.getIdentifier(chat.photo, "drawable", context.packageName)
            chatPhoto?.setImageResource(resourceId)
            chatName?.text = chat.name
            chatMessage?.text = chat.message
        }
    }
}
```
<br>

코틀린에서는 [람다](/studynote/2017-11-22-Kotlin-Lambda.html)의 이용이 가능하다. **Chat 을 파라미터로 받아서, 아무것도 반환하지 않는** 파라미터를 람다식으로 나타낼 것이다. 그 생김새는 아래와 같다.

```Kotlin
val itemClick : (Chat) -> Unit
```
<br>

Chat을 파라미터로 사용하고, 아무것도 반환하지 않는(Unit) 기능을 하는 함수 자체를 `itemClick` 변수에 넣는다. 그 itemClick 변수를 Adapter의 파라미터로 넣고, 어댑터 내에서 setOnClickListener 기능을 설정할 때 `(Chat) -> Unit`에 해당하는 함수 자체를 하나의 변수로 꺼내 쓸 수 있는 것이다.

```kotlin
class ChatRecyclerAdapter(val context: Context, val chatData: ArrayList<Chat>, val itemClick: (Chat) -> Unit) : RecyclerView.Adapter<ChatRecyclerAdapter.Holder>() {
    /* (1) Adapter의 파라미터에 람다식 itemClick을 넣는다. */

    override fun onCreateViewHolder(parent: ViewGroup?, viewType: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.chat_list_item, parent, false)
        return Holder(view, itemClick)
        /* (4) Holder의 파라미터가 하나 더 추가됐으므로, 이곳에도 추가해준다. */
    }

    override fun getItemCount(): Int {
        return chatData.size
    }

    override fun onBindViewHolder(holder: Holder?, position: Int) {
        holder?.bind(chatData[position], context)
    }


    inner class Holder(itemView: View?, val itemClick: (Chat) -> Unit) : RecyclerView.ViewHolder(itemView) {
        /* (2) Holder에서 클릭에 대한 처리를 할 것이므로, Holder의 파라미터에 람다식 itemClick을 넣는다. */
        val chatName = itemView?.findViewById<TextView>(R.id.chatNameTv)
        val chatMessage = itemView?.findViewById<TextView>(R.id.chatMessageTv)
        val chatPhoto = itemView?.findViewById<ImageView>(R.id.chatPhotoImg)

        fun bind (chat: Chat, context: Context) {
            val resourceId = context.resources.getIdentifier(chat.photo, "drawable", context.packageName)
            chatPhoto?.setImageResource(resourceId)
            chatName?.text = chat.name
            chatMessage?.text = chat.message
            itemView.setOnClickListener { itemClick(chat) }
            /* (3) itemView가 클릭됐을 때 처리할 일을 itemClick으로 설정한다.
             * (Chat) -> Unit 에 대한 함수는 나중에 MainActivity.kt 클래스에서 작성한다. */
        }
    }
}
```
<br>

Adapter에서 itemClick에 대한 설정이 끝났으면 MainActivity로 돌아와서 람다에서 실행할 코드를 입력한다. 실제 Chat App이라면 다른 액션을 취하겠지만, 이 예제에서는 간단하게 Toast 메세지 띄우는 것으로 대신했다.

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val chatRecyclerAdapter = ChatRecyclerAdapter(this, DataService.chatData) { chat ->
            Toast.makeText(this, "${chat.name}님이 보낸 메시지 - ${chat.message}", Toast.LENGTH_SHORT).show()
        }
        /* (5) Adapter의 파라미터를 추가했으므로, (context, chatData)에서 (context, chatData, Lambda)가 되어야한다.
         * 코틀린에서 맨 마지막 파라미터가 람다식일 경우, 코드의 가독성을 위해서 괄호() 뒤로 람다식을 빼고 대괄호{}로 묶는다.
         * {(Chat) -> Unit} 에 대한 내용을 Toast를 실행하는 것으로 만들었다. */

        myRecyclerView.adapter = chatRecyclerAdapter
        val myLayoutManager = LinearLayoutManager(this)
        myRecyclerView.layoutManager = myLayoutManager
        myRecyclerView.setHasFixedSize(true)
    }
}
```
<br>

클릭 리스너가 완성됐다. 빌드하여 실행 후, 채팅 목록 중 하나를 클릭하면 다음과 같이 Toast 메시지가 나온다.

<center>
<img src="/assets/post-img/171207-capture1.JPG" width="40%" height="40%">
</center>
<br>
