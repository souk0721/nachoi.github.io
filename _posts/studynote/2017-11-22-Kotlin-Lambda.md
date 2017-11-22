---
layout: post
title:  "Kotlin Lambda 코틀린 람다"
author: Yena Choi
categories: studynote
tags: [kotlin, lambda]
---


## Lambda 람다
Function Literal, 즉 함수를 선언하지 않고 곧바로 식으로 전달돼서 표현된다.
- 파라미터는 `->` 왼쪽에 선언됨. (파라미터가 있다면) 식으로 전달됨.
- `->` 의 오른쪽에서는 `function` 작동.
- 람다 식은 중괄호 `{ }`로 시작하고 끝난다.

"변수 생성" - "메소드 생성" - "메소드 실행" 에 해당하는 기능의 요약판.

<br>
```java
fun sayHello (name: String) {
   println("Hello, $name!")
}
sayHello("Nachoi")
```

위와 같은 식을 아래처럼 표현 가능하다.
```java
val sayHello = { name: String -> println("Hello, $name!")}
sayHello("Nachoi")
```
<br>
### Lambda Parameter 람다식을 파라미터로 사용하기

람다식 자체를 function의 하나의 파라미터로 사용할 수도 있다.
```java
fun downloadData(url: String, completion: ()-> Unit) {
   completion()
}
/* completion: ()-> Unit 까지의 람다식이 하나의 파라미터이다.
  ()->Unit의 의미는 파라미터가 없고, Unit 즉 아무것도 반환되지 않는 function */

downloadData("myUrl.com", {
   println("completion이 완료돼야 이 글이 출력됩니다.")
   /* downloadData function의 두 파라미터인 String과 람다식 파라미터를 입력함. */
})

```

<br>
혹은, 람다식이 하나 이상의 파라미터를 가질 수 있다.
```java
fun downloadWeather (url: String, completion: (String) -> Unit) {
   val weatherData = "Cloudy, -3℃"
   /* url에서 데이터를 가져온 후, 아래의 String 변수 weatherData에 저장 */
   completion(weatherData)
}
```

<br>
마지막 파라미터가 람다식이면 중괄호 `{ }` 부분을 소괄호 밖으로 뺄 수 있다.  
그 파라미터가 한 개면, 생략 후 'it'을 사용하여 대체 가능하다.
```java
downloadWeather("weatherUrl.com") { weatherData ->
   println(weatherData)
}
```

```java
downloadWeather("weatherUrl.com") {
   println(it)
}
```
