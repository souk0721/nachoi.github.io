---
layout: post
title:  "Github 이미지 사이즈 조절 & 정렬"
author: Yena Choi
categories: studynote
tags: [github, jekyll, markdown, image]
---

깃허브 포스트를 올렸는데, 이미지 크기가 원본보다 커져서 당황했다. 여차저차 구글링 해서 가로/세로 사이즈를 줄였는데, 가로 길이가 줄어들지 않는다. 어떻게든 줄여놓으니 또 이미지 정렬이 말썽이었다. 나중에 분명 이걸로 또 고생할 것 같아서 따로 포스트를 쓰기로 한다. 본 블로그는 Jekyll 지킬로 만들어졌으므로, 포스트도 지킬에 적용되는 부분을 기준으로 작성했다.

## 이미지 업로드
깃허브에서 이미지를 업로드 하는 기본적인 방법은 [마크다운 가이드](https://guides.github.com/features/mastering-markdown/)에 잘 나와있듯이 아래와 같다.

```markdown
![title](http://fakeImgUrl.com/img/01.jpg)
![title](/img/myComputer.png)ㅆ
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

혹은, `%` 단위로 이미지 사이즈 조절이 가능하다. 주의할 점은 이미지 원본 크기가 100% 인 것이 아니라, **차지할 수 있는 전체 범위를 100%로** 보는 듯하다. 가로, 세로 각각 100%로 나타내면 이미지는 아래와 같아진다.
```
![title](/img/myImg.png){: width="100%" height="100%"}
```

![nachoi](/assets/post-img/171123-nachoi-300.jpg){: width="100%" height="100%"}

<br><br>

다른 방법으로는, html의 `<img>` 영역을 직접 텍스트로 입력하는 방법이 있다. 마찬가지로, width 나 height를 100%로 지정하면 차지할 수 있는 전체 영역의 100% 사이즈가 된다.

```html
<img src="/img/myImg.png" width="300" height="300">
```
<img src="/assets/post-img/171123-nachoi-300.jpg" width="300" height="300">

<br>

#### 이미지 사이즈 조절이 안될 때
지킬 블로그에 외부 테마를 적용했다면 위의 코드들이 작동하지 않을 수 있다. 기본적으로 설정된 post의 레이아웃에 이미지의 크기나 정렬 등이 미리 설정되어 있다면 크기가 바뀌지 않을 것이다. 나의 경우, `default width` 값이 있어서 세로 길이만 변하고 가로는 고정되는 오류가 났다.

해결책은 `layout.css` 파일에서 기본 설정된 값을 해제하는 것이었다. 테마 마다 조금씩 다른데, 나는 \_layout.scss 파일에서 .page-content 의 img 영역의 이미지 설정값을 비활성화했다.

**요약하자면, post의 `body` 내용을 관리하는 layout css 파일에서 이미지 관련 기본 설정된 값이 있을 수 있는지 확인해본다.**

<br><br>
### Center Image 이미지 가운데 정렬
미리 요약하자면, 현재 마크다운에서 이미지 정렬 관련된 기능을 공식적으로 제공하고 있지는 않는 것 같다. 사람들이 주로 사용하는 방법은 **새 css 파일을 만들고 가운데 정렬 속성을 부여한 후 이미지에 적용시키는 방법이었다.**

```css
/* 새 css class */
.center {
  display: block;
  margin: auto;
}
```

css 클래스를 만든 후, 마크다운 파일로 와서 이미지에 적용시킨다.
```
![nachoi](/assets/post-img/171123-nachoi-300.jpg){: .center}
```


<br>
두 번째 방법은 조금 더 단순히, **html의 `<center>` 기능을 이용하는 것이다.** 이미지 앞뒤로 center 속성을 적용하면 가운데 정렬을 할 수 있다. 텍스트에도 같은 방법으로 사용할 수 있다.

```html
<center><img src="/img/myImg.png" width="300" height="300"></center>
```

<center><img src="/assets/post-img/171123-nachoi-300.jpg" width="300" height="300"></center>

<br><br>

두 번째 방법이 간단했지만, `left` `right` 오른쪽 정렬 등 다른 기능을 이용하려면 직접 css로 custom 하는 것도 좋아보인다.


### References
- https://nayunhwan.github.io/dev/2016/12/11/Img-centering/
-  https://stackoverflow.com/questions/3912694/using-markdown-how-do-i-center-an-image-and-its-caption
- https://thornelabs.net/2014/11/30/centering-images-with-jekyll-and-markdown.html
- http://gumpcha.github.io/blog/github-issue-image-resize/
- https://www.w3schools.com/css/css_align.asp
- ~~엄청난 시행착오~~
