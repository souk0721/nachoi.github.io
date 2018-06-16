---
layout: post
title:  "[Android] Layout Design Tips 레이아웃 꾸미기"
author: Yena Choi
categories: studynote
tags: [android, kotlin, layout]
comments: true
---


## Attributes
`res-layout-activity.xml` 의 Design 탭 우측에 View의 상세 설정을 돕는 Attributes 창이 있다.

- 가장 위에서는 **ID** 를 설정할 수 있다.
- 아이콘을 눌러 View 크기 지정하는 방법을 설정.

  1. wrap_content: 내용의 크기를 감싸도록 크기 자동 조정.<br>
  <img src="/assets/post-img/171123-img1.jpg" width="245" height="187">
  <br>

  2. fixed: 크기의 절대값을 지정.<br>
  <img src="/assets/post-img/171123-img2.jpg" width="245" height="187">
  <br>

  3. match_constraints: 가능한 최대 범위까지 크기 확장.<br>
  <img src="/assets/post-img/171123-img3.jpg" width="245" height="187">

    이 설정을 사용할 경우, 좌측 상단에 삼각형이 생긴다. 클릭하면 View 가로와 세로의 비율을 지정할 수 있다.<br>
       - *ex) ratio 1:1 로 지정할 경우, 가능한 범위 내에서 가로와 세로 길이가 같아진다.*
<br><br>

## View 정렬 및 관리하기
Shift 누르고 여러 View를 선택한 후, 오른쪽 클릭을 하여 정렬하거나 관리할 수 있다.

<center><img src="/assets/post-img/171123-chain.jpg" width="50%" height="50%"></center>

- **Organize**: 여러 View의 그룹을 지정할 수 있다.
  - 이 속성을 지정하지 않으면, 실행취소 할 때 하나씩 움직인다.
- **Align**: 좌우나 중앙을 기준으로 View를 정렬한다.
  - 설정을 바꾸기 전까지 정렬이 계속 유지된다.
- **Chain**: 여러 View를 가로 혹은 세로로 묶어서 배치한다. Design 탭에서 사슬 아이콘을 누르면 3가지 모드로 정렬된다.
  1. spread: Margin을 포함하여 균등하게 정렬.
  2. spread-inside: View를 가로/세로에 꽉 차게 균등하게 정렬.
  3. packed: View 간 간격 없이 화면 가운데에 정렬.

<br><br>
## 이미지 파일 정돈하기
- 이미지 resource 를 사용할 경우, 여백이나 채우기 방법을 지정할 수 있다.
- `Attributes`-`Scaletype` 에 **fit** 이나 **crop** 등의 속성 선택.
<br>

### 이미지 모양 다듬기
- 사각형 이미지의 모서리를 둥글게 하려면, java 파일에서 비트맵을 만든 후에 이미지를 입힌다.
  ```java
  val bitmap = BitmapFactory.decodeResource(resources, R.drawable.myImgFileName)
  val rounded = RoundedBitmpaDrawableFactory.create(resources, bitmap)
  rounded.cournerRadius = 15f //모서리 둥글게하기
  myImgId.setImageDrawable(rounded)
  ```
- 이미지를 원형으로 만드려면 `cournerRadius` 대신 `rounded.isCircular` 속성을 적용한다.
  ```java
  rounded.isCircular = true
  ```

<br><br>
### Remove ActionBar 액션바 지우기
- `res-values-styles.xml` 에서 테마를 `NoActionBar`로 변경한다.

<br><br>
### 최상단 상태표시줄 색 변경
- `res-values-colors.xml` 에서 Primary 색을 재지정한다.
