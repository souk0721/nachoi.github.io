---
layout: post
title:  "[Android][Kotlin] NumberPicker 넘버피커 숫자 선택"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view]
---

앱을 만들면서 유저로부터 숫자를 입력받아야 할 때가 있다. 일일히 타이핑을 해야 하는 EditText보다 `NumberPicker` 넘버피커를 이용하면 더 쉽게 숫자를 선택할 수 있다.
<br><br>

### NumberPicker
넘버 피커는 안드로이드 스튜디오의 Palette - Advanced - NumberPicker를 통해 레이아웃에 추가할 수 있다.

<center>
  <img src="/assets/post-img18/180119-01.jpg" width="100%" height="100%"><br>
</center>
<br>

코틀린에서는 `findViewById` 할 필요가 없으므로, MainActivity에서 id를 입력해서 import 한다. 우선 넘버피커의 최소 값과 최대 값을 정한다. 이 예제에서는 activity_main.xml 레이아웃에서 넘버피커의 id를 mNumberPicker로 지정했다.

```Kotlin
/* MainActivity.kt */

mNumberPicker.minValue = 0
mNumberPicker.maxValue = 12
```
<br>
minVaule와 maxValue만 설정하고 실행하면 기본적인 NumberPicker가 생성된다.

<center>
  <img src="/assets/post-img18/180119-02.jpg" width="40%" height="40%"><br>
</center>
<br>

끝 숫자인 12 다음에 시작 숫자인 0으로 순환하지 않게 하는 옵션은 `wrapSelectorWheel` 이다. 이 값을 false로 설정하면 NumberPicker의 범위가 시작~끝으로 고정된다. 또, 스크린샷처럼 NumberPicker의 기본 설정은 EditText로 되어 있어서 스크롤 뿐만 아니라 키보드를 통해서도 변경이 가능하다. 이 기능은 `descendantFocusability` 값을 변경하는 것으로 해제할 수 있다.

```Kotlin
mNumberPicker.wrapSelectorWheel = false
mNumberPicker.descendantFocusability = NumberPicker.FOCUS_BLOCK_DESCENDANTS
```
<br><br>

#### setOnValueChangedListener
액티비티에 실시간으로 값을 반영할 때에는 `setOnValueChangedListener`를 사용할 수 있다. 넘버피커, 기존 Int, 변경된 Int를 통해 식을 작성할 수 있다. 하단에 mTextView라는 id를 가지는 텍스트뷰를 추가해서 예제를 작성해보았다.

```Kotlin
mNumberPicker.minValue = 0
mNumberPicker.maxValue = 12
mNumberPicker.wrapSelectorWheel = false
mNumberPicker.descendantFocusability = NumberPicker.FOCUS_BLOCK_DESCENDANTS

mTextView.text = ""

mNumberPicker.setOnValueChangedListener { numberPicker, i1, i2 ->
    /* i2가 변경될 때마다 실행할 when 조건문 */
    when {
        i2 < 6 ->  mTextView.text = "충분한 수면이 필요할 것 같아요."
        i2 in 6..9 -> mTextView.text = "건강한 수면 습관을 가지고 있어요."
        else -> mTextView.text = "잠을 줄이고 활동적인 하루를 계획해보세요."
    }
}
```
<br>

<center>
  <img src="/assets/post-img18/180119-03.jpg" width="40%" height="40%"><br>
</center>
<br>


### References
- https://developer.android.com/reference/android/widget/NumberPicker.html
- https://android--examples.blogspot.kr/2015/05/how-to-use-numberpicker-in-android.html
- https://stackoverflow.com/questions/4873196/dynamically-update-textview-in-android
- https://stackoverflow.com/questions/8854781/disable-soft-keyboard-on-numberpicker
