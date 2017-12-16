---
layout: post
title:  "[Android] 안드로이드 키보드 숨기기"
author: Yena Choi
categories: studynote
tags: [android]
---

EditText에 내용을 쓰고 버튼을 누르는 등 액션을 할때, 하단의 키보드가 자동으로 사라지게 하는 것이 사용하기 편하다.

`InputMethodManager`를 이용해서 System Service에 접근해 제어가 가능하다.

### java에서 하기

```java
View view = this.getCurrentFocus();
if (view != null) {  
    InputMethodManager imm = (InputMethodManager)getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.hideSoftInputFromWindow(view.getWindowToken(), 0);
}
```
<br><br>

### kotlin에서 하기

```kotlin
fun View.hideKeyboard() {
    val imm = context.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
    imm.hideSoftInputFromWindow(windowToken, 0)
}
```
<br><br>


### References
- https://stackoverflow.com/questions/1109022/close-hide-the-android-soft-keyboard
- https://stackoverflow.com/questions/41790357/close-hide-the-android-soft-keyboard-with-kotlin
