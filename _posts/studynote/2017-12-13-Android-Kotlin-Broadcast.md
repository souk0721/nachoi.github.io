---
layout: post
title:  "[Android] 안드로이드 Broadcast"
author: Yena Choi
categories: studynote
tags: [android]
---

## Broadcast
안드로이드 앱에서는 시스템이나 다른 앱으로 Broadcast를 보내거나 받을 수 있다. 이 브로드캐스트는 특정 이벤트가 발생했을 때 전송된다. 예를 들어, 안드로이드 시스템은 부팅되거나 충전되는 등의 상황에서 Broadcast를 보낸다. 앱에서는 데이터 다운로드 완료가 될 때처럼, 다른 곳에 알려주고 싶을 때 Broadcast를 보낼 수 있다.

Broadcast가 '방송하다'라는 의미을 가진 것처럼, 특정한 Broadcast를 수신하기 위해서는 그것을 등록(register)해두어야 한다. 쉽게 생각해서, 특정 라디오 채널 방송을 듣기 위해 주파수를 맞추는 것과 유사하다. 앱에서 여러 개의 Broadcast를 쓸 수 있지만, 너무 많을 경우 백그라운드 작업이 계속 되어 메모리가 낭비될 수 있다.
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
위와 같이 receiver를 정의하면 `intent-filter`를 통해 Broadcast가 액션을 특정할 수 있다. 여기서는 충전기 연결/해제 두 가지 액션을 확인하도록 설정했다.
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
Context에 등록하는 방식의 BroadcastReceiver는 그 context를 사용할 수 있을 때까지만 유효하다. 만약 `Activity`의 context에 등록했다면 그 액티비티가 `onDestroy()`될 때까지 유효하며, `Application` context에 등록했다면 App이 종료될 때까지 유효할 것이다.

```java
/* MainActivity.java */
public class MainActivity extends AppCompatActivity {

    BroadcastReceiver receiver;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        IntentFilter filter = new IntentFilter();
        filter.addAction(Intent.ACTION_POWER_CONNECTED);
        filter.addAction(Intent.ACTION_POWER_DISCONNECTED);
        /* 충전기 연결/해제 액션을 intentFilter에 등록 */

        receiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                String message = ("액션 발생! " + intent.getAction());
                Toast.makeText(context, message, Toast.LENGTH_LONG).show();
            }
        };
    }
}
```
<br><br>

#### Unregister BroadcastReceiver
코드로 Broadcast를 등록할 때의 주의할 것은 **receiver의 사용이 끝날 때에는 등록을 해제(unregister)** 해주어야 한다는 점이다. App이 종료되었는데도 receiver가 작동한다면, 디바이스에서 불필요한 메모리를 계속 사용하게 되기 때문이다.

Receiver를 등록/해제하는 lifecycle의 시점은 일관성 있게 작성해야 한다. `onCreate(Bundle)`에서 등록했다면 `onDestroy()`에서 해제해주어야 하고, `onResume()`에서 등록했다면, 중복 등록을 막기 위해 `onPause()`에서 해제해주어야 한다. 또한 unregister 하기 전에, `if` 등으로 receiver null 체크를 할 필요가 있다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    ...
    registerReceiver(receiver, filter);
    super.onCreate(savedInstanceState);
}

@Override
protected void onDestroy() {
    if (receiver != null) {
        unregisterReceiver(receiver);
        receiver = null;
    }
    super.onDestroy();
}
```
<br>

#### LocalBroadcastManager
시스템 혹은 다른 App이 아니라, 하나의 App 내에서 발생한 Broadcast를 수신하려면 `LocalBroadcastManager`를 사용해야 한다. (*ex.두 액티비티 간 송수신되는 Broadcast*)

```java
LocalBroadcastManager.getInstance(Context).registerReceiver(BroadcastReceiver, IntentFilter)
```
<br><br>

### References

- https://developer.android.com/guide/components/broadcasts.html
- https://stackoverflow.com/questions/4805269/programmatically-register-a-broadcast-receiver
