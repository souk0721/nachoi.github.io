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
    lateinit var adapter : ArrayAdapter<Category>
    /* 커스텀 클래스인 Category를 Array에 나열하는 ArrayAdapter 선언 */

    override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main).

      adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, categories)
      /* adapter 첫번째 파라미터로 this를 써서 context를 가져오고,
         안드로이드 기본 View인 simple_list_item_1 View를 가져오고,
         커스텀한 클래스 Category의 변수 categories를 가져왔다 */

      myListView.adapter = adapter
    }
}
```
<br><br>

### Custom ListView를 통해 리스트뷰 만들기
문자, 그림 등의 View를 원하는 대로 배치하여, 원하는 형태의 View로 ListView를 채울 수 있다.
새로운 layout.xml을 생성하여 원하는 형태의 Item을 만든다.

(height 폭을 고정폭으로 설정하여 Item이 ListView 안에 들어갈 수 있도록 한다.)

그 후 새로운 Adapter를 커스텀하기 위한 Class를 생성한다. 이 어댑터 클래스는 `BaseAdapter`를 extend한다

```Kotlin
class MyAdapter(Context context, categories: List<Category>): BaseAdapter() {
    val context = context
    val categories = categories

    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
      val categoryView: View

      categoryView = LaoutInflater.from(context).inflate(R.layout.my_list_item.xml, null)
      val categoryTitle : TextView = categoryView.findViewById(R.id.categoryTitle)
      val categoryImage : ImageView = categoryView.findViewById(R.id.categoryImage)
      /* 새로 생성한 categoryView의 각 뷰의 내용을 실제 데이터에 있는 Category클래스의 데이터와 각각 연결 */

      val category = categories[position]

      val resourceId = context.resources.getIdentifier(category.image, "drawable", context.packageName)
      categoryImage.setImageRecource(resourceId)
      categoryTitle.text = catogory.categoryTitle

      return categoryView
    }

}
```

어댑터 설정이 끝나면 ListView를 사용할 액티비티로 돌아와서 어댑터를 설정해준다.
```Kotlin
class MainActivity : AppCompatActivity() {
    lateinit var adapter : MyAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        adapter = MyAdapter(this, categories)
        myListView.adapter = adapter

    }
}
```
