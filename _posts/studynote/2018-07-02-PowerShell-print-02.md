---
layout: post
title:  "[PowerShell] 기본"
date:   2018-07-02
author: souk0721
categories: studynote
tags: [powershell,basic]
comments: true
---


# 필요 개념
  - `Powershell` : <del>자동화(Automation)을 위해 많이 사용되는, Windows 운영체제의 API를 사용가능한, 마이크로소프트의 확장 가능한 윈도 파워셸(Windows PowerShell)은 마이크로소프트가 개발한 확장 가능한 명령줄 인터페이스(CLI) 셸 및 스크립트를 지원하는 명령어 인터프리터이다.</del>
  `[요약]`윈도우에서 명령어 한줄로 쉽게 내가원하는 것을 얻어낼 수 있는 인터프리터

### When?
`PowShell`을 사용하기 위해 알아아할 가장 기본적인 사항입니다.

### How?
1. 파워쉘 파일 만들기
 - 바탕화면에서 빈 공간에 `마우스 오른쪽 버튼 클릭 후 새로 만들기` -> `텍스트 문서`
 - 텍스트 문서 파일 확장자 `.ps1`으로 변환
2. 파워쉘 편집
 - 변환된 파일 마우스 오른쪽 버튼 -> 편집
 ![powshell01](/assets/post-img-18-07/powshell-print-01.jpg)
3. 파워쉘 스크립트 작성
 ![powshell02](/assets/post-img-18-07/powshell-print-02.jpg)
  **보안오류라고 스크립트 실행이 않될 떄**<br>
 시작 -> 실행 -> CMD(관리자모드실행) -> powershell 엔터 -> Set-ExecutionPolicy RemoteSigned
4. 파워쉘 스크립트 실행
 - 텍스트 창에 코드를 삽입후 단축키 `F5` 누루면 실행 됩니다.

**위의 1 ~ 4번 과정은 다른 파워쉘 스크립트 과정에서도 필요하니 꼭 숙지 하시길 바랍니다.**

### Tip?
 - 간혹 보안문제로 파워쉘이 실행이 않될 때가 있습니다. 
 그럴경우 아래의 명령줄에서 `c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1`를 수정해서 편리하게 [`ex)배치파일`]({% post_url jekyll/2018-07-02-tip-01 %})로 만들어 사용하시면 문제 없이 관리자 모드로 실행 될 것입니다.
 - 창 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1""' -Verb RunAs}";
 ```
 - 백 그라운드 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -windowstyle hidden -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1""' -Verb RunAs}";
 ```




