---
layout: post
title:  "[Android] Including Layout 레이아웃 포함하기"
author: Yena Choi
categories: studynote
tags: [android, layout]
comments: true
---

## Including Layout
- 가로/세로 모드 각각 다른 xml 파일을 만들어도 되지만, **데이터와의 연결성**
때문에 xml 내에 layout을 `<include>`를 통해 가져온다.

- `activity_main.xml` 외에 res 내부에 `layout-land` (landscape, 가로모드) 폴더를
만들어서 똑같이 `acitivity_main.xml` (가로모드 전용 xml) 파일을 만든다.

- 분할해두었던 layout을 `include`를 통해서 가져오고, 각각 배치한다.


### 최소 픽셀에 따른 해상도
- 가로/세로모드 상관 없이, **최소 픽셀** 기준으로 다른 레이아웃 설정 가능하다.
  - *ex)* layout-sw600dp(태블릿 최소 단위) 폴더에 activity.xml (메인이랑 같은 이름)
- 요구 해상도에 조건이 안맞으면, 하위 레벨에 있는 레이아웃 적용됨.
