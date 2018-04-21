---
layout: post
title:  "[Android] ConstraintLayout 레이아웃"
author: Yena Choi
categories: studynote
tags: [android, layout]
---

언제부턴가 Android Studio에서 ConstraintLayout 를 강력하게 밀고 있는 듯하다. New project를 만들면 기본적으로 생성되는 "Hello World!" 텍스트가 있는 액티비티도 ConstraintLayout으로 생성된다.
<br>

## ConstraintLayout

ConstraintLayout(컨스트레인트 레이아웃)은 위치와 크기 조절을 유연하게 할 수 있는 ViewGroup이다. 다양한 해상도를 지원하는 앱 작업을 할 때 상당히 편리하다.


아래 예제는 **같은 코드로 작성한 xml 파일을 다른 기기(해상도)에서 실행한** 것이다. 1) TextView, 2) ImageView, 3) Button의 세 가지 뷰로 구성되어 있다. 각 View마다 크기 및 위치 조건은 다음과 같이 주었다.

- 이미지 : 원본 크기 맞춤, Parent의 수직 및 수평 중앙.
- 텍스트 : 텍스트 크기 맞춤, 이미지의 왼쪽에 맞춰 정렬, 이미지의 8dp 위에 위치.
- 버튼 : 이미지의 좌우 크기에 맞춰 크기 확장, 이미지ㅡParent의 20% 되는 높이에 위치
<br>

<center>
  <img src="/assets/post-img18/180418-01.jpg"><br>
  ▲ Nexus 4 (768 x 1280) <br><br>
  <img src="/assets/post-img18/180418-02.jpg"><br>
  ▲ Pixel 2 XL (1440 x 2880) <br><br>
  <img src="/assets/post-img18/180418-03.jpg"><br>
  ▲ Nexus 10 (2560 x 1600) <br><br>

</center>
<br>

이제 하나씩 살펴보자. `app`-`res`-`layout`-`activity_main.xml` 에서 xml 파일로 된 레이아웃 코드를 볼 수 있다. Design 탭에서 직접 뷰를 보면서 조정해도 된다. Design탭과 xml 탭에서 내용을 변경할 시 다른 한 쪽에서도 자동으로 변경된다.

```xml
<!-- activity_main.xml -->

<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:text="Hello Layout!"
        app:layout_constraintBottom_toTopOf="@+id/imageView"
        app:layout_constraintStart_toStartOf="@+id/imageView" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintDimensionRatio="1:1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/simpsonize_nachoi" />

    <Button
        android:id="@+id/button"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:text="Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="@+id/imageView"
        app:layout_constraintStart_toStartOf="@+id/imageView"
        app:layout_constraintTop_toBottomOf="@+id/imageView"
        app:layout_constraintVertical_bias="0.19999999" />

</android.support.constraint.ConstraintLayout>
```
<br><br>

### 특징(1) - width 와 height
각 뷰는 필수적으로 가로와 세로 높이를 나타내는 속성을 가지고 있어야 한다.
- `android:layout_width` : 뷰의 가로 길이를 나타냄. (폭, 너비)
- `android:layout_height` : 뷰의 세로 길이를 나타냄. (높이)

그리고 이 가로, 세로 속성은 3종류의 값을 가질 수 있다.

- `none` : 화면의 비율에 상관 없이 고정 값을 갖는다. `dp` 단위로 표기한다.
- `wrap_content` : 뷰가 차지하는 크기 만큼의 길이. Image의 경우, 특별히 dp값을 지정하지 않으면 원본 크기가 되며, TextView의 경우는 글자를 감싸는 크기가 된다.
- `match_constraint` : 실제로 xml 파일에서는 `"0dp"`로 표기한다. 뷰의 가로 혹은 세로를 다른 곳에 고정시킨 후, 해당하는 영역을 모두 차지한다.


예제의 TextView나 ImageView의 width와 height 값 모두 wrap_content이다.

```xml
android:layout_width="wrap_content"
android:layout_height="wrap_content"
```
<br>

반면 Button의 width값은 match_constraint 이다. 0dp로 표시되며, 왼쪽과 오른쪽이 연결된 곳에 길이가 자동으로 맞춰진다. 만약 왼쪽과 오른쪽을 TextView에 연결했으면 TextView가 차지하는 영역 만큼이 버튼 크기가 되고, 레이아웃(Parent)에 연결했으면 화면을 꽉 채우는 크기가 된다.
```xml
android:layout_width="0dp"
android:layout_height="wrap_content"
```
<br>


