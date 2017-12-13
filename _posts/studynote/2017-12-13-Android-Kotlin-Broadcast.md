---
layout: post
title:  "[Android][Kotlin] 안드로이드 Broadcast"
author: Yena Choi
categories: studynote
tags: [android]
---

## Broadcast
안드로이드 앱에서는 시스템이나 다른 앱으로 Broadcast를 보내거나 받을 수 있다. 이 브로드캐스트는 특정 이벤트가 발생했을 때 전송된다. 예를 들어, 안드로이드 시스템은 부팅되거나 충전되는 등의 상황에서 Broadcast를 보낸다. 앱에서는 데이터 다운로드 완료가 될 때처럼, 다른 곳에 알려주고 싶을 때 Broadcast를 보낼 수 있다.

Broadcast가 '방송하다'라는 의미을 가진 것처럼, 특정한 Broadcast를 수신하기 위해서는 그것을 등록(register)해두어야 한다. 쉽게 생각해서, 특정 라디오 채널 방송을 듣기 위해 주파수를 맞추는 것과 유사하다.

앱에서 여러 개의 Broadcast를 쓸 수 있지만, 너무 많을 경우 백그라운드 작업이 계속 되어 메모리가 낭비될 수 있다.
<br><br>

## Broadcast 수신
브로드캐스트 수신 방법에는 두 가지 방법이 있다.
- `manifest`에 선언하기
- `context`에 등록하기

### 1. manifest에 선언하기   
`BroadcastReceiver`를 manifest에 선언하면, 시스템은 Broadcast가 수신될 때 앱을 실행한다. 어떤 이벤트에 대해 알림을 받을 것인지 매니페스트에 정의해주면 된다.

```java
/* AndroidManifest.xml */
<receiver
  android:name=".MyBroadcastReceiver"
  android:exported="true">
  <intent-filter>
      <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
      <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
  </intent-filter>
</receiver>
```
위와 같이 receiver를 정의하면 `intent-filter`를 통해 Broadcast가 액션을 특정할 수 있다. 여기서는 충전기 연결/충전기 연결해제 두 가지 액션을 확인하도록 설정했다.
<br>

그 후 `BroadcastReceiver`를 extend(상속)받는 클래스를 하나 생성하고, `onReceive(Context, Intent)` 메소드를 override(재정의)한다.

```java
public class MyBroadcastReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        String message = ("액션 발생! " + intent.getAction());
        Toast.makeText(context, message, Toast.LENGTH_LONG).show();
    }
}
```
<br>

<center>
  <a href="../../../../assets/post-img/171213-charge.png" target="_blank">
    <img src="/assets/post-img/171213-charge.png" width="100%" height="100%">
  </a>
</center>
<br>
배터리 충전 상태를 변경하면 시스템 이벤트를 인식하고 Broadcast를 보낸다_._
<br><br>

### 2. context에 등록하기


<br><br>

### References

- https://developer.android.com/guide/components/broadcasts.html
