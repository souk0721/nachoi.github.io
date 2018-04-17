---
layout: post
title:  "[Github] Login Error 깃헙 자동 로그인 실패"
author: Yena Choi
categories: studynote
tags: [github]
---

### 오류: HttpRequestException encountered

Github을 **Windows** 환경에서, **터미널** 로, **https** 주소로 로그인해서 쓰고 있다. 그런데 오랜만에 접속해보니 해봐도 다음과 같은 로그인 오류가 발생했다.

```
fatal: HttpRequestException encountered.
    이 요청을 보내는 동안 오류가 발생했습니다.
Username for 'https://github.com':
Password for ... :
```

<img src="/assets/post-img18/180418-01.jpg" width=507 height=133>


자동 로그인이 해제되었다. github 아이디와 패스워드를 입력하면 `push`는 가능하다. 하지만 언제까지나 이렇게 수동 로그인 할 순 없으므로 구글링을 해봤는데, 황당하게도 이 문제를 **git 버전을 업그레이드 하는 것으로 해결했다.** 실제로 2.14 버전에서 2.17 버전으로 업그레이드를 하니 더 이상 계정 정보를 묻지 않고 `push`가 가능해졌다.

git 업데이트는 [공식 홈페이지](https://gitforwindows.org)를 통해 직접 다운받거나, 터미널을 통해 `git update` 명령어를 실행하면 할 수 있다.

<br><br>

### SSH 방식으로 로그인 하는 법
이리저리 방법을 찾아보다가 SSH 주소로 로그인 하는 방법이 설명된 곳을 찾았다. [오픈 튜토리얼스](https://opentutorials.org/module/2676/15433) 링크 첨부.

HTTPS 방식이 아닌 SSH 방식으로 로그인 하는 것인데, `ssh-keygen` 명령어를 통해 키를 만들어서 등록하는 방법이다. 즉, github 계정 정보에 ssh 키를 등록해두면 해당 키가 설치된 PC에서는 로그인 없이 사용이 가능한 방식이다. 맥 혹은 리눅스, 유닉스 OS에서 사용하는 법도 동영상으로 자세하게 설명되어 있다.

<br><br>


### References
- https://github.com/Microsoft/Git-Credential-Manager-for-Windows/issues/488
- https://opentutorials.org/module/2676/15433
