---
layout: post
title:  "[Docker] 개념"
date:   2018-07-10
author: souk0721
categories: studynote
tags: [basic,docker]
comments: true
---


# 필요 개념
  - `Docker(도커)` : 컨테이너 기반의 오픈소스 가상화 플랫폼입니다.
  - `컨테이너` : 
  1. 다양한 프로그램, 실행환경을 컨테이너로 추상화하고, 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해줍니다.
  2. 백엔드 프로그램, DB, 메시지 큐등 어떤 프로그램도 컨테이너로 추상화 할수 있습니다.
  3. 격리된 공간에서 프로세스가 동작하는 기술 입니다. 즉 서버의 제원을 필요한 만큼 가져와 독립 된 공간에서 원하는 만큼 사용하는 기술 입니다.
  - `이미지`
  1. **컨테이너 실행에 필요한 파일과 설정값 등을 포함**하고 상태값을 가지지않고 변하지 않습니다.
  2. 컨테이너는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값은 컨테이너에 저장됩니다.
  3. 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아 있습니다.<br>
  **`요약`** 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 더 이상 의존성 파일을 컴파일 하고 **이것저것 설치할 필요가 없습니다.**

### 참조
  참조 1. [https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)


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
