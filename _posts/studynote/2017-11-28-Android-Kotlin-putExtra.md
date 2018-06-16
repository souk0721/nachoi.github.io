---
layout: post
title:  "[Android][Kotlin] PutExtra 데이터 전달"
author: Yena Choi
categories: studynote
tags: [android, kotlin, intent]
comments: true
---

`Java`를 이용한 값 전달 방법은 [다른 포스트](/studynote/2017/11/06/Android-Intent.html)에 작성하였다.
<br>

## PutExtra & GetExtra
`Intent`를 통해 액티비티를 전환할 때에 `putExtra`를 통해 String이나 Int 등 간단한 데이터를 전달할 수 있다. `putExtra(key: String, value: 간단한 데이터)` 의 형식을 띤다.

```Kotlin
/* FirstActivity.kt */

val nextIntent = Intent(this, SecondActivity::class.java)
nextIntent.putExtra("nameKey", "nachoi")
startActivity(nextIntent)
```
<br>

FirstActivity에서 `putExtra` 한 후, SecondActivity 에서 `getStringExtra`, `getBooleanExtra` 등 자료형에 맞는 메소드를 사용하여 데이터를 꺼내올 수 있다. 이때 **null check** 를 하지 않으면 0이나 null 등 잘못된 값을 꺼내올 수 있으므로, 해당 key가 전달할 값을 가지고 있는지 `if hasExtra`로 체크하는 것이 좋다.

```kotlin
/* SecondActivity.kt */

if (intent.hasExtra("nameKey")) {
    textView.text = intent.getStringExtra("nameKey")  
    /* "nameKey"라는 이름의 key에 저장된 값이 있다면
       textView의 내용을 "nameKey" key에서 꺼내온 값으로 바꾼다 */

} else {
    Toast.makeText(this, "전달된 이름이 없습니다", Toast.LENGTH_SHORT).show()
}

```
<br>
하나의 intent에 여러 개의 데이터를 `putExtra` 할 수 있다.

<br><br>

### Parcelable
만약 전달해야 할 데이터의 개수가 많다면 일일이 put/get하기 번거로울 것이다. 이럴 때에는 새로운 Class를 만들어 그 클래스의 변수를 넘길 수 있다.

```kotlin
/* 'name', 'score', 'grade' 데이터를 가지는
   'Exam' 이라는 이름의 새로운 Kotlin Class 파일을 생성 */

class Exam constructor (var name: String, var score: Int, var grade: String)
```

하지만 FirstActivity에서 Exam 변수를 생성해서 putExtra로 전달하려고 하면 오류가 난다. 문자열, 숫자 등 간단한 데이터는 putExtra로 전달 가능하지만 임의로 커스텀한 Class의 데이터는 putExtra로 바로 전달할 수 없다. 이럴 때 필요한 것이 `Parcelable` 이다.

Parcelable은 클래스에 있는 데이터 형식을 바꿔서 Activity -> Activity 등에 전달할 수 있게 하는 인터페이스이다. 쉽게 비유하자면, 파일을 zip 형식으로 압축해서 이동 완료한 후 압축을 푸는 것과 비슷하다. Java 에서는 implement 를 입력하고, Kotlin 에서는 클래스 뒤에 `:` 를 붙여 사용한다.

```kotlin
 class Exam constructor (var name: String, var score: Int, var grade: String) : Parcelable {
   /* : Parcelable에서 Alt + Enter하여 내용을 implement한다. */

     constructor(parcel: Parcel) : this(
             parcel.readString(),
             parcel.readInt(),
             parcel.readString()) {
     }

     override fun writeToParcel(parcel: Parcel, flags: Int) {
         parcel.writeString(name)
         parcel.writeInt(score)
         parcel.writeString(grade)
     }

     override fun describeContents(): Int {
         return 0
     }

     companion object CREATOR : Parcelable.Creator<Exam> {
         override fun createFromParcel(parcel: Parcel): Exam {
             return Exam(parcel)
         }

         override fun newArray(size: Int): Array<Exam?> {
             return arrayOfNulls(size)
         }
     }
 }

```

뭔가 아주 많이 생겼지만, 여기서 따로 건드릴 것은 없다. 그리고 이제 `Parcelable` 자료형의 데이터를 putExtra 할 수 있다!
<br>

```kotlin
/* FirstActivity.kt */

var myExam = Exam("nachoi", 90, "A")
/* name, score, grade 데이터를 가지는 Exam 변수 생성*/

nextBtn.setOnClickListener{
    val nextIntent = Intent(this, SecondActivity::class.java)
    nextIntent.putExtra("examKey", myExam)
    startActivity(nextIntent)
    /* intent.putExtra 로 Exam 클래스의 변수 myExam을 저장 */
}
```

```kotlin
/* SecondActivity.kt */

if (intent.hasExtra("examKey")) {
    var exam = intent.getParcelableExtra<Exam>("examKey")
    /* exam변수를 초기화 할 때 자료형이 정해지지 않아서
     getParcelableExtra 뒤에 <>가 생기고, 이곳에 자료형을 입력한다. */

    println("${exam.name}의 시험 점수는 ${exam.score}점이므로 ${exam.grade}등급이다.")
}
```
<br><br>

### References
- https://developer.android.com/reference/android/os/Parcelable.html
