---
layout: post
title:  "[Android][Kotlin] 생명주기의 Instance State"
author: Yena Choi
categories: studynote
tags: [android, kotlin, lifecycle]
---

App에 데이터를 입력하다가 전화를 받는 등, 다른 작업을 하고 나서 입력한 데이터가 사라질 위험이 있다. 이를 방지하기 위해 액티비티가 재생성될 때에 Instance State에 임시로 데이터를 저장할 수 있다.

## onSaveInstanceState
액티비티가 onStop 상태가 되면 강제 종료될 가능성이 있다. 혹은 앱의 가로/세로 모드를 전환하면 onDestroy 된 후 다시 onCreate 된다. 액티비티가 다시 **생성(Create)되기 전에**  임시 데이터가 저장되어 있다면 불러올 수 있다. 이것이 `onSaveInstanceState` 의 역할이다. onSaveInstanceState 메소드를 override 해서 outState에 데이터를 `put` 할 수 있다.

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onSaveInstanceState(outState: Bundle?) {
        super.onSaveInstanceState(outState)
        outState?.putString("stringKey", "This is a String value.")
        outState?.putBoolean("booleanKey", true)
        /* 맨 처음에는 저장되어있는 Bundle 데이터가 없으므로, outState가 null일 수 있다.
           따라서 outState 뒤에 ? 를 붙여 safeCall 한다. */
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```
<br><br>

## onRestoreInstanceState
onCreate 가 되기 전에 데이터를 저장했으니, onCreate 되고 나서는 데이터를 불러와야 한다. 이 때는 `onRestoreInstanceState` 메소드를 override 한다.

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onSaveInstanceState(outState: Bundle?) {
        super.onSaveInstanceState(outState)
        outState?.putString("stringKey", "This is a String value.")
        outState?.putBoolean("booleanKey", true)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    override fun onRestoreInstanceState(savedInstanceState: Bundle?) {
        super.onRestoreInstanceState(savedInstanceState)
        if (savedInstanceState != null) {
            println(savedInstanceState.getString("stringKey"))
            println(savedInstanceState.getBoolean("booleanKey"))
            /* savedInstanceState 에 데이터가 있는지 if로 확인한다 */
        }
    }
}
```
