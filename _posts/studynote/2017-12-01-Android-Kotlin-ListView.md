---
layout: post
title:  "[Android][Kotlin] ListView 리스트뷰"
author: Yena Choi
categories: studynote
tags: [android, kotlin, view]
---

## ListView
안드로이드에서 리스트뷰를 사용하기 위해 준비해야 할 것을 정리하자면
1. 사용할 데이터 정리하기. (클래스 파일 생성하는 것이 편리함. 여기서는 Item 클래스)
2. ListView를 사용할 레이아웃의 xml 파일에 리스트뷰 만들기
3. `Adapter` 설정하기
4. (옵션) Custom ListView 레이아웃 파일 만들고 적용하기.


### Default ListView Item을 통해 리스트뷰 만들기

```Kotlin
class MainActivity : AppCompatActivity() {
    lateinit var adapter : ArrayAdapter<Item>
    /* 커스텀 클래스인 Item을 Array에 나열하는 ArrayAdapter 선언 */

    override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main).

      adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, item)
      /* adapter 첫번째 파라미터로 this를 써서 context를 가져오고,
         안드로이드 기본 View인 simple_list_item_1 View를 가져오고,
         커스텀한 클래스 Item의 변수 item을 가져왔다 */

      myListView.adapter = adapter
    }
}
```
