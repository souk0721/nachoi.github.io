---
layout: post
title:  "[Android][Kotlin] 코틀린 ViewHolder 뷰홀더"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view]
---

## ViewHolder의 필요성
ListView에서 발견된 문제점은, 스크롤을 움직이는 등 View가 보이거나 사라지면 그 때마다 `findViewById`를 통해 convertView에 들어갈 요소를 찾는다는 점이었다. 스크롤 할 때마다 View를 찾으면 리소스를 많이 사용하게 되고, 속도가 느려진다. `ViewHolder`를 이용하면 이 View의 재활용(recycle)이 가능하다.

## ViewHolder 활용
ListView의 각 View와 실제 데이터를 매칭하는 것이 Adapter의 역할이다. 따라서 ViewHolder를 사용하려면 Adapter 내에 설정해야한다.

[이전 ListView 포스트](/studynote/2017/12/01/Android-Kotlin-ListView.html)에 이어, 채팅 화면을 만들기 위해 사용했던 커스텀 어댑터인 ChatAdapter 클래스를 다시 사용했다. ChatAdapter의 맨 끝에 `private class` 인 ViewHolder Class를 만들어 각 View요소를 null로 선언했다.

```Kotlin
class ChatAdapter (val context: Context, val chatData: ArrayList<Chat>) : BaseAdapter() {
    override fun getView(...)
    override ...
    ...

    private class ViewHolder {
      var chatName : TextView? = null
      var chatMessage: TextView? = null
      var chatPhoto : ImageView? = null
    }
}
```
<br>

이 ViewHolder는 `getView` 메소드를 override 할 때 사용한다. getView 설정을 할 때, **ViewHolder 안에 데이터가 아무것도 없으면 findVidwById 하여 ListView의 View를 생성하고, ViewHolder 안에 데이터가 있을 경우 이를 재활용** 하도록 설정한다.

ViewHolder까지 설정한 ChatAdapter의 최종 모습은 아래와 같다.

```Kotlin
class ChatAdapter (val context: Context, val chatData: ArrayList<Chat>) : BaseAdapter() {

    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
        val chatView : View
        val holder : ViewHolder

        if (convertView == null) {
            chatView = LayoutInflater.from(context).inflate(R.layout.chat_list_item, null)
            holder = ViewHolder()
            holder.chatName = chatView.findViewById(R.id.chatNameTv)
            holder.chatMessage = chatView.findViewById(R.id.chatMessageTv)
            holder.chatPhoto = chatView.findViewById(R.id.chatPhotoImg)

            chatView.tag = holder
            /* convertView가 null, 즉 최초로 화면을 실행할 때에
            ViewHolder에 각각의 TextView와 ImageView를 findVidwById로 설정.
            마지막에 태그를 holder로 설정한다. */

        } else {
            holder = convertView.tag as ViewHolder
            chatView = convertView
            /* 이미 만들어진 View가 있으므로, tag를 통해 불러와서 대체한다. */

        }

        val chat = chatData[position]

        val resourceId = context.resources.getIdentifier(chat.photo, "drawable", context.packageName)
        holder.chatPhoto?.setImageResource(resourceId)
        holder.chatName?.text = chat.name
        holder.chatMessage?.text = chat.message
        /* holder와 실제 데이터를 연결한다. null일 수 있으므로 변수에 '?'을 붙여 safe call 한다. */

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

    private class ViewHolder {
        var chatName : TextView? = null
        var chatMessage: TextView? = null
        var chatPhoto : ImageView? = null
    }

}
```
