---
layout: post
title:  "[Android] Clicked Item 클릭 처리"
author: Yena Choi
categories: studynote
tags: [android]
comments: true
---

## Clicked Item
RecycleView 같은 곳에서 클릭 이벤트 발생 시 적용되는 레이아웃.   

1. drawable에 파일 생성. (drawable - selector 의 root element)

2. 배경 색을 바꾸는 것으로 지정할 경우,

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">

<item android:drawable="@color/colorPrimaryLight" android:state_pressed="true"/>
<item android:drawable="@color/colorPrimaryLight" android:state_activated="true"/>
<item android:drawable="@color/colorPrimaryLight" android:state_selected="true"/>
// 위 3개는 "클릭" 상태의 값


<item android:drawable="@android:color/background_light" />
// default 값

</selector>
```
그후, 해당 레이아웃에 가서 background 설정.

```xml
android:background="@drawable/list_item_selector"
```
