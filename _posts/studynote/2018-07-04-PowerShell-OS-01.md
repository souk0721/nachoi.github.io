---
layout: post
title:  "[PowerShell] 윈도우 OS, Bit 나눠서 선택하기"
date:   2018-07-04
author: souk0721
categories: studynote
tags: [powershell]
comments: true
---


# 필요 개념
  - `OS` : 윈도우 7, 윈도우 10, 등등
  - `Bit` : 32Bit, 64Bit  

### When?
`원 클릭 통합드라이버`를 만들려고 할 때 사용자PC가 윈도우7,윈도우10인지 32Bit,64Bit인지 선택하는 스크립트를 작성해 보겠습니다.


### How?
- `Powershell` : [파워쉘 기본 만들기]({% post_url studynote/2018-07-02-PowerShell-print-02 %})

### Code?
파워쉘 텍스트 창에 아래의 코드를 추가하시면 됩니다.

```
# 자신의 PC 환경의 OS이름을 반환 
# ex)필자는 윈도우10
# 반환 값 : Microsoft Windows 10 Pro|C:\Windows|\Device\Harddisk0\Partition2
$OSCaption = (Gwmi Win32_OperatingSystem -ComputerName .).Name

#시스템이 64bit일경우 $OSbit가 True를 반환
$OSbit = (Gwmi Win32_ComputerSystem -ComputerName .).SystemType -match ‘(x64)’

#윈도우7일경우
 if($OSCaption -match "windows 7"){
               Write-Host 'OS=windows7' -fore red
                  #64bit일 경우, OSbit가 True일경우
                 if($OSbit){
                    Write-Host 'OSbit=64' -fore red
                    #여기에 윈도우7,64Bit용 프로그램 설치
                 }
                #32bit일 경우
                 else{
                    Write-Host 'OSbit=32' -fore red
                    #여기에 윈도우7,32Bit용 프로그램 설치
                 }
            
}
#윈도우 10일 경우
 if($OSCaption -match "windows 10"){
               Write-Host 'OS=windows10' -fore red
                 #64bit일 경우, OSbit가 True일경우
                 if($OSbit){
                    Write-Host 'OSbit=64' -fore red
                    #여기에 윈도우10,64Bit용 프로그램 설치
                
                 }
                #32bit일 경우
                 else{
                    Write-Host 'OSbit=32' -fore red
                    #여기에 윈도우10,32Bit용 프로그램 설치
                 }
            
}
```

### Tip?
 - 간혹 보안문제로 파워쉘이 실행이 않될 때가 있습니다. 
 그럴 경우 [`보안문제 해결`]({% post_url  studynote/2018-07-04-Tip-PowerShell-01 %})참고




