---
layout: post
title:  "[Android] ConstraintLayout 레이아웃"
author: Yena Choi
categories: studynote
tags: [android, layout, constraintlayout]
---

## ConstraintLayout
- build.gradle (Module:app)에서 ConstraintLayout의 dependency를 추가해야 한다.
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
