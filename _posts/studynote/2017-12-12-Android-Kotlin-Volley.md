---
layout: post
title:  "[Android][Kotlin] 안드로이드 Volley"
author: Yena Choi
categories: studynote
tags: [android, kotlin]
---

## Volley
안드로이드의 `Volley`는 안드로이드 앱에서 쉽고 빠르게 네트워킹을 할 수 있게 해주는 HTTP 라이브러리이다. ~~(한글로 적었는데, 다시 보니 모두 영어다)~~

기본적으로 안드로이드에서 Web Request를 요청할 때 HTTP 라이브러리를 쓸 수도 있지만, 사용하기 어렵기도 하고 AsyncTask를 써야하는 등 번거로움이 많다. Volley의 장점을 몇 가지만 살펴보자면 다음과 같다. ([Android Developers Volley 페이지](https://developer.android.com/training/volley/index.html)와 [gist.github.com/benelog/5981448](https://gist.github.com/benelog/5981448) 페이지를 참고)

- 네트워크 요청(Request) 우선 순위를 자동으로 관리한다.
  - 요청별 우선 순위를 지원한다. '목록 조회'와 '이미지 다운로드' 요청이 있을 경우, 우선 순위가 높은 '목록 조회' 요청은 이미지 로딩이 끝나지 않더라도 수행된다.
- 동시에 여러 네트워크 요청을 할 수 있다.
- 요청을 할 때 Cache 적용 여부를 의식하지 않아도 된다.
<br><br>

### Volley 사용 준비
안드로이드 스튜디오의 그래들 `build.gradle (Module:app)` dependencies 하단에 volley 관련 코드를 추가한다.

```
dependencies {
    ...
    compile 'com.android.volley:volley:1.1.0'
}
```
dependency 추가 후에는 우측 상단의 **Sync Now** 를 클릭하여 업데이트를 해준다.
<br>

또한, 매니페스트 파일에서 인터넷 퍼미션을 허가해주어야 한다.
```
<uses-permission android:name="android.permission.INTERNET" />
```
<br><br>

### Volley로 간단한 Request 요청하기
Volley를 사용하려면 `RequestQueue`를 만들고 `Request` 객체를 전달해야 한다.

RequestQueue는 작업이 이루어지는 스레드를 관리한다. 이 작업자 스레드에서는 네트워크 작업 수행, 캐시 읽기/쓰기, 그리고 응답(Response)을 분석(Parsing)이 이루어진다.

Request는 response의 원본 데이터를 파싱하고, Volley는 파싱 된 response를 다시 메인 스레드로 전달하는 역할을 한다.


### References
- https://developer.android.com/training/volley/index.html
- https://developer.android.com/training/volley/simple.html
- https://gist.github.com/benelog/5981448
