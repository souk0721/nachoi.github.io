---
layout: post
title:  "Kotlin Class & Inheritance 코틀린 클래스와 상속"
author: Yena Choi
categories: studynote
tags: [kotlin, class, inheritance]
---

## Class
코틀린에서 클래스 생성할 때는, constructor 에서 바로 변수 선언 가능하다.

```java
class Car constructor(make: String, model: String) {
   val make = make
   val model = model
}
// 위의 식을 아래와 같이 생략할 수 있다.
class Car (val make: String, val model: String)
```

클래스 생성할때 기본적으로 final로 생성된다. 따라서 상속하려면 `open`을 붙이고 사용.
```java
open class Vehicle (val make: String, val model: String) {
   fun accelerate() {
      println("vroom vroom")
   }
}
```
<br>
## inheritance
**상속할 때에, parent class에 파라미터가 있다면 상속할 때 지정해줘야 한다.**

```java
class Car(make: String, model: String, var color: String) : Vehicle(make, model)
// Vehicle의 make,model 변수에 Car의 make,model값 입력
```

children class에서 parent class의 메소드를 `Override`하면, 메소드 재설정할 수 있다.

```java
override fun accelerate() {
   println("method changed!")
   super.accelerate() // 기존에 작동하던 메소드 기능. 그대로 두거나 삭제할 수 있다.
}
```
