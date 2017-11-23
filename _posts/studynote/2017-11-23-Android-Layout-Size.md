---
layout: post
title:  "Android Layout Size 레이아웃 크기"
author: Yena Choi
categories: studynote
tags: [android, layout, size]
---


## 화면 크기에 따른 Layout

폰이나 태블릿 화면 해상도와 크기에 따라 레이아웃이 부자연스러워지는 경우가 있다. 큰 화면 전용 레이아웃 파일을 따로 만들어두면 디바이스에 최적화된 레이아웃을 자동으로 적용할 수 있다.

1. `res` - `layout` 폴더에 새로운 Layout resource 파일을 추가한다.   

  ![newlayout](/assets/post-img/171123-layout.jpg)

2. 파일 이름은 기존 xml 파일과 동일하게 한다.

3. `Available qualifiers` 에서 최소 스크린 폭이나 가로/세로, 크기, 비율 등을 설정할 수 있다. 이에 따라 레이아웃을 상황별로 최적화할 수 있다. 태블릿용 화면을 만들고 싶으면 `Size`에서 가장 큰 화면을 선택하면 된다.

4. 기존 xml 영역을 복사해와서 화면에 맞게 재배치한다.

<br><br>
## 가로모드 및 세로모드 전용 Layout
마찬가지로, 새 Layout resource 파일을 만들어 `Available qualifiers` 에서 `Orientation` 속성을 변경한다.
