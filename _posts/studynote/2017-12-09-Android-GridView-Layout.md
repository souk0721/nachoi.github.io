---
layout: post
title:  "[Android] GridView 그리드뷰 아이템 만들기"
author: Yena Choi
categories: studynote
tags: [android, view]
---


## GridView item
그리드뷰를 사용하려면 우선 뷰를 채울 `item`을 새 레이아웃 파일로 만든다.

이미지를 정사각형으로 유지하려면 width와 height 값이 같아야 한다. 하지만 dp로 길이 값을 입력하면 화면 크기가 달라져도 이미지 크기가 고정이 되어서 예쁘게 배열되지 않는다.

이럴 때에는 이미지뷰를 클릭하고 `attributes` 창 왼쪽에서 **ratio 1:1** 설정과 **match_constraint** 설정을 해주어야 한다.

<center>
<img src="/assets/post-img/171209-01.JPG" width="90%" height="90%">
</center>
<br>

```xml
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <ImageView
        android:id="@+id/gridImg"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:contentDescription="@string/gridimg"
        app:layout_constraintDimensionRatio="1:1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher" />

    <TextView
        android:id="@+id/gridName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/gridImg"
        tools:text="Title" />

    <TextView
        android:id="@+id/gridSub"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/gridName"
        tools:text="Subtitle" />

</android.support.constraint.ConstraintLayout>
```
<br>
