---
layout: post
title:  "[PowerShell] 보안 문제 해결"
date:   2018-07-04
author: souk0721
categories: studynote
tags: [tip,powershell]
comments: true
---


<!-- # 필요 개념
  - `윈도우 작업 스케줄러` : 윈도우에서 사용자가 임의로 지정한 시간 및 방식에 따라서 자동으로 프로그램 및 스크립트를 사용하도록 해주는 윈도우 시스템 도구   -->

### When?
PowerShell을 실행하면 보안문제라며 실행이 않될 때가 있습니다.
그 때 참고하시면 됩니다.
  
### How?
- PowerShell 권한 주기 :

`시작` -> `실행` -> `CMD(관리자모드)` -> `powershell 엔터` -> `Set-ExecutionPolicy RemoteSigned 엔터` 
- 배치파일로 실행하기

아래의 명령줄에서 `c:\pcldrv\test.ps1`를 수정해서 편리하게 [`배치파일`]({% post_url  studynote/2018-07-02-basic-01 %})로 만들어 사용하시면 문제 없이 관리자 모드로 실행 될 것입니다.<br><br>
창 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\test.ps1""' -Verb RunAs}";
 ```
백 그라운드 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -windowstyle hidden -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\test.ps1""' -Verb RunAs}";
 ```

 