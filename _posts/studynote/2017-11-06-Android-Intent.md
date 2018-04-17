---
layout: post
title:  "[Android] Intent 인텐트로 값 전달하기"
date:   2017-11-06
author: Yena Choi
categories: studynote
tags: [android, activity, intent]
---

코틀린의 경우, 다음 링크 참조
- [\[Android\]\[Kotlin\] 액티비티 전환하기](/studynote/2017/11/27/Android-Kotlin-Activity.html)
- [\[Android\]\[Kotlin\] PutExtra 데이터 전달](/studynote/2017/11/28/Android-Kotlin-putExtra.html)


## Intent 로 Activity 전환하기
Java에서 다른 액티비티로 전환 할 때에는 인텐트를 다음과 같이 생성한다.

```Java
/* MainActivity.java */

Intent myIntent = new Intent(this, secondActivity.class);
/* 첫 번째에는 현재의 context 즉 'this', 두 번째에는 Activity.class 입력 */
```

예제에서는 인텐트 이름을 그냥 intent나 i로 쓰는 경우를 많이 보았는데, 나의 경우는 언제 쓰는 인텐트인지 헷갈릴까봐 myIntent처럼 정해주는 것을 선호한다.


다음 액티비티로 가기 위해 인텐트를 수행할 버튼을 만들고, intent를 통해 액티비티를 시작한다.

```Java
Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(myIntent);
            }
        });
```

버튼을 누르면 `startActivity(myIntent)`가 수행되며 `MainActivity` 위에 `SecondActivity`가 로드된다.

<br><br>

## Intent 인텐트로 값 전달하기
`MainActivity`에서 `SecondActivity`로 간단한 자료형(Int, String, Boolean 등)의 데이터를 intent에 포함하여 전달할 수 있다. 인텐트를 정의한 후에 `putExtra`를 통해 값을 담을 수 있다.

<center>
  <img src="/assets/post-img/171106-01.jpg"><br><br>

여러 형태의 값을 put 할 수 있다.
<br>
</center>
<br>

`putExtra(Key, Value)`의 형태와 같이, String으로 된 키 이름을 첫 번째에 넣고 int, String 등의 전달할 값을 두 번째에 넣는다.

```Java
/* MainActivity.java */
Intent myIntent = new Intent(this, SecondActivity.class);
myIntent.putExtra("번호", 12345);
myIntent.putExtra("메시지", "이것이 메시지의 vaule입니다.");
/* 하나의 인텐트에 여러 형태, 여러 개의 extra 값을 담을 수 있다 */
```
<br>

그 후, `startActivity(myIntent)`를 실행하면 SecondActivity에서 `getIntent()`를 통해 새 인텐트를 만들고, `getExtra`로 값을 꺼내올 수 있다. 이때 입력한 자료값에 따라 `getStringExtra`, `getIntExtra` 등의 메소드를 사용한다.

```Java
/* SecondActivity.java */
Intent secondIntent = getIntent();
secondIntent.getIntExtra("번호", 0);
secondIntent.getStringExtra("메시지");
/* int의 경우 value에 default 값을 적어준다. */
```
<br>

만약 SecondActivity의 TextView를 MainActivity에서 보낸 String값으로 바꾸고 싶다면, SecondActivity에서 intent의 String 값을 받을 변수를 추가해주면 된다.

```Java
/* SecondActivity.java */
TextView tv = findViewById(R.id.textView);

Intent secondIntent = getIntent();
String message = secondIntent.getStringExtra("메시지");

tv.setText(message);
```
<br>

<center>
<img src="/assets/post-img/171106-02.jpg" width="40%" height="40%"><br><br>
<img src="/assets/post-img/171106-03.jpg" width="40%" height="40%">
</center>
<br>


<br>
### Key와 관련된 팁
- intent에서 putExtra 할때 key의 값으로 별도의 String을 지정하지 않아도, `Intent.EXTRA_TEXT` 등등 Intent Class에 포함된 키를 사용할 수 있다.

  ```Java
  /* MainActivity.java */
  myIntent.putExtra(Intent.EXTRA_TEXT, "String Value 입니다.");

  /* SecondActivity.java */
  secondIntent.getStringExtra(Intent.EXTRA_TEXT);
  ```
  <br>

- 인텐트에 포함된 값을 꺼내오는 액티비티에서, `getExtra` 할 때 `if(intent.hasExtra)` 로 데이터가 있는지 확인하면 TextView를 ""로 바꾸는 등의 실수를 줄일 수 있다.

  ```Java
  /* hasExtra(key) 검사를 안 하는 경우 */
  tv.setText(message);

  /* hasExtra(key)로 검사를 하고, 전달된 값이 있을 때에만 textView를 바꾸는 경우 */
  if (secondIntent.hasExtra("메시지")) {
          tv.setText(message);
  }
  ```
  <br

  `hasExtra`를 암시적 intent에 응용하면, 사용 할만한 앱이 있을 경우에만 실행한다.
  ```java
  if (intent.resolveActivity(getPackagerManager()) != null) {
   startActivity(intent);
  }
  ```

<br>

### Intent로 Share하기
공유할 때는 아래와 같은 폼을 사용한다.
```java
ShareCompat.IntentBuilder.from(this)
 .setChooserTitle("제목")
 .setType("text/plain")
 .setText("공유될 텍스트");
 .startChooser();
```
역시, intent로 getExtra도 그냥 하는게 아니라,   

  **1) intent가 null인지 먼저 물어보고,**   
  **2) intent.hasExtra가 null인지 물어본 후에 getExtra**

<br>
나는 언젠가 이걸 빼먹고 에러를 낼 것 같다. 그러니까 bold처리.
<br>
