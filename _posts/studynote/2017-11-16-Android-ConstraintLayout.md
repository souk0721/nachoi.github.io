---
layout: post
title:  "[Android] ConstraintLayout 레이아웃"
author: Yena Choi
categories: studynote
tags: [android, layout]
---

언제부턴가 Android Studio에서 ConstraintLayout 를 강력하게 밀고 있는 듯하다. New project를 만들면 기본적으로 생성되는 "Hello World!" 텍스트가 있는 액티비티도 ConstraintLayout으로 생성된다.

## ConstraintLayout

ConstraintLayout(컨스트레인트 레이아웃)은 위치와 크기 조절을 유연하게 할 수 있는 ViewGroup이다. 다양한 해상도를 지원하는 앱 작업을 할 때 상당히 편리하다.

아래 예시는 같은 코드로 작성한 xml 파일을 다른 해상도로 나타낸 것이다. 각 View마다 크기 및 위치 조건을 주었다.
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
<br><br>

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
