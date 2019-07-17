# Kotlin code style guide

## [Class component rules]

### ✓ 클래스 내부의 요소들은 아래와 같은 정렬을 따른다.

- package + import statements
- two empty lines
- typealias definitions(if exist)
- class/interface definition
- extra contructors(if exist)
- companion object definition
- interface(callback) definition
- nested/inner class definition(if exist)
- concrete members(if exist)
- abstract methods(if exist)
- init definition(if required)
- lifecycle related methods with basic order(if required)
- concreate method
  - override
  - public
  - private
- interface/class implementation members(if exist)

``` kotlin
😍
package code.style.guide

import i.will.get.audi.r8.eventually


typealias MyCustomInt = Int

internal abstract class MyCustomLayout(
    private val context: Context,
    private val attrs: AttributeSet? = null

) : FrameLayout(context, attrs) {

  constructor(context: Context, attrs: AttributeSet? = null, extra: Int) : this(context, attrs, extra)
  
  constructor(
      context: Context, 
      attrs: AttributeSet? = null, 
      extra: Int, 
      extra1: Int
  ) : this(context, attrs, extra)
  
  companion object {
    private const val MAX_LAYOUT_WIDTH  = 100F
    private const val MAX_LAYOUT_HEIGHT = 100F
  }

  interface Listener {
    fun onButtonClicked()
  }
  
  class ViewState(val profileImageBitmap: Bitmap, val state: Entry)
  
  sealed class Entry {
    class Open : Entry()
    
    class Lock : Entry() 
  }
  
  init {
    initializeViews()
  }
  
  private lateinit var viewState: ViewState
  
  private var listener: Listener? = null
  
  override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    ...
    
    processMeasure()
  }

  override fun onLayout(changed: Boolean, left: Int, top: Int, right: Int, bottom: Int) {
    ...
  }

  override fun onDraw(canvas: Canvas?) {
    ...
  }

  fun setViewState(viewState: ViewState) {
    this.viewState = viewState
    
    ...
  }
  
  fun setListener(listener: Listener) {
    this.listener = listener
  }
  
  fun changeProfileImage(bitmap: Bitmap) {
    ...
  }
  
  private fun initilizeViews() {
    ...
  }
  
  private fun processMeasure() {
    processMeasureRunnable.run()
  }
  
  private val processMeasureRunnable = Runnable { ... }

}
```

## [Basic rules]

### ✓ enum 은 대문자로 시작해 camel case 를 사용한다.

``` kotlin
😰
enum BadEnum {
  BAD_ENUM,
  alsoBadEnum
}

😍
enum GoodEnum {
  GoodEnum,
  AlsoGoodEnum
}
```

### ✓ : 은 아래의 룰에 따른다.

- 리턴 값을 의미할 경우 suffix space 를 넣는다.
- 상속/구현을 의미할 경우 prefix/suffix spaces 를 모두 넣는다.

``` kotlin
😰
fun bad() : String {
  return "a"
}

class Bad constructor(context: Context): FrameLayout(context)

😍
fun good(): String {
  return "a"
}

class Good constructor(context: Context) : FrameLayout(context)
```

### ✓ 생성자는 아래와 같이 작성한다.

``` kotlin
😰
internal class BadClass contructor(
    private val context: Context,
    private val resources: Resource
) : FrameLayout(context) { /** 윗 라인에 공백라인을 추가한다. */

  private val variable: Int = 0

  ...
  
}

😍
internal class GoodClass contructor(
    private val context: Context,
    private val resources: Resource
    
) : FrameLayout(context) {

  private val variable: Int = 0
  
  ...
  
}
```

### ✓ 빈 함수의 경우 Unit 을 리턴한다.

``` kotlin
😰
fun badEmptyFunction() {
}

😍
fun goodEmptyFunction() = Unit
```

### ✓ data class 를 사용하지 않는다.

- kotlin 은 class 앞에 data keyword 를 붙여 data class 로써 편의함수(equal, hash, copy)등을 자동포함하도록 하는 기능이 있다.
- 하지만 이를 사용하지 않는다.
- 그 이유에 대해서는 이곳에 서술하기엔 길어지므로 lead programmer 에게 문의

### ✓ 변수명만으로 타입의 의미를 표현하기 힘든 경우 typealias 를 적극적으로 사용한다.

``` kotlin
😰
val badVariable = hashMapOf<Int, String>()

😍
typealias PictureIndex = Int
typealias PictureUrl   = String

val goodVariable = hashMapOf<PictureIndex, PictureUrl>()
```


