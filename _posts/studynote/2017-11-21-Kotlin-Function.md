---
layout: post
title:  "[Kotlin] Function 코틀린 함수"
author: Yena Choi
categories: studynote
tags: [kotlin, function]
---

## Function 함수
코틀린에서 함수를 정의할 때에는 `fun` 사용.
반환타입을 지정할 수 있으며, 지정하지 않으면 타입을 추론해서 반환한다.
```Kotlin
fun calculate(num1 : Int, num2: Int) : Int {
    // 함수명  파라미터1   파라미터2     반환타입
    return num1 + num2
}

calculate(8,3) // Int 11 반환
```

또한 default 파라미터를 지정할 수도 있다.

```Kotlin
fun myCoffee(menu: String = "Americano") {
  println(menu)
}

myCoffee() // default 값인 Americano 출력
myCoffee("Latte") // 파라미터로 입력한 Latte 출력

```

<br><br>

### Conditional Logic - 조건문
코틀린 조건문으로는 `if`, `when`, `for`, `while` 등을 사용할 수 있다.

#### if
조건이 맞을 경우 실행, 아닐 경우 다음 else 실행.
```Kotlin
if (a > b) {
    println(a가 b보다 크다)
} else {
    println(a는 b보다 작거나 같다)
}
```
<br>
#### When
조건에 따라 결과가 달라진다. (다른 언어에서의 `switch` 비슷)
```Kotlin
var case = 2
when (case) {
    1 -> println("조건이 1일때 이거 실행")
    2 -> println("조건이 2일때 이거 실행") // 이 메세지 프린트
  else -> println("나머지는 이거 실행")
}
```

같은 실행결과를 가질 경우, 콤마 `,`를 통해 조건을 묶을 수 있다.
```Kotlin
var case = 2
when (case) {
    1, 2 -> println("1 혹은 2이다.") // 이 메세지 프린트
    else -> println("나머지")
}
```

`in` 혹은 `!in` 을 통하여 숫자 범위를 지정할 수도 있다.
```Kotlin
var case = 2
when (case) {
    in 0..3 -> println("조건 범위 내에 있다") // 이 메세지 프린트
    !in  0..3 -> println("조건 범위 밖에 있다.")
    else -> println("해당없음")
}
```

`is` 로는 값이 특정 유형인지 판단할 수 있다.

```Kotlin
fun naming(input: Any) = when (input) {
    is String -> println("이름을 $input 님으로 지정합니다.")
    else -> println("문자 형식으로 입력해주세요.")
}

// input의 타입을 추론해서, 해당 유형일 경우 조건 실행한다.
naming("나최")
naming(100)
```
<br>
#### for

루프 안에 있는 것을 반복한다. 다른 언어에서의 for 문과 유사하다.

```Kotlin
for (num: Int in 0..4) {
    println(num)
    // 0 부터 4까지 총 5줄이 출력된다.
}
```

`arrayList` 나 `hashMap` 등의 값을 출력하는 방법은 [Kotlin Collections 링크](Kotlin-Collections.html) 참조.
<br>
#### while

역시 다른 언어에서의 while 문과 유사하다.

```Kotlin
var num = 0
while (num < 5) {
    println(num)
    num++
    // 0 부터 4까지 총 5줄이 출력된다.
}
```

`do..while` 형식으로도 사용 가능하다.

```Kotlin
var num = 0
do {
    println(num)
    num++
} while (num < 5)
```


### Reference
[https://kotlinlang.org/docs/reference/basic-syntax.html](https://kotlinlang.org/docs/reference/basic-syntax.html)
[https://kotlinlang.org/docs/reference/control-flow.html](https://kotlinlang.org/docs/reference/control-flow.html)
