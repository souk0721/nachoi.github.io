---
layout: post
title:  "[Server] linode에서 Ubuntu Server 호스팅하기"
date:   2018-07-10
author: souk0721
categories: studynote
tags: [basic,server,ubuntu,linode]
comments: true
---


# 필요 개념
  - `linode` : 리눅스 기반 서버 호스팅 업체

# 가입하기
- [`linode`](https://www.linode.com/)에서 `E-mail`, `ID`, `Pw` 입력하면 인증문자를 클릭하면 가입이 완료 됩니다.(7일까지 무료 사용을 보장한다고 하네요)
![[jpg1]](/assets/post-img-18-07/linode-sign-01.JPG)

- 가입 시 기입한 E-mail로 인증 문자가 갑니다. 
![[jpg1]](/assets/post-img-18-07/linode-sign-02.JPG)

- OK 클릭
![[jpg1]](/assets/post-img-18-07/linode-sign-03.JPG)

- 아래의 사진처럼 가입 폼 작성
![[jpg1]](/assets/post-img-18-07/linode-sign-04-form.JPG)

- 아래의 사진처럼 체크
![[jpg1]](/assets/post-img-18-07/linode-sign-05.JPG)

- 오른쪽 하단의 Add a Linode 클릭
![[jpg1]](/assets/post-img-18-07/linode-sign-07.JPG)

- 아래의 사진처럼 체크 (테스트용으로 월 5$)
![[jpg1]](/assets/post-img-18-07/linode-sign-08.JPG)

- 생성된 linode를 클릭
![[jpg1]](/assets/post-img-18-07/linode-sign-09.JPG)

- Deploy 클릭 후 아래의 사진과 같이 변경 후 Depoly
![[jpg1]](/assets/post-img-18-07/linode-sign-10.JPG)

# Putty를 이용한 SSH 접속
- [`putty 개념 및 설치`]({% post_url  studynote/2018-07-10-basic-putty-01 %})
- putty 접속 서버 IP를 linode에 맞게 변경 후, ID : root PW : linode에서 지정한 값 으로 접속하면 됩니다.




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

링크
 [`배치파일`]({% post_url  studynote/2018-07-02-basic-01 %})로 
 [https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

 -->
