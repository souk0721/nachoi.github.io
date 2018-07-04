---
layout: post
title:  "[PowerShell] 프린터 추가하기"
date:   2018-07-03
author: souk0721
categories: studynote
tags: [powershell, printer]
comments: true
---


# 필요 개념
  - `클라우드PC(가상PC)` : (VDI : Virtual Desktop Infrastructure)
  - `Local PC` : 클라우드 PC 환경을 띄운 PC
  

### When?
PC 사용자들이 모르게 윈도우 시작 시에 자동으로 프린터를 설치 하고 설치된 프린터를 기본 프린터로 만들어 보겠습니다.


### How?
- `배치파일` : [배치 파일 만들기]({% post_url studynote/2018-07-02-basic-01 %})
- `Powershell` : [파워쉘 기본 만들기]({% post_url studynote/2018-07-02-PowerShell-print-02 %})
- `작업 스케줄러` : [작업 스케줄러 만들기]({% post_url studynote/2018-07-03-basic-job-01 %})

# 계속~

<!-- ### Code?
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
 그럴 경우 [`보안문제 해결`]({% post_url  studynote/2018-07-04-Tip-PowerShell-01 %})참고




