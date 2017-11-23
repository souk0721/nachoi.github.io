---
layout: post
title:  "Github 이미지 사이즈 조절 & 정렬"
author: Yena Choi
categories: studynote
tags: [github, jekyll, markdown, image]
---

깃허브 포스트를 올렸는데, 이미지 크기가 원본보다 커져서 당황했다. 여차저차 구글링 해서 가로/세로 사이즈를 줄였는데, 가로 길이가 줄어들지 않는다. 어떻게든 줄여놓으니 또 이미지 정렬이 말썽이었다. 나중에 분명 이걸로 또 고생할 것 같아서 따로 포스트를 쓰기로 한다. 본 블로그는 Jekyll 지킬로 만들어졌으므로, 포스트도 지킬에 적용되는 부분을 중심으로 작성했다.

## 이미지 업로드
이미지를 업로드 하는 기본적인 방법은 [마크다운 가이드](https://guides.github.com/features/mastering-markdown/)에 잘 나와있듯이 아래와 같다.

```markdown
![title](http://fakeImgUrl.com/img/01.jpg)
![title](/img/myComputer.png)
```
<br>

가로, 세로 300 x 300 픽셀 이미지를 올려봤다. ~~단순히 내 심슨 캐릭터인데, 증명사진을 올린 것 같은 기분.~~

![nachoi](/assets/post-img/171123-nachoi-300.jpg)
<br>

### Resize Image 이미지 리사이징
마크다운 파일에서 이미지의 크기를 조절하려면 `{: width="" height=""}` 부분을 추가한다. 숫자 뒤 `px` 단위를 붙이거나 생략할 수 있다.

```
![title](/img/myImg.png){: width="100" height="100"}
```

![nachoi](/assets/post-img/171123-nachoi-300.jpg){: width="100" height="100"}
<br>

혹은, `%` 단위로 이미지 사이즈 조절이 가능하다.
```
![title](/img/myImg.png){: width="100%" height="100%"}
```

![nachoi](/assets/post-img/171123-nachoi-300.jpg){: width="100%" height="100%"}

<br><br>

다른 방법으로는, html의 `<img>` 영역을 직접 텍스트로 입력하는 방법이 있다.

```html
<img src="/img/myImg.png" width="300" height="300">
```
<img src="/assets/post-img/171123-nachoi-300.jpg" width="300" height="300">
<img src="/assets/post-img/171123-nachoi-300.jpg" width="100%" height="100%">


### References
- https://nayunhwan.github.io/dev/2016/12/11/Img-centering/
-  https://stackoverflow.com/questions/3912694/using-markdown-how-do-i-center-an-image-and-its-caption
- https://thornelabs.net/2014/11/30/centering-images-with-jekyll-and-markdown.html
- http://gumpcha.github.io/blog/github-issue-image-resize/
