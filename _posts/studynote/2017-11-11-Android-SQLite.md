---
layout: post
title:  "[Android] SQLite 데이터 저장하기"
author: Yena Choi
categories: studynote
---

## SQLite
- 서버가 아닌 응용프로그램에 넣어 사용하는 데이터베이스.
- 하나의 파일을 사용하며, 중소 규모의 데이터를 관리할 떄 사용한다.
- 연락처와 같이, 구조적이고 반복적인 데이터 관리에 적합하다.
- 안드로이드에서는 `android.database.sqlite` 패키지에서 API 사용.

## SQLite 데이터의 영구적 저장 (file로 만들어서 저장, 표 형식)
  1) SQLiteOpenHelper를 extend하는 dbHelper class 생성
  2) class 내에 dbHelper 생성
   (context, NAME, Cursor, VERSION) 값 받음

   ```java
   public DBHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version) {
     super(context, name, factory, version);
   }
  3) DB를 값으로 가지는 onCreate를 생성, DB.exec(String)
     String에서 테이블이름, \_ID, item들 입력.
  4) 버전 업글용 upgrade함수
  5) MainActivity에서는 새 DBHelper를 선언 (getApp~context(), ".db이름", cursor(|| null), ver)
  ```

- onCreate하기 전에 SQL 자체를 선언하고, add나 remove 메소드를 만들어서, mDb.add나 remove
