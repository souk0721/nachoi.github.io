---
layout: post
title:  "[Kotlin] Random 코틀린 랜덤"
author: Yena Choi
categories: studynote
tags: [kotlin, function]
comments: true
---

코틀린에서 랜덤을 쓰기 위해서는 java.util을 import 해야한다. 안드로이드 스튜디오에서는 Alt + Enter로 import하면 java.util.* 전체가 임포트 된다.

```Kotlin
import java.util.Random

val random = Random()
val num = random.nextInt(5)
/* val num 변수에 0~4 사이의 무작위 Int 저장 */
```

0부터 카운트하기 때문에 입력한 정수 -1 값이 최대치라는 점에 주의해야 한다.

### function 만들어서 사용하기
두 수를 입력하면 그 사이의 Int를 출력하는 `function`을 만들어 사용할 수도 있다.

```Kotlin
import java.util.random

val random = Random()
fun rand(from: Int, to: Int) : Int {
    return random.nextInt(to - from) + from
}

println(rand(5,10))
/* 5,6,7,8,9 Int 중 하나 출력 */
```
<br>
