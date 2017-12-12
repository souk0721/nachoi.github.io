---
layout: post
title:  "[Android][Kotlin] 안드로이드 코틀린 Volley"
author: Yena Choi
categories: studynote
tags: [android, kotlin]
---

## Volley
안드로이드의 `Volley`는 안드로이드 앱에서 쉽고 빠르게 네트워킹을 할 수 있게 해주는 HTTP 라이브러리이다. ~~(한글로 적었는데, 다시 보니 모두 영어다)~~

기본적으로 안드로이드에서 Web Request를 요청할 때 HTTP 라이브러리를 쓸 수도 있지만, 사용하기 어렵기도 하고 AsyncTask를 써야하는 등 번거로움이 많다. Volley의 장점을 몇 가지만 살펴보자면 다음과 같다. ([Android Developers Volley 페이지](https://developer.android.com/training/volley/index.html)와 [gist.github.com/benelog/5981448](https://gist.github.com/benelog/5981448) 페이지를 참고)

- 네트워크 요청(Request) 우선 순위를 자동으로 관리한다.
  - 요청별 우선 순위를 지원한다. '목록 조회'와 '이미지 다운로드' 요청이 있을 경우, 우선 순위가 높은 '목록 조회' 요청은 이미지 로딩이 끝나지 않더라도 수행된다.
- 동시에 여러 네트워크 요청을 할 수 있다.
- 요청을 할 때 Cache 적용 여부를 의식하지 않아도 된다.
<br><br>

### Volley 사용 준비
안드로이드 스튜디오의 그래들 `build.gradle (Module:app)` dependencies 하단에 volley 관련 코드를 추가한다.

```
dependencies {
    ...
    compile 'com.android.volley:volley:1.1.0'
}
```
dependency 추가 후에는 우측 상단의 **Sync Now** 를 클릭하여 업데이트를 해준다.
<br>

또한, 매니페스트 파일에서 인터넷 퍼미션을 허가해주어야 한다.
```
<uses-permission android:name="android.permission.INTERNET" />
```
#### Request 및 Response 테스트할 서버
본 포스트에서는 [heroku](https://www.heroku.com) 서버를 사용했다. heroku에서는 몇 가지 제약 사항이 있긴 하지만 간편하게, 무료로 서버를 제공해주고 있다. 결제 필요 없이, 신용카드 정보를 입력하면 무료로 데이터베이스까지 연결해준다. 일단은 Volley 기능을 숙지하는 데에 초점을 두고, 데이터베이스 연동 없이 default 텍스트만 있는 서버에 접속했다. ([nachoitest](https://nachoitest.herokuapp.com/))
<br><br>

### Kotlin에서 Volley로 Request 요청하기
Volley를 사용하려면 `RequestQueue`를 만들고 `Request` 객체를 전달해야 한다.
- RequestQueue는 작업이 이루어지는 스레드를 관리한다. 이 작업자 스레드에서는 네트워크 작업 수행, 캐시 읽기/쓰기, 그리고 응답(Response)을 분석(Parsing)이 이루어진다.
- Request는 response의 원본 데이터를 파싱하고, Volley는 파싱 된 response를 다시 메인 스레드로 전달하는 역할을 한다.
<br>

프로젝트의 어느 곳에서든 이러한 작업을 요청할 수 있도록 해야 편리할 것이다. 따라서, Singleton 파일을 생성한 후에 Request 및 Volley에 관련된 기능을 함수로 추가하기로 한다.
대략적인 생김새는 다음과 같다.

```Kotlin
/* VolleyService.kt 파일을 Object로 생성 */

object VolleyService {

    fun testVolley(context: Context, success: (Boolean) -> Unit) {

        val myJson = ...
        val testRequest = ...

        Volley.newRequestQueue(context).add(testRequest)

        /* json을 만들고, 그 json을 서버에 보낼 testRequest 인스턴스 생성.
         * Volley로 새로운 RequestQueue를 만들고, 해야할 요청에 testRequest를 추가. */
    }
}
```
<br>

Volley를 사용할 준비가 되었으면, 요청 작업을 실행할 액티비티로 이동한다. 이 예제에서는 MainActivity에서 Btn 버튼을 눌렀을 때 Volley가 setOnClickListener 에서 실행되도록 작업했다.

```Kotlin
/* MainActivity.kt */

Btn.setOnClickListener {
    VolleyService.testVolley(this) { testSuccess ->
        if (testSuccess) {
            Toast.makeText(this, "통신 성공!", Toast.LENGTH_LONG).show()
        } else {
            Toast.makeText(this, "테스트 실패...!", Toast.LENGTH_LONG).show()
        }
    }
}
```
<br>

<center>
<img src="/assets/post-img/171213-success.JPG" width="40%" height="40%">
</center>
<br>

최종적으로 이런 결과를 얻으면 성공이다.

<br>


대략적인 코드 생김새를 살펴보았으니 Volley를 설정하는 함수를 구체화해보자.

이 예제에서는 빈 json을 서버에 보내 요청(Request)하고, String 형식의 응답(Response)을 수신한다. 이러한 형식의 통신을 하려면 `StringRequest` 인스턴스를 생성한다.
```
StringRequest(Method, Url, Response.Listener{}, Response.ErrorListener{})
```
파라미터는 위와 같이 4가지이다. Response 성공, 에러 시의 Listener lambda를 각각 작성해주어야 한다..

<br>
그럼 드디어, 실제 서버에 요청을 해보자.

```Kotlin
/* VolleyService.kt */

object VolleyService {

    val testUrl = "https://nachoitest.herokuapp.com/"

    fun testVolley(context: Context, success: (Boolean) -> Unit) {

        val myJson = JSONObject()
        val requestBody = myJson.toString()
        /* myJson에 아무 데이터도 put 하지 않았기 때문에 requestBody는 "{}" 이다 */

        val testRequest = object : StringRequest(Method.GET, testUrl , Response.Listener { response ->
            println("서버 Response 수신: $response")
            success(true)
        }, Response.ErrorListener { error ->
            Log.d("ERROR", "서버 Response 가져오기 실패: $error")
            success(false)
        }) {
            override fun getBodyContentType(): String {
                return "application/json; charset=utf-8"
            }

            override fun getBody(): ByteArray {
                return requestBody.toByteArray()
            }
            /* getBodyContextType에서는 요청에 포함할 데이터 형식을 지정한다.
             * getBody에서는 요청에 JSON이나 String이 아닌 ByteArray가 필요하므로, 타입을 변경한다. */
        }

        Volley.newRequestQueue(context).add(testRequest)
    }
}
```

```Kotlin
/* MainActivity.kt */

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Btn.setOnClickListener {
            VolleyService.testVolley(this) { testSuccess ->
                if (testSuccess) {
                    Toast.makeText(this, "통신 성공!", Toast.LENGTH_LONG).show()
                } else {
                    Toast.makeText(this, "통신 실패...!", Toast.LENGTH_LONG).show()
                }
            }
        }
    }
}
```
<br>
성공하면 String 형식으로 Response를 받아온다.

<center>
<img src="/assets/post-img/171213-full.JPG" width="100%" height="100%">
</center>
<br>

### References
- https://developer.android.com/training/volley/index.html
- https://developer.android.com/training/volley/simple.html
- https://gist.github.com/benelog/5981448
- http://ljs93kr.tistory.com/8
