---
layout: post
title:  "[Android][Kotlin] 액티비티 전환하기"
author: Yena Choi
categories: studynote
tags: [android, kotlin, intent, activity]
---

## Code에 View 가져오기
코틀린에서는 더이상 자바처럼 ~~findViewById~~ 할 필요가 없다. 코드에 `view`의 `id`를 적으면 자동완성에 어느 layout의 view를 import 할 것인지 표시된다. 코드에서 메인 레이아웃에 있는 'nextBtn'라는 id의 버튼 View를 호출하면, 상단에 다음과 같이 import 된다.

`import kotlinx.android.synthetic.main.activity_main.*`

<br>


<br><br>

## Lambda 를 이용한 setOnClickListener
자바에서는 setOnClickListener 기능을 아래와 같이 `onClick`을 `override`해서 클릭했을 때의 행동을 입력해야 했다.

```java
button.setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v){
        doSomething();
    }
});
// java
```
<br>

Kotlin 에서는 람다식을 이용하여 다음과 같이 setOnClickListener 를 사용한다.
```java
button.setOnClickListener { doSomething() }
// kotlin
```
`{ doSomething() }` 람다식을 setOnClickListener의 파라미터로 받아서 바로 동작할 수 있게 만들어졌다. 보기에도, 쓰기에도 훨씬 간편하다.
<br><br>

## Intent로 액티비티 전환하기
액티비티 이동하기 위해서는 mainActivity에서 secondActivity로 이동하는 Intent를 만든다. 도착하는 액티비티를 입력할 때에는 ~~activity~~ 혹은 ~~activity.class~~가 아니라, **activity::class.java** 형식으로 입력한다.

```Kotlin
class MainActivity : AppCompatActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main)

      nextBtn.setOnClickListener {
         val nextIntent = Intent(this, secondActivity::class.java)
         startActivity(nextIntent)  
      }
   }
}
```

<br><br>

### Reference
- https://developer.android.com/kotlin/index.html
