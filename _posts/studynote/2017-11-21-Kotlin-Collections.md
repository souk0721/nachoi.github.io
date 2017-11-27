---
layout: post
title:  "Kotlin Collections 코틀린 컬렉션"
author: Yena Choi
categories: studynote
tags: [kotlin, collections, arraylist, hashmap]
---

## Collections
여러 데이터를 모아놓은 하나의 단위.

### listOf()
- 데이터가 일렬로 배열된 형태의 컬렉션.
- immutable - 변경 불가능하다.
- 변경 불가능한 변수 묶음. 다른 언어의 `Array` 와 유사하다.
- listOf()의 메소드
```Kotlin
myList.sorted()  //알파벳순 정렬
myList[n] //n번째 항목 반환
myList.first() || myList.last() //처음이나 끝 항목 반환
myList.contains(something)  //something을 포함하는지 확인
```
<br>
### ArrayList
- listOf 와 유사하지만 mutable - 포함된 데이터를 변경 가능하다.
- **arrayList 를 사용하려고 하면 여러 가지 선택지가 뜬다. 그 중 `(arrayListOf(vararg elements: T)(kotlin.collections))` 항목을 선택하여 import 한다.** (2017/11/21 기준)
- arrayList()의 메소드
```Kotlin
myArrayList.add() //맨 끝에 항목 추가
myArrayList.add(n,something) //n번째에 항목 추가
myArrayList.size() //List의 크기 반환
myArrayList.indexOf(something) //something 항목의 번호를 반환(0부터 시작)
myArrayList.remove(something) //something을 삭제
```
<br>
### mapOf
- `key` to `value` (키와 값) 을 가지는 형태의 데이터 컬렉션.
  - 예를 들어, 전화번호부에서 '이름'을 선택하면 자동으로 그 사람의 '번호'로 연결되는 형태와 유사하다.
- immutable - 변경 불가능하다.
- **mapOf 를 사용하려고 할 때 여러 선택지가 뜬다. import 할 때 `(varag pairs: pair<K,V>) (kotlin.collections)` 를 선택한다.**
- mapOf()의 메소드  

  ```Kotlin
  val myMap = mapOf("myNickname" to "Nachoi", "myBlogUrl" to "nachoi.github.io")

  println(myMap["myNickname"])    //Nachoi
  println(myMap.get("myNickname"))    //Nachoi
  println(myMap.getOrDefault("myBlogUrl", "default.com") ) //nachoi.github.io
  println("모든 keys: ${myMap.keys}")   //모든 keys: [myNickname, myBlogUrl]
  ```
<br>
### hashMapOf
- `key` to `value` (`키`와 `값`) 을 가지는 형태의 데이터 컬렉션.
- mapOf 와 유사하지만 mutable - 포함된 데이터를 변경 가능하다.
- hashMapOf()의 메소드
  ```Kotlin
  val myHashMap = HashMap<String, Any>()
  myHashMap.put("thisKey", "thisValue")
  myHashMap.put("secondKey", 2)
  println(myHashMap.keys) //[thisKey, secondKey]
  println(myHashMap.get("thisKey"))   //thisValue

  myHashMap.clear() //모든 내용 지우기
  println(myHashMap.isEmpty()) //내용이 없는지 확인하여 Boolean 반환(true)
  ```
<br><br>

## Print Collections by Loops - 반복문으로 컬렉션 출력
`arrayListOf` 혹은 `hashMapOf` 에 있는 값을 출력하는 방법은 반복문과 함께 `in`을 사용하는것.

  ```Kotlin
  for (something in myArrayList) {  //something은 임의의 변수 이름
    println("값은: $something")
  }
  // arrayList의 모든 값이 차례로 출력됨.


  for ((key, value) in myHashMap) {  //역시 key, value는 바꿀수 있는 변수명
    println("$key에 해당하는 값은 $value이다.")
  }
  ```

for 에 관한 내용은 [Kotlin Funcion 링크](Kotlin-Function.html) 참조.
