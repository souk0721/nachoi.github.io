---
layout: post
title:  "액션바 메뉴 ActionBarMenu"
date:   2017-11-05
author: Yena Choi
categories: studynote
---


# Android Study
안드로이드에 관해 공부한 내용을 정리, 나중에 찾아보기 위해 짧게 메모한 내용.

### 액션바 메뉴 (Actionbar Menu)
- 액션바에 버튼 달려면 메뉴 xml 파일에 `app:ShowAsAction="ifRoom"` 하면되겠다.
  - `ifRoom` 옵션은 **자리가 있을 경우에만 표시** 하는 옵션.
- `onCreateOptionsMenu` 오버라이딩 후, 아래와 같이 메뉴를 가져와 inflate한다.
```
getMenuInflater().inflate(R.menu.메뉴, menu);
return true;
```
- 메뉴를 만들었으면, 그 안의 옵션 내용은 `onOptionsItemSelecten` 으로 만든다. 그 후 `menu.getItemId` 해서 `switch`나 `if` 문으로 `R.id.action` 이름과 비교해서 일치하는 경우의 할일 지정.
