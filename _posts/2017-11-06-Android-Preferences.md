---
layout: post
title:  "Intent 인텐트로 값 전달하기"
date:   2017-11-06
author: Yena Choi
categories: studynote
---

# Android Study
안드로이드에 관해 공부한 내용을 정리, 나중에 찾아보기 위해 짧게 메모한 내용.


### Preferences 환경설정
- 몰랐던 사실, 세팅 전용 함수랑 xml 테마가 따로 있었다!
- gradle dependency에 컴파일 추가.  
```
compile 'com.android.support:preference-v7:25:1.0'
```
- 그후 클래스를 만들고 (여기서는 SettingsFragment 라고 하자) `extents PreferenceFragmentCompat` 한다.
- res -> pref 전용 xml 파일을 만든다.
```xml
<CheckBoxPreference
  android:defaultValue="true"
  android:key="show_button"
  android:summaryOff="Hidden"
  android:summaryOn="Shown"
  android:title="Show Button" />
```

- 새로 만든 SettingsFragment(라고 하자) 클래스에서 `onCreatePreferences` Override 후,
 `addPreferencesFromResource(R.xml.pref 일_이름)`
- style에 가서 item name을 **"preferenceTheme"**, 본문은 **@style/PreferenceThemeOverlay**
- 마지막으로 **setting Layout** 을 다 지우고, **root Layout** 을 `fragment`로 변경한다.
```xml
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
android:id="@+id/activity_settings"
android:name="android.example.com.myPreferences.Visuals.SettingsFragment"
android:layout_width="match_parent"
android:layout_height="match_parent"/>
```


### Preference - ListPreferences

- 이전에는 **CheckBoxPreference** 로 summaryOn/summaryOff 상태값 표시 가능
- ListPreferences 는 **arrays.xml** 을 만들어서 label - value를 연결함  
  (strings value에 각각 등록한 후에 array item을 생성, 연결)
- ListPreferences 에서는 summary 기능을 사용하려면 꽤 복잡하다.


1. SettingsFragment 에 `onSharedPreferencesChangeListner` 등록한다.
2. PreferenceScreen 과 Count를 통해 전체 SharedPreferences 개수 확인  
(반복문을 통해서 모든 pref의 인덱스를 count한다.)   
`onCreatePreferences` 부분에서 최초로 한번 등록, 그다음 set 메소드에서는
변경 시에 pref를 반영하여 변경한다.
3. 메소드를 통해서 Preference가 ListPreferences가 맞는지 확인한다.  
  맞다면 가져온 후에 Value값을 이용해 Index를 가져온 후, Index의 Label 불러온다.
  - 우리가 summary에 보여줄 것은 label 이니까.
4. `onCreatePreferences`로 다시 돌아와서, index 반복문 중에 pref가
  CheckBoxPreference가 아니면 메소드를 통해 label 값을 설정함.
5. 리스너 등록 위해서 Pref가 null이 아니고 CbPref가 아니면 메소드로 값 성정.
6. 리스너를 `onCreate`와 `onDestroy`에 등록.



### PreferenceListener vs SharedPreferencesListener
- **PreferenceListener는 pref파일에 저장되기 전에 반응, Shared는 저장된 후에 반응.**
  - 따라서 잘못 저장할일 없도록 pref리스너에서 우선 확인 후 shared리스너에 저장하는 편이 좋겠다.
- 하나의 preference와 value를 값으로 받아서 boolean을 return하는 onPreferenceChange
- 숫자가 범위 초과이거나, try-catch를 통해 숫자가 아닌경우를 모두 등록. 이럴땐 false 리턴
