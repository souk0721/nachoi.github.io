---
layout: post
title:  "[PowerShell] 기본 프린터 설정하기"
date:   2018-07-02
author: souk0721
categories: studynote
tags: [powershell, printer]
comments: true
---


# 필요 개념
  - `클라우드PC(가상PC)` : (VDI : Virtual Desktop Infrastructure)
  - `Local PC` : 클라우드 PC 환경을 띄운 PC
  - `배치파일` : 확장자가 `.bat`이며 실행가능한 파일 
  - `Powershell` : <del>자동화(Automation)을 위해 많이 사용되는, Windows 운영체제의 API를 사용가능한, 마이크로소프트의 확장 가능한 윈도 파워셸(Windows PowerShell)은 마이크로소프트가 개발한 확장 가능한 명령줄 인터페이스(CLI) 셸 및 스크립트를 지원하는 명령어 인터프리터이다.</del>
  `[요약]`윈도우에서 명령어 한줄로 쉽게 내가원하는 것을 얻어낼 수 있는 인터프리터

### When?
`Local PC`에서 `클라우드 PC환경`을 띄우게 되면 `클라우드 PC`는 `Local PC`에
물려있는 프린터들을 가져와서 `클라우드 PC환경`에 별도 설치없이 프린터를 사용할 수 있습니다.

그런데 `Local PC환경`에서 와는 다르게 기본 프린터가 `클라우드 PC`에서는 다르게 잡혀있을 수 있습니다. 

인쇄 할 때 그냥 바꿔서 사용하면 상관 없지만, 기업의 사용자들 대부분은 애기들 이기 때문에 기본 프린터로 설정 되지 않으면 거의 `클라우드 PC`에 잡힌 기본 프린터로 인쇄를 날리게 되더라구요.

### How?
- [파워쉘 환경 만들기 링크]({% post_url jekyll/2018-07-02-PowerShell-print-02 %})

### Code?
예를 프린터명이 `Sindoh N610_410 MF4000 Series PCL6`일 경우에 대하여 코드를 작성하겠습니다.

```
function PrinterSetting{
$Printers =  Get-WmiObject -Class Win32_Printer -ComputerName .
    foreach($val in $Printers){
     $PrinterName = $val.name

     if($PrinterName -like "Sindoh N610_410 MF4000 Series PCL6" ){
       Write-Host 'Print Find' -fore red
       $val.SetDefaultPrinter()
       
     }
  }
}
PrinterSetting
```
위의 코드에서 "`Sindoh N610_410 MF4000 Series PCL6`" 만 자신의 PC환경 프린터명으로 변경하면 됩니다.

### Tip?
 - 간혹 보안문제로 파워쉘이 실행이 않될 때가 있습니다. 
 그럴경우 아래의 명령줄에서 `c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1`를 수정해서 편리하게 `배치파일`로 만들어 사용하시면 문제 없이 관리자 모드로 실행 될 것입니다.
 - 창 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1""' -Verb RunAs}";
 ```
 - 백 그라운드 모드
 ```
 PowerShell -Command "& {Start-Process PowerShell -windowstyle hidden -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\LGCNS_PCL_DRV_(LOCAL).ps1""' -Verb RunAs}";
 ```




