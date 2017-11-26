---
layout: post
title:  "Github에 안드로이드 프로젝트 올리기"
author: Yena Choi
categories: studynote
tags: [github, android, initialize, repository]
---
안드로이드 새 프로젝트를 만들고 Github의 새 repository 에서 버전 관리를 하고 싶은데, 막상 하려니 헷갈린다. 깃허브에 저장소를 추가하고, 새 안드로이드 프로젝트 파일을 올리는 방법을 공부할 겸 정리해보았다.

## Github repository 생성
1. `https://github.com/<user_id>`에서 Repository 탭으로 이동한다.

2. New를 클릭하여 새로운 저장소를 만든다.

3. 확인을 누르면 `https` 혹은 `ssh` 형식의 url을 찾을 수 있다.

  <center>![img](/assets/post-img/171126-up3.JPG){: width="70%" height="70%"}</center>

  상단에서 https나 ssh 형식을 고르고, 오른쪽 아이콘을 클릭하면 **repository의 url** 을 클립보드에 복사할 수 있다.

<br><br>

## Android Project 생성
1. New Project 를 생성한다. 프로젝트의 위치는 git을 주로 관리할 때 사용하는 폴더로 지정했다. (`C:\git`)
<center>![img](/assets/post-img/171126-up1.JPG){: width="70%" height="70%"}</center>
<br>

2. 버전 관리를 위해 메뉴 창에서 **VCS - Enable Version Control Intergration** 클릭, Git 선택 한 후 확인한다.
<center>![img](/assets/post-img/171126-up2.JPG){: width="70%" height="70%"}</center>

<br><br>

## Terminal 을 이용하여 Git에 업로드하기

1. 디렉토리를 새 프로젝트가 있는 곳으로 이동한다.
```
cd C:\git\UploadTest\
```
<br>

2. 새로 만든 프로젝트의 모든 파일을 Index에 추가하려면 `add` 명령어에 `-A`를 붙여 입력한다. 여기서 Index란, 실제 작업하던 파일에서 최종 업로드를 확정 하기 전에 거쳐 가는 준비 단계(staging area)의 역할을 한다.
```
git add -A
```
<br>

3. Index에 있는 변경사항을 확정하기 위해 `commit` 한다. `-m` 기능을 통해 이번 커밋에 포함된 내용을 설명하는 메세지를 작성한다. 여기서는 새 프로젝트를 초기화(initialize) 한다는 내용을 작성했다.
```
git commit -m "Init project"
```
commit 완료된 내용은 준비 단계인 **Index** 에서, 최종 확정본인 **HEAD** 로 반영된다.
<br>

4. 변경된 내용을 서버에 올려야 하지만, 새로운 저장소이기 때문에 아직 git은 서버 주소를 모른다. git에 업로드할 repository의 url을  `origin` 이라는 이름으로 새로 추가한다. url은 위에서 설명한 것처럼, repository 페이지에서 url 오른쪽 아이콘을 눌러 복사할 수 있다.
```
git remote add origin https://github.com/nachoi/upload-test.git
```
성공적으로 추가 되었는지 확인하려면 `git remote -v` 명령어를 사용한다. `origin`의 이름으로 `fetch`와 `push`에 사용되는 url이 보이면 완료.
<br>

5. 이제 최종적으로 `push` 하여 서버로 업로드한다.
```
git push origin master
```
특별히 지정한 branch가 없기 때문에 기본 branch인 master branch로 업로드한다.
<br>

6. `https://github.com/<user_id>/<repo_name>`에 접속했을 때 Android Project 파일들이 보이면 업로드 완료.



### References
- http://rogerdudler.github.io/git-guide/index.ko.html
