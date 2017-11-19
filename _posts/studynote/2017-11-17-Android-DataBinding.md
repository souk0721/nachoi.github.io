---
layout: post
title:  "DataBinding 데이터 바인딩"
author: Yena Choi
categories: studynote
tags: [android, findviewbyid, databinding]
---

## DataBinding
findViewById 하는게 아니라, UI와 실제 데이터를 연결해놔서
**어느 액티비티에서든 일일이 findView하지 않아도 사용할 수 있도록** 도와주는 라이브러리.


```java
// build.gradle (Module:app) 에서
dataBinding.enabled = true;

// 데이터로 채워진 Class, 예를 들어 아래와 같은 class와 연결.
public class SampleInfo {
  public String name;
  public int age;
  public timeStamp currentTime;
}
```

- dataBinding을 enable 한 후, `layout.xml` 전체의 루트 태그를 <layout>으로 지정.
- 그 하위에 있던 정의들은 옮겨온다. 그 후, 프로젝트를 Rebuild 하여 바인딩 클래스 생성

<br>
```java
// MainActivity의 onCreate 전에
ActivityMainBinding mBinding;

// onCreate에서
mBinding = DataBindingUtil.setContentView(this, R.layout.activity_main);
SampleInfo sampleinfo = Data가져오는Util.generateInfo(); //메소드 실행

// MainActivity에 void 메소드 생성해서 데이터값 연결. (SampleInfo 클래스를 변수로)
mBinding.(layout에있던TextViewId).setText(info.연결할String등의값Name);
mBinding.textViewName.setText(info.stringName);
```

- 마지막으로 `onCreate`에서 메소드 실행.
