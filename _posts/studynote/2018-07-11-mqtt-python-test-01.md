---
layout: post
title:  "[MQTT] mosquitto - python 테스트"
date:   2018-07-11
author: souk0721
categories: studynote
tags: [mqtt,python]
comments: true
---


# 사전 준비
  - [`Python 설치`]({% post_url  studynote/2018-07-11-basic-python-01 %})
  - 서버가 없을 경우 : [`Ubuntu-Docker-MQTTbroker 설정`]({% post_url  studynote/2018-07-10-mqtt-docker-01 %})
  

### 설치환경
- OS : Ubuntu : 16.04 LTS
- Docker : 18.05.0-ce
  
### 설치
- Ubuntu 서버에 MQTT broker(mosquitto)가 서비스 되어 있다는 전제하에 Python으로 테스트 하겠습니다.

- python MQTT 라이브러리 설치

```
pip install paho-mqtt
``` 

- Pub(발행) - 코드  

```
import paho.mqtt.publish as publish

msgs = \
[
    {
        'topic':"/seoul/yuokok",
        'payload':"multiple 1"
    },

    (
        "/seoul/yuokok",
        "multiple 2", 0, False
    )
]
publish.multiple(msgs, hostname="test.mosquitto.org")
#Topic /seoul/yuokok 에 문자값 multiple 1, multiple 2를 발행한다.
```

- Sub(구독) - 코드

```
import paho.mqtt.client as mqtt

# The callback for when the client receives a CONNACK response from the server.
def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))

    # Subscribing in on_connect() means that if we lose the connection and
    # reconnect then subscriptions will be renewed.
    client.subscribe("/seoul/yuokok") # Topic /seoul/yuokok을 구독한다.

# The callback for when a PUBLISH message is received from the server.
def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

client.connect("test.mosquitto.org") # - 서버 IP '테스트를 위해 test.mosquitto.org'로 지정

# Blocking call that processes network traffic, dispatches callbacks and
# handles reconnecting.
# Other loop*() functions are available that give a threaded interface and a
# manual interface.
client.loop_forever()
```

- `sub.py`를 만들어 위의 코드를 삽입하고 `python sub.py`를 실행하고 `pub.py`를 만들어 `python pub.py`를 실행하면, `sub.py`를 실행한 커맨드 창에 아래와 같이 나온다.

```
/seoul/yuokok multiple 2
/seoul/yuokok multiple 1
```




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
 -->
