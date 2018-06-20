---
layout: post
title:  "[Android] Android Studio 렌더 오류"
author: Yena Choi
categories: studynote
tags: [android, error]
comments: true
---
<br><br>
### Render Problem - 렌더 오류
모처럼 포스팅하려고 샘플 앱을 만들었는데 layout 화면이 안 뜬다.
<br>

<center>
  <img src="/assets/post-img18/180617-01.jpg" width="100%" height="100%"><br>
</center>
<br>

**Render Problem** Falied to load App compat ActionBar with unknown error.<br>
Tip: Try to refresh the layout.<br>
**Failed to instantiate one or more classes.**

대략 이런 메시지가 나온다.
알 수 없는 이유로 렌더링 오류가 났다고 하는 것 같다.

View에 각각 id도 부여해보고, 다른 형식의 레이아웃으로도 바꿔봤지만 화면은 여전히 백지 상태. 결국 구글의 힘을 빌리게 되었다.

<br><br>

### 해결

시도해 본 방법을 나열해보면 아래와 같다.
1. 오류 메세지 확인
2. SDK 재확인
3. (Windows) 폴더명, 혹은 사용자 이름에 공백이 있을 시 수정.
<br>

#### 1. 오류 메세지 확인
우선, 오류 메세지에 나와있는 대로 새로고침도 해보고 캐시도 지워보자. 사실 이걸로 해결이 되지는 않았다. 이렇게 간단히 해결될 거였으면 안드로이드 스튜디오가 한 번 더 확인 해줬을 것이다. 나의 친구 안드로이드는 그렇게 멍청할 리 없다.

<center>
  <img src="/assets/post-img18/180617-02.jpg" width="100%" height="100%"><br>
</center>
<br><br>

#### 2. SDK 재확인
사용 중인 PC에 필요한 SDK가 설치되어 있지 않을 때에도 렌더링 오류가 나는 듯하다.

**File - Settings** 에 들어가서 **Appearance & Behavior - System Settings - Android SDK** 메뉴를 찾거나, Setting 상단에 **SDK** 를 검색한다. 나의 경우 얼마 전 포멧을 했기 때문에 주로 사용하던 SDK가 없거나 일부만 설치되어 있었다.

<center>
  <img src="/assets/post-img18/180617-03.jpg" width="100%" height="100%"><br>
</center>
<br>

`build.gradle (Module:app)` 파일에서 SDK 버전을 확인한 후 다시 한 번 `Sync Now` 해준다.

<center>
  <img src="/assets/post-img18/180617-04.jpg" width="100%" height="100%"><br>
</center>
<br>

#### 3. 폴더명, 혹은 사용자 이름에 공백이 있을 시 수정.
맥의 경우는 잘 모르겠다. 윈도우의 경우 **사용자 이름**에 공백이 포함되어 있을 경우에 오류가 나는 모양이다. (실제로 프로젝트를 생성할 때, 안드로이드 프로젝트의 경로가 공백(Whitespace)이 포함된 경로에 저장되면 문제가 생길 수도 있다는 경고를 해 준다.) 이것도 결국 2번과 관련된 것인데, 공백이 포함 된 경우에는 **SDK**를 제대로 불러 오지 못할 수 있다. 단순히 폴더 이름에 공백이 있는 것이라면 수정하면 되겠지만, 사용자 이름에 공백이 있다면 조금 더 복잡해진다. 예를 들어 이름이 'YENA CHOI'일때 바탕화면에 프로젝트를 저장한다면 경로는 `C:\Users\YENA CHOI\Desktop` 와 같이 공백을 포함하게 된다.

**내 PC 아이콘 우클릭 - 관리** 메뉴에 들어가면 '컴퓨터 관리' 화면으로 진입할 수 있다. **로컬 사용자 및 그룹 - 사용자** 에서 이름에 공백이 있는 경우 꽤 복잡한 방법을 통해 이것을 수정해야 한다.
<center>
  <img src="/assets/post-img18/180617-05.jpg" width="100%" height="100%"><br>
</center>
<br><br>

우여곡절 끝에 제대로 표현되는 레이아웃 :D

<center>
  <img src="/assets/post-img18/180617-06.jpg" width="100%" height="100%"><br>
</center>
<br>

### References
- https://stackoverflow.com/questions/18195807/android-studio-rendering-problems
- https://stackoverflow.com/questions/29194479/android-studio-rendering-problems-the-following-classes-could-not-be-found
- http://itrainbowm.tistory.com/29
