---
layout: post
title:  "AsyncTask"
date:   2017-11-05
author: Yena Choi
categories: studynote
---
# Android Study
안드로이드에 관해 공부한 내용을 정리, 나중에 찾아보기 위해 짧게 메모한 내용.

### AsyncTask
: 메인 UI에서 서브 쓰레드로 doInBackground를 보내고, 선택에 따라 결과를 메인
UI에 표시하며, onPostExecute로 다 끝낸 후 값을 Return받음

```java
public class MyAsyncTask extends AsyncTask<URL, Void, String>
// 여기서는 중간과정 받는걸 Void를 통해 생략
  @Override
  protected String doInBackground(URL... urls) {
    URL searchUrl = urls[0];
    String githubSearchResults = null;
    try {
      githubSearchResults = NetworkUtils.getResponseFromHttpUrl(searchUrl);
    } catch (IOException e) {
      e.printStackTrace();
    }

    return githubSearchResults;
  }
  // 네트워크유틸 통해서 url 보낸다음에 반응에 대한 결과값을 String으로 받음.

  @Override
  protected void onPostExecute(String s) {
    if (s != null && !s.eqauls("")) {
        mSearchResultsTextView.setText(s);
    }
  }
}
```


다 만들었으면 에이싱크 태스크 시작.

```java
new MyAsyncTask().execute(githubSearchUrl); //url 하나 넣고 시작
```
