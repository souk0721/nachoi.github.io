---
layout: post
title:  "[Docker] 설치"
date:   2018-07-10
author: souk0721
categories: studynote
tags: [basic,docker]
comments: true
---


# 필요 개념
  - [`Docker 개념`]({% post_url  studynote/2018-07-10-basic-docker-01 %})
  - `리눅스` : OS 중하나 검색 해서 찾아보시는게 나으실 듯합니다. [`linode에서 ubuntu 호스팅하기`]({% post_url  studynote/2018-07-10-basic-ubuntu-01 %})
  - `putty` : 리눅스 SSH 접속 프로그램  [`링크`]({% post_url  studynote/2018-07-10-basic-putty-01 %})
  

### 설치환경
- OS : Ubuntu : 16.04 LTS
  
### 설치
1. Docker 설치 
```
curl -fsSL https://get.docker.com/ | sudo sh
```
2. Docker 버젼확인
```
sudo docker version
```
**18-07-10기준 : 18.05.0-ce**

  참조 1. [https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)


<!-- ### How?
1. 작업 등록
 - `시작`->`실행`->`compmgmt.msc`엔터 
 - `시스템 도구`->`작업 스케줄러`에서 마우스 오른쪽 버튼 ->`작업 만들기`
 - 일반 탭s
 ![job01](/assets/post-img-18-07/job-01.JPG)
 - 트리거 탭
 ![job02](/assets/post-img-18-07/job-02.JPG)
 - 조건 탭
 ![job03](/assets/post-img-18-07/job-03.JPG)
 - 실행 (마우스 오른쪽 버튼 누루고 실행)
 ![job04](/assets/post-img-18-07/job-04.JPG)
 -->
