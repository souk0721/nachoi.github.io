---
layout: post
title:  "Url 만들기 & Uri.Builder"
date:   2017-11-05
author: Yena Choi
categories: studynote
---

# Android Study
안드로이드에 관해 공부한 내용을 정리, 나중에 찾아보기 위해 짧게 메모한 내용.


### URL 만들기
- NetworkUtils 클래스를 하나 만들어서 원 URL이랑 구분할 파라미터를 String 지정. (어디에서든 메소드를 불러올 수 있게)
- URL을 return하는 buildUrl 메소드를 만든다.

  *(example)*
  ```
  public static URL buildUrl(String githubSearchQuery) {
    Uri builtUri =
            Uri.parse(GITHUB_BASE_URL).buildUpon()
              .appendQueryParameter(PARAM_QUERY, githubSearchQuery)
              .appendQueryParameter(PARAM_SORT, sortBy)
              .build();

    // 위에서 조합한 uri를 아래와 같이 자바 url 형태로 변환
    URL url = null;
    try {
        url = new URL(builtUri.toString());
    } catch (MalformedURLException e) {
        e.printStackTrace();
    }

    return url;
  }
  ```

- 메인액티비티에서 EditText의 문자열을 반영해 URL를 생성하는 메소드 만들기

  ```
  void makeGithubSearchQuery() {
      String githubQuery = mSearchBoxEditText.getText().toString();
      URL githubSearchUrl = NetworkUtils.buildUrl(githubQuery);
      mUrlDisplayTextView.setText(githubSearchUrl.toString());
  }
  ```

##### Uri Builder
- Uri.parse는 uri를 가져오는 역할을 한다. 그렇지만 Uri.Builder를 사용하면 여러 요소들을 합쳐 만들 때 실수할 수 있는 부분들을 방지해준다.

*(example)*   
https://nachoi.github.io/category/posts?param=study#section 라는 Uri를 만들고 싶을 때.

```
Uri.Builder builder = new Uri.Builder()
      .scheme("https")
      .authority("nachoi.github.io")
      .appendPath("category")
      .appendPath("posts")
      .appendQueryParameter("param", "study")
      .fragment("section");

  Uri myUri = builder.build();

```

*(example)*   
지도에 표시할 geo 정보를 uri로 가져올 경우
```
Intent showMapIntent = new Intent(Intent.ACTION_VIEW);
showMapIntent.setData(myUri);

if(showMapIntent.resolveActivity(getPackagerManager()) =! null) {
    startActivity(showMapIntent);
}
```
