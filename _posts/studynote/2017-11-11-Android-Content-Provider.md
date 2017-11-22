---
layout: post
title:  "Content Provider 컨텐트 프로바이더"
author: Yena Choi
categories: studynote
---
## Content Provider
- 앱과 앱 저장소 사이에서 데이터 접근을 쉽게 하도록 관리해주는 클래스.
  - 왜 쓸까?
    1. 앱의 직접적인 코드 변경 없이 데이터 접근/변경할 수 있도록 해줌.
    2. Loader나 CursorAdapter 같은 클래스들도 사용하기 때문.
    3. \*\*다른 사용자들이 앱에 접근, 사용, 수정할 권한을 줌. (안전하게)
- manifest에서 권한을 부여해주어야 한다.

## Content Resolver
- 폰 안에 여러 앱, 여러 프로바이더가 있기 때문에 이들을 관리하고 흐름을 통제.
  앱이 접근하고자 하는 프로바이더 사이에서 중개자 역할.
- **query(읽기), insert, update, delete** 작업이 가능.
```java
ContentResolver resolver = getContentResolver();
Cursor cursor = resolver, query(DroidTermsExampleContract.CONTENT_URI,
                null, null, null, null);
// 위에서 Uri는 대략 이렇게 생김 - content://com.example.udacity.droidtermsexample/terms
```

### Content Provider 사용
- 우선 manifest에 등록해야한다. application 안에 <provider> 및 name, authority 등록
- Uri에 자주 쓰이는 Schema, authority, path 등은 String 및 Uri 변수로 설정.
  - 여러곳에서 쓰기 위해 `private static String/Uri` 로 선언하는게 편함.
- 예를 들어, 앱 내에 사용되는 Uri가 `content://authority` 와 `content://authority/path` 두 가지라면 각각의 경우에 다른 처리가 필요하다. 이를 만약 if문으로 구분하게 되면 경우의 수가 많아지므로 'UriMatcher'를 사용한다.

#### UriMatcher
- Content Provider가 받는 Uri의 종류를 결정해준다.
- 사용자가 Uri1 / Uri2를 구분해놓고, 상수 와일드카드(#) 사용할 수 있다.
- UriMatcher 함수 안에서 NO MATCH 및 추가 MATCH 등록하여 사용하면 편리.

  ```java
  public static final int CODE_ALL = 100;
  public static final int CODE_ID = 101;
  private static final UriMatcher sUriMatcher = buildUriMatcher();
  //아래 함수 작성 후, 프로바이더에서 편하게 사용하기 위해 static 선언

  public static UriMatcher buildUriMatcher() {
    UriMatcher uriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
    uriMatcher.addUri(Contract.AUTHORITY, Contract.PATH, CODE_ALL);
    uriMatcher.addUri(Contract.AUTHORITY, Contract.PATH + "/#", CODE_ID);
    // 위 코드는 특정 정수 로우값을 가질 경우 사용
    return uriMatcher;
  }
  ```


- 참고) UI에서 데이터를 query(읽기) 할 경우
Resolver를 가져와 query 메소드 호출(Uri 같이 전달) -> Uri의 Authority를 확인하여
매치되는 Provider를 찾아 쿼리를 전달 -> 쿼리함수에서 UriMatcher를 사용해 가져올 데이터 종류 확인
  - -> Uri와 기타 파라미터를 해독해 알맞은 SQL코드 작성 -> 해당되는 Data 가져옴 -> Cursor 반환

## Content Provider 메소드
6개의 메소드를 Override한다.
- OnCreate (void)
- insert (return Uri)
- query (return Cursor)
- update (return int)
- delete (return int)
- getType (return String)

### OnCreate
Content Provider 자바파일에서 context 가져온 후 DbHelper선언.

```java
public boolean onCreate() {
  Context context = getContext();
  mDbHelper = new DbHelper(context);
  return true;
}
```

### insert
- 1) db에 접근할수 있도록 가져온다. Writable (계속 쓸거니 final로 선언)
- 2) int구분값 선언 후 switch문 UriMatcher로 Uri 종류 구분.
- 3) Uri를 Return받을 변수를 선언한 후, switch문을 통해서
    UriMatcher종류에 맞는 id를 찾는다.
    db.insert메소드 호출하여 테이블에서 아이디 생성. 성공했다면 아이디는 0보다 크다.
    returnUri에 ContentUris.withAppendedId 함수를 통해 값 넣기
