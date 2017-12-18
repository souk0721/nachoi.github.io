---
layout: post
title:  "[Android][Kotlin] SharedPreferences 데이터 영구 저장"
author: Yena Choi
categories: studynote
tags: [android, kotlin]
---

## SharedPreferences
데이터를 영구 저장하는 방법으로는 SharedPreferences(쉐어드프리퍼런스)가 있다. App에 포함되는 데이터 파일을 만들어서, 디바이스에 저장하는 방식이다. `(key, value)` 형태로 저장/로드한다. 또한 `.edit()` 에디터를 이용해야 데이터의 수정, 삭제 등의 액션이 가능하다. 주로 'editor'라는 변수명에 저장해서 사용한다.

`MainActivity`의 `onCreate`에서 쓸 수 있는 SharedPreferences 예제는 다음과 같다.


```Kotlin
val pref = this.getPreferences(0)
val editor = pref.edit()
/* context.getPreferences의 SharedPreferences 인스턴스를 저장.
 * (0은 (Context.MODE_PRIVATE)와 같음)
 * 에디터를 호출해 editor로 초기화. */

 mainEt.setText(pref.getString("MessageKey", ""))
 /* mainEt(EditText)의 텍스트를 "MessageKey"에 해당하는 vaule로 설정.
  * 값을 불러오지 못했을 경우, default vaule는 ""로 지정. */

 mainBtn.setOnClickListener {
     editor.putString("MessageKey", mainEt.text.toString()).apply()
     /* mainBtn(Button) 클릭하면 "MessageKey"에 해당하는 String 데이터를 mainEt에서 불러와 저장. */

     val msg = pref.getString("MessageKey", "")
     if (msg == "") {
         Toast.makeText(this, "텍스트가 초기화되었습니다.", Toast.LENGTH_LONG).show()
     } else {
         Toast.makeText(this, "저장됨: $msg", Toast.LENGTH_LONG).show()
     }
     /* 값 저장 후 Toast 출력 */
```
<br>

<center>
  <img src="/assets/post-img/171218-01.JPG" width="40%" height="40%">
    <br> 텍스트를 저장한 후, 앱을 종료후 재실행 하거나 다시 Build하면 EditText에 저장했던 텍스트가 표시된다. <br><br>
  <img src="/assets/post-img/171218-02.JPG" width="40%" height="40%">
    <br> 앱을 재실행한 모습. <br><br>
</center>
<br><br>



### References
- https://developer.android.com/training/data-storage/shared-preferences.html
- http://blog.teamtreehouse.com/making-sharedpreferences-easy-with-kotlin
- https://myprogrammingexperiments.wordpress.com/2017/06/04/manipulating-shared-prefs-with-kotlin-just-two-lines-of-code/
