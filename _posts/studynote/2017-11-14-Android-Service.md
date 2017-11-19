---
layout: post
title:  "Service 안드로이드 서비스"
author: Yena Choi
categories: studynote
tags: [android, service, pendingintent]
---

## Service
UI에 영향을 주지 않거나, 백그라운드에서 비교적 오래 걸리는 작업을 수행할 때 사용.

1. **Loader** 를 쓸때-> 액티비티에서만 로딩 작업이 쓰일때, 생명주기와 연관돼있을때, UI에 영향을 줄때
2. **Service** 를 쓸때-> 데이터의 최종 결과가 UI에 영향을 주지 않을 때.


### Service를 시작하는 방법

1. Start(수동 시작)
  - startservice() : 강제 시작 할수 있으나, 컴포넌트와 통신을 하지는 않음.
  - 특정 조건이 만족됐을때 반응하려면 `JobSservice`를 만들고 `JobScheduler` 실행.

2. Schedule

3. Bind
  - 클라이언트-서버와 같은 역할. Activity를 Cilent로, Service를 Server로 취급.
  - 액티비티와 커뮤니케이션 할수 있음. ex) MP3플레이어. UI를 업데이트 해야함.

*** 1,2,3 하나 이상의 속성을 가질수 있음.


### Service의 생명주기
- `startService()`
- `onCreate()`
- `onStartCommand()`(실제 직업 수행, AsyncTask나 쓰레드 O)
- `stopService` (**\*중요. 할일이 끝나면 이걸로 알려야함. 꼭 호출!**)
- `onDestroy()`


### IntentService
- 서브 스레드에서 실행되는 서비스. (다른 Service 등등은 무조건 메인쓰레드에서 돌아야함.)
```java
Intent intent = new Intent(this, MyIntentservice.class);
startService(intent);
```
- 차례대로 실행되기 때문에 순서가 있는 작업에 적합하다.
- SharedPreferences 업데이트 하거나, 서버에 데이터 요청(ex.날씨) 등에 사용.

1. IntentService를 상속하는 클래스 생성.

2. 그 안에서 onHandleIntent 오버라이드 하여 실제 수행할 작업 작성.

3. 새로운 인텐트를 만들어서 방금 만든 Service class를 넘겨줌.

4. startService(intent)


## Background Service

### PendingIntent (펜딩 인텐트, 보류 인텐트)
- 다른 앱에서 내 앱 접근이 가능하도록 할때, 근데 System Service 처럼 권한을 바꿀 수 없을 떄 사용.
- intent를 PendingIntent로 감싸서 다른곳에 넘겨줄 수 있고, 거기서 실행가능!
- 고유 앱처럼 사용할 수 있음. (Service, Private Activity, 혹은 앱 비실행 중일 때에도 사용 가능)
- `getActivicy`, `getService`, `getBroadcast` 등등의 메소드 보유
  - `getActivity` 파라미터 :  
  1. Context
  2. 고유ID
  3. intent(실제 수행할 작업)
  4. flag


*사용 예시 (PendingIntent를 생성하여 Notification 수행)*

1. PendingIntent 생성 역할을 하는 메소드 생성
2. 실제 수행할 intent를 생성하고, PendingIntent.getActivity를 return
3. NotificationCompat.Builder 변수를 생성 후 color, title, vibrate 등 설정
4. NotificationManager 변수를 생성하고, `(NotificationManager)` 형식으로
`context.getSystemService(Context.NOTIFICATION_SERVICE)` 객체를 변수에 저장
5. manager.notify(고유ID, builder.build());



### Notification에 Action 추가
- 안드로이드 Jellybean 이후 버전에서는 상단에 앱이 표시됨 (Priority_HIGH)
- 안드로이드 Nougat 이후 버전에서는 알림에서 간단한 액션 가능!

```java
NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
.set //세팅 추가
.addAction(methodAction(context));

private static Action methodAction(Context context) {
  PendingIntent actionPendingIntent;
  Action action = new Action(icon, "액션타이틀", actionPendingIntent)
  return action;
}
```

- 동시에 두개 이상의 intent를 실행하려면 `Broadcast` 기능을 써야하는 듯하다.