- 4) 예외사항들 만들어준후 Uri 반환
- 5) 하기 전에 ContentResolver에서 notifyChange하게 해줌.

```java
public Uri insert(@NonNull Uri uri, ContentValues values) {
    final SQLiteDatabase db = mTaskDbHelper.getWritableDatabase();
    int match = sUriMatcher.match(uri);

    Uri returnUri;

    switch (match) {
        case TASKS :
            long id = db.insert(TABLE_NAME, null, values);
            if (id > 0) { // id 생성에 성공한 케이스
                returnUri = ContentUris.withAppendedId(
                TaskContract.TaskEntry.CONTENT_URI, id);
            } else {
                throw new SQLException(
                "Failed 데이터 추가에 실패했습니다." + uri);
            }
            break;

        default :
            throw new UnsupportedOperationException(
            "Unknown Uri !!" + uri);
    }
    getContext().getContentResolver().notifyChange(uri, null);

    return returnUri;
}
```

이렇게 만들어뒀으면 나중에 실제로 insert작업 할때 메소드 호출하여 db에 저장


### query
insert랑 비슷하다. (아래 예시는 CursorAdapter를 사용한거니 참고.)

- 특정 데이터가 아닌 전체 데이터를 가져올 경우.
  1. db에 접근할수 있도록 가져온다. Readable (계속 쓸거니 final로 선언)
  2. int구분값 선언 후 switch문 UriMatcher로 Uri 종류 구분.
  3. Cursor를 Return받을 변수를 선언한 후, UriMatcher종류에 맞는 id를 찾는다.
    db.query 메소드 호출하고 인자들을 입력한 후 break;
  4. 예외사항들 만들어준후 Cursor 반환
  5. 반환하기 전에 Cursor setNotificationUri 하면 변동사항 있을때 알아서 알아.

```java
@Override
public Cursor query(@NonNull Uri uri, String[] projection, String selection,
                    String[] selectionArgs, String sortOrder) {

    final SQLiteDatabase db = mTaskDbHelper.getReadableDatabase();
    int match = sUriMatcher.match(uri);
    Cursor returnCursor;

    switch (match) {
        case TASKS :
            returnCursor = db.query(TABLE_NAME, projection,
              selection, selectionArgs, null, null, sortOrder);
            break;
        default :
            throw new UnsupportedOperationException(
            "데이터를 읽어올 수 없습니다." + uri);

    }
    returnCursor.setNotificationUri(
                getContext().getContentResolver(), uri);
    return returnCursor;
}
```

- 지정한 데이터만 가져올 경우 (selection과 selectionArgs 파라미터 재설정)

```java
case TASK_ID :
  String id = uri.getPathSegments().get(1);
  // uri가 아래와 같은 경우에 0레벨(task) 옆의 1(#)을 id에 저장
  // content://<com.~todolist authority>/tasks/#

  String thisSelection = "\_id=?"; // 디폴트 id 묻는값임
  String[] thisSelectionArgs = new String[]{id};
  // 위에서 구한 path(id) 값을 모두 잘라와서 배열
  returnCursor = db.query(TABLE_NAME, projection,
      thisSelection, thisSelectionArgs, null, null, sortOrder);

  break;
```


**\* query는 읽어오기 작업이고 오래걸리기 때문에 background thread나
    AsyncTask에서 쓰는것이 좋다.**


### delete
```java
@Override
public int delete(@NonNull Uri uri, String selection,
                  String[] selectionArgs) {
    final SQLiteDatabase db = mTaskDbHelper.getWritableDatabase();
    int match = sUriMatcher.match(uri);
    int deleted; // 삭제된 개수. 0에서 시작.

    switch (match) {
        case TASK_WITH_ID:
            String id = uri.getPathSegments().get(1);
            deleted = db.delete(TABLE_NAME,
                            "_id=?", new String[]{id});
            break;
        default:
            throw new UnsupportedOperationException(
            "Unknown uri: " + uri);
    }

    if (deleted != 0) {
        getContext().getContentResolver().notifyChange(uri, null);
    }
    return deleted;
}
```

### Reference
[안드로이드 개발자 홈페이지](https://developer.android.com/training/basics/data-storage/databases.html?hl=ko#DeleteDbRow)
