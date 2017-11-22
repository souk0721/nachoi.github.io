---
layout: post
title:  "Kotlin Nullability 코틀린에서의 null"
author: Yena Choi
categories: studynote
tags: [kotlin, nullability, null]
---

## Nullability
코틀린에서는 변수 선언과 동시에 값을 입력해야 한다. 하지만, 아무데나 `= null` 값을 지정할 수 없다.

```java
var name : String = "Nachoi"
name = null   // 이렇게 null을 지정할 수 **없다!**
```

null값으로 지정하고 싶으면 변수형 뒤에 `?`를 붙인다.
```java
var name : String? = "Nachoi"
name = null   // 가능
```
<br><br>

### Null 여부 확인
**null 상태에서 메소드를 호출하면 `NullPointerException` 위험 있으므로 , 체크!**

1. `if` 문으로 체크
  ```java
  var length = 0
  if (name  != null) {
      length = name.length
  } else {
      length = -1
  }
  // 혹은, 아래와 같이 한줄로 가능.
  var length = if (name != null) name.length else -1
  ```
  <br>
2. Safe Call Operator `?`
  ```java
  println(name?.length)
  // null일 경우 null 출력. 변수명 뒤에 `?` 를 붙일 수 있다!
  ```
  <br>
3. Elvis Operator `?:`
  ```java
  var length = name?.length ?: -1
  // `?:` 를 붙여서 오류 상황에 대한 default값 지정.
  ```
  <br>
4. force (`!!`)   
변수 안의 값에 상관없이 메소드 실행. 따라서 오류가 날 수 있으니, 100% 확실한 상황에서만 사용.
```java
println(name!!.length)  //null일 경우 오류
```