### 특징(2) - constraint (A)\_to(B)Of

xml 파일을 살펴보면 공통적으로 여러 개의 constraint(A)\_to(B)Of 옵션을 가지고 있다. 말 그대로 상/하/좌/우를 다른 뷰의 어느 곳에 연결할 것인지 정해주는 옵션이다. **constraint** 를 정해주지 않으면 경고 표시가 발생하며, View를 다른 곳에 연결하지 않은 채 앱을 실행하면 View가 좌측 상단으로 이동해버린다. 그렇기 때문에 반드시 다른 View 혹은 Parent에 연결해야 한다.

**ImageView**는 상/하/좌/우가 각각 parent에 연결되어 있다.
```xml
app:layout_constraintBottom_toBottomOf="parent"
app:layout_constraintEnd_toEndOf="parent"
app:layout_constraintStart_toStartOf="parent"
app:layout_constraintTop_toTopOf="parent"
```
길이 값이 wrap_content이면서 4방향 모두 parent에 연결되어 있을 때 View는 예제와 같이 화면 중앙에 위치하게 된다. (만약 길이 값이 match_constraint인 경우에는 parent의 크기에 맞춰 이미지가 크기가 늘어난다.)

다음으로, **TextView**는 좌측이 ImageView의 좌측에, 하단이 ImageView의 상단에 각각 연결되어 있다.
```xml
app:layout_constraintBottom_toTopOf="@+id/imageView"
app:layout_constraintStart_toStartOf="@+id/imageView"
```
<br>
View의 id값을 통해 View를 찾고, 이를 기준으로 constraint 옵션이 생긴다. 하단에는 `layout_marginBottom` 값이 있기 때문에 8dp의 여백이 생긴다. 이 값이 없다면 TextView는 ImageView 바로 위에 위치하게 된다.
```xml
android:layout_marginBottom="8dp"
```
<br>

**Button** 에 적용된 constraint는 다음과 같다.
```xml
app:layout_constraintBottom_toBottomOf="parent"
app:layout_constraintEnd_toEndOf="@+id/imageView"
app:layout_constraintStart_toStartOf="@+id/imageView"
app:layout_constraintTop_toBottomOf="@+id/imageView"
```
<br>

좌우는 ImageView에, 상하는 각각 ImageView와 parent에 연결되었다. match_constraint 옵션이기 때문에 ImageView의 width와 길이가 같다. 만약 다른 곳에 연결한다면 Button의 크기도 달라지게 된다.
<br>

<center>
  <img src="/assets/post-img18/180418-04.jpg"/>
</center>
<br>
Button의 Start와 End를  **ImageView, TextView, parent** 의 좌우에 연결했을 때 `match_constraint`, 즉 `android:layout_width="0dp"` 값을 가질 때 위와 같이 적용된다.


<br>


### 그 밖의 참고사항

- 이전에는 `build.gradle(Module:app)`에서 ConstraintLayout의 dependency를 추가해야 했는데, 요즘에는 자동으로 implementation에 추가되는 것 같다. 만약 없다면 아래와 같은 형태로 그래들에 추가 해야 한다.
  ```
  dependencies {
      ...
      implementation 'com.android.support.constraint:constraint-layout:1.1.0'
  }
  ```


- 몰랐던 ConstraintLayout 팁들
```
android:layout_marginLeft="10dp" // 좌측 여백을 위한것.
android:layout_marginStart="10dp" // UI의 좌우를 바꿀때 필요. (Left와 dp값 같게)
android:text="실제 App에 표시되는 text들"
tools:text="디자인을 위해서만 필요한 Fake text들"
```

- View가 기준이 되는 Layout의 상/하단 중심에 오게 하려면
  `constraintTop_toBottomOf` 및 `constraintBottom_toTopof` 해야한다.


- View의 수적/수평 조건은 다음 셋 중 하나의 방식으로 동작한다.
  1. 고정 크기(Fixed)
  2. 늘거나 줄면서 조건유지(AnySize)
  3. Wrap Content

### References
- https://developer.android.com/reference/android/support/constraint/ConstraintLayout.html
-
