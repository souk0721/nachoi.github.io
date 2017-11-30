---
layout: post
title:  "[Android] Intent 인텐트로 값 전달하기"
date:   2017-11-06
author: Yena Choi
categories: studynote
tags: [android, intent]
---

### Intent 인텐트로 값 전달하기
- intent에서 putExtra 할때 key의 값을 intent.EXTRA_TEXT 등등 intent에서 꺼내올 수 있다.
- default 변수명이 더 안 헷갈림. 그리고 getExtra할때 그냥 꺼내는 게 아니라 `if(intent.hasExtra)` 로 데이터가 있는지 확인.
- 암시적 intent 사용할 때에도 사용할만한 앱이 있을 경우에만 실행

```java
if (intent.resolveActivity(getPackagerManager()) != null) {
 startActivity(intent);
}
```


#### Intent로 Share하기
- 공유할 때는 아래와 같은 폼을 사용한다.

```java
ShareCompat.IntentBuilder.from(this)
 .setChooserTitle("제목") // 딱히 제목이 공유되는건 아니더라.
 .setType("text/plain")
 .setText("공유될 텍스트");
 .startChooser(); //이거 안하면 안됨! 왠지 나중에 까먹을것같다.
```
- intent로 getExtra도 그냥 하는게 아니라,   

  **1) intent가 null인지 먼저 물어보고,**   
  **2) intent.hasExtra가 null인지 물어본 후에 getExtra**
  - 나는 언젠가 이걸 빼먹고 에러를 낼 것 같다. 그러니까 bold처리
