---
layout: post
title:  "[PowerShell] 통합 프린터 드라이버 만들기"
date:   2018-07-03
author: souk0721
categories: studynote
tags: [powershell, printer]
comments: true
---


# 필요 개념
- `배치파일` : [배치 파일 만들기]({% post_url studynote/2018-07-02-basic-01 %})
- `Powershell` : [파워쉘 기본 만들기]({% post_url studynote/2018-07-02-PowerShell-print-02 %})
- `VBS` : [VBS 만들기]({% post_url studynote/2018-07-04-basic-vbs-01 %})
  

### When?
하나의 파일로 여러 운영체제에서 드라이버를 설치하기 위해 코드를 작성했습니다.

### How?
1. 지정 된 경로(여기서는 `c:\pcldrv`)에 OS, Bit별 드라이버 분류 하기[그림1]참고
- windows7 : `c:\pcldrv\win7\PCL32`, `c:\pcldrv\win7\PCL64`
- windows10 : `c:\pcldrv\win10\PCL32`, `c:\pcldrv\win10\PCL64`
  ![print03](/assets/post-img-18-07/Pinter-03.JPG)

2. 프린트 포트 확인 하기(이미설치 된 포트가 있어야함.없으면 설치필요)
- `[Code]`에서 **$PrinterPortName** 변경
 ![print01](/assets/post-img-18-07/Pinter-01.JPG)
3. 설치 .INF 파일명 변경
- 제조사 프린터 드라이버의 `.INF` 파일명
- `[Code]`에서 설치 드라이버에 맞는 **$InfName** 변경 

4. 설치 프린트 드라이버명 변경
- `[Code]`에서 **$DriverName** 변경 
 ![print01](/assets/post-img-18-07/Pinter-02.JPG)
5. `[Code]`를 PowerShell 파일로 만들기
- [파워쉘 기본 만들기]({% post_url studynote/2018-07-02-PowerShell-print-02 %})
6. `생성한 PowerShell`파일 배치파일로 실행하기
- 배치파일을 만들고 아래의 명령줄 삽입 `c:\pcldrv\test.ps1`자신에 맞게 수정
- [배치 파일 만들기]({% post_url studynote/2018-07-02-basic-01 %})
```
PowerShell -Command "& {Start-Process PowerShell -windowstyle hidden -ArgumentList '-NoProfile -ExecutionPolicy Unrestricted -File ""c:\pcldrv\test.ps1""' -Verb RunAs}";
```
7. 선택
- CMD창 없이 백 그라운드로 실행하려면
[VBS 만들기]({% post_url studynote/2018-07-04-basic-vbs-01 %})


 
<br>


### Code?
```
#이건 그냥 디폴트로 자기자신을 가리킴.
$ComputerList = @(“.”)

#프린터 포트이름(이미 생성 된 것을 바탕으로 한다) - 자신의 환경에 맞게 변경
$PrinterPortName = “165.244.227.25”

#프린터명(아무거나 상관 없음) - 자신의 환경에 맞게 변경
$PrinterCaption = "MPS Driver" 

#프린터를 설치할 선택해야 하는 드라이버명[그림2]에서 알 수있음. - 자신의 환경에 맞게 변경
$DriverName = “SINDOH N600 Series PCL-8”

#드라이버 경로
$Win7_64bit = "c:\pcldrv\win7\PCL64\"
$Win7_32bit = "c:\pcldrv\win7\PCL32\"
$Win10_64bit = "c:\pcldrv\win10\PCL64\"
$Win10_32bit = "c:\pcldrv\win10\PCL32\"

#설치파일 Inf 파일명 - 자신의 환경에 맞게 변경
$InfName = "KOAYFJA_.inf"

# 자신의 PC 환경의 OS이름을 반환 
# ex)필자는 윈도우10
# 반환 값 : Microsoft Windows 10 Pro|C:\Windows|\Device\Harddisk0\Partition2
$OSCaption = (Gwmi Win32_OperatingSystem -ComputerName .).Name

#시스템이 64bit일경우 $OSbit가 True를 반환
$OSbit = (Gwmi Win32_ComputerSystem -ComputerName .).SystemType -match ‘(x64)’

function OSPath{
#윈도우7일경우
 if($OSCaption -match "windows 7"){
               Write-Host 'OS=windows7' -fore red
                  #64bit일 경우, OSbit가 True일경우
                 if($OSbit){
                    $DriverInf =  $Win7_64bit + $InfName
                    Write-Host 'OSbit=64' -fore red
                    return $DriverInf

                
                 }
                #32bit일 경우
                 else{
                    $DriverInf =  $Win7_32bit + $InfName
                    Write-Host 'OSbit=32' -fore red
                    return $DriverInf
                    
                 }

            
}
#윈도우 10일 경우
 if($OSCaption -match "windows 10"){
               Write-Host 'OS=windows10' -fore red
                 #64bit일 경우, OSbit가 True일경우
                 if($OSbit){
                 $DriverInf =  $Win10_64bit + $InfName
                 Write-Host 'OSbit=64' -fore red
                 return $DriverInf
                
                 }
                #32bit일 경우
                 else{
                 $DriverInf =  $Win10_32bit + $InfName
                 Write-Host 'OSbit=32' -fore red
                 return $DriverInf
                    
                 }
            
    }
}

Function CreatePrinter {
param ($PrinterCaption, $PrinterPortName, $DriverName, $ComputerName,$DriverInf)
        
       #프린터를 설치한다.
       CMD.EXE /C "printui.exe /if /b `"$PrinterCaption`" /f `"$DriverInf`" /r `"$PrinterPortName`" /m `"$DriverName`""
       Write-Host 'Print Create' -fore red
       sleep 5

       #설치된 프린터 오브젝트를 담는다.
       $Printers =  Get-WmiObject -Class Win32_Printer -ComputerName .
       foreach($val in $Printers){
       $PrinterName = $val.name
       
       #설치된 프린터 중에 $PrinterCaption의 프린터명이 있다면
       if($PrinterName -like $PrinterCaption ){
       Write-Host 'New Print Find' -fore red
       
       #해당 프린터를 기본 프린터로 지정한다.
       $val.SetDefaultPrinter()
       Write-Host 'Print Basic Setting' -fore red
       }

       }


   
  

}

foreach($computer in $ComputerList){
#OSPath 함수를 실행하여 드라이버 경로를 retrun 한다.
$DriverInf로 = OSPath

#OSPath에서 return받은 드라이버 경로 및 프린터 설치 설정 값을 가지고 프린터를 설치한다.
CreatePrinter -PrinterPortName $PrinterPortName -DriverName `
$DriverName -PrinterCaption $PrinterCaption -ComputerName $computer `
-DriverInf $DriverInf

}




```
### Tip?




