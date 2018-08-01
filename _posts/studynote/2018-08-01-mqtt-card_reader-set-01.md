---
layout: post
title:  "[CardReader] 리눅스 셋팅"
date:   2018-08-01
author: souk0721
categories: studynote
tags: [mqtt,python,linux,card_reader]
comments: true
---


# 사전 준비

  

### 설치환경
- OS : Ubuntu 18.04 LTS
- Docker : 18.05.0-ce
  
### 설치
1. python 패키지 설치
  - python3
```
sudo apt-get install python3 python-dev python3-dev \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip
```
  - python2
```
sudo apt-get install python-dev  \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip
```

2. python 라이브러리 설치(requirements.txt)
  - **python 2.7**
```
pycrypto==2.6.1
pyreadline==2.1
pyscard==1.9.7
pyserial==3.4
nyamuk==0.2.0
```

3. CardReader 드라이버 패키지 설치
  - 리눅스 터미널
```
sudo apt-get install pcscd pcsc-tools libusb-dev \
     libpcsclite1 libpcsclite-dev dh-autoreconf
```
  - 테스트
```
pcsc_scan
```

4. RFIDIOt 설치
  - git clone
```
git clone https://github.com/AdamLaurie/RFIDIOt
```

5. Card_Reader 테스트
  - cardselect.py
```
python cardselect.py
```

### driver_option_ccid_exchange_authorized 오류발생 시

```
  edit /etc/libccid_Info.plist
  
  Search for:
  ifdDriverOptions
  0x0000
  
  And change it to:
  ifdDriverOptions
  0x0001
```
# Source

## nymuk_test.py
```
#
# Nyamuk Publish Test
#

import sys
from nyamuk import nyamuk
import nyamuk.nyamuk_const as NC
from nyamuk import event

# Assign Server/Client/Payload details
server_ip = "test.mosquitto.org"
client_id = "test_client"
topic = "/nyamuk/test"
payload = "Horay, it works!"

# Connect to the MQTT Server
ny = nyamuk.Nyamuk(client_id, server = server_ip)
print "Connecting..."
rc = ny.connect()

# Check for a successful connection
if rc != NC.ERR_SUCCESS:
    print "Can't connect"
    sys.exit(-1)
print "Connected!"

# Publish the Payload
pb = ny.publish(topic, payload)

# Check for a successful publish
if pb:
    print "Publish Failed"
else:
    print "Publish Success!"
rc = ny.loop()

```


## read_and_publish.py
```
import sys
import time
import json
import rfidiot
from nyamuk import nyamuk
import nyamuk.nyamuk_const as NC
from nyamuk import event

# Functions
def open_reader():
	""" Attempts to open the card reader """
	try:
		card = rfidiot.card
		return card
	except:
		print "Couldn't open reader!"
		sys.exit()
		return None

def listen(card, interval):
	""" Listens for a card to be placed on the reader """
	
	while 1:	
		if card.select():
			data = json.dumps({"card_info":
				[{"card_id": card.uid}, {"timedate": get_time()}, {"action": "Placed"}]})
			ny.publish(ny_topic, data)
			ny.loop()
			break
		print 'Waiting: Card Placement'
		time.sleep(interval)
	return card.uid

def listen_remove(card, interval, card_id):
	""" Listens for a card to be placed on the reader """
	while 1:
		if not card.select():
			data = json.dumps({"card_info":
				[{"card_id": card_id}, {"timedate": get_time()}, {"action": "Removed"}]})
			ny.publish(ny_topic, data)
			ny.loop()
			break
		print "Waiting: Card Removal"
		time.sleep(interval)

def get_time():
	""" Returns a string with the time and date """
	return time.strftime("%a, %d %b %Y %H:%M:%S + 0000", time.gmtime())

ny_server = "test.mosquitto.org"
ny_client = "RFID-Reader"
ny_topic = "/nyamuk/test"

# Open the card reader
card = open_reader()
card_info = card.info('cardselect v0.1m')

# Connect to the MQTT Server
ny = nyamuk.Nyamuk(ny_client, server = ny_server)
print "Connecting..."
rc = ny.connect()

# Check for a successfull connection
if rc != NC.ERR_SUCCESS:
    print "Can't connect"
    sys.exit(-1)
print "Connected!"

# Main loop
while 1:
	card_id = listen(card, 0.1)
	listen_remove(card, 0.1, card_id)

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
