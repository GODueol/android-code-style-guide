# General

## [ Basic rules ]

### ✓ 무엇을 선언할 때는 기본적으로 가시성이 가장 좁게 선언한다.

#### 🌟뭐든지 우선 안되게 하고 나중에 필요할 때 되게하라. 🌟
- 변수/함수를 선언할 경우 습관적으로 private 를 붙여 선언하고 이후 외부에서 필요 시 public 으로 전환한다.
- 클래스의 경우 기본적으로 internal 을 붙여 선언한다.
- 위처럼 할 경우 자신이 짠 코드의 변수에 private 가 없으면 다른 개발자가 자신의 코드를 볼 때 해당 변수는 다른 곳에서 참조되고 있다는 것을 자연스럽게 알 수 있게 된다.

  - 코틀린에서 기본 클래스가 상속이 안되고 상속이 되게하려면 open 을 붙이도록하는 것은 위 🌟 표시의 철학과 비슷하다.

``` kotlin
😰 
val badVarible = 0

class BadClass

😍
private val goodVariable = 0

internal class GoodClass
```

### ✓ CamelCase base

- 기본적으로 Camel case 를 사용한다.
- 하지만 아래와 같은 예외는 있다.
  - 테스트 코드의 실제 테스트 함수명은 snake case 로 사용한다.
  - ~테스트 코드는 한글로 작성할까 고려 중..~
    ``` kotlin
    fun this_function_return_true() { ... }
    ```
  - 테스트 코드 중 setUp 과 tearDown 함수는 Camel case 를 사용한다.
    ``` kotlin
    @Before
    fun setUp() { ... }
    
    @After
    fun tearDown() { ... }
    ```

### ✓ static / const 필드는 전부 대문자로 작성하며 snake case 를 사용한다.

``` kotlin
😰
companion object {
  private const val badConstVariable = 1
}

😍
companion object {
  private const val GOOD_CONST_VARIABLE = 1
}

```

### ✓ 모든 컴포넌트 사이의 공백은 한칸을 사용한다. 두칸 이상 사용하지 않는다.

``` kotlin
😰
private val someVariable = 0
                                        /** 이곳에 공백라인이 두칸이면 */
                                        /** 안된다. */
private fun someFunction1() {
  ...
}
                                        /** 이곳에 공백라인이 두칸이면 */
                                        /** 안된다. */
private fun someFunction2() {
  ...
}

😍
private val someVariable = 0
                                        /** 한칸의 공백라인만 !*/
private fun someFunction1() {
  ...
}
                                        /** 한칸의 공백라인만 !*/
private fun someFunction2() {
  ...
}
```

### ✓ 불필요한 코멘트는 피한다.
 
- 아래와 같은 경우에만 코멘트를 작성하며 코멘트는 ``` // ``` 가 아닌 ``` /** ... */ ``` 를 사용한다.
  - 코드가 하는 작업이 변수명이나 메소드명만으로는 그 의미를 전달하기 매우 힘들 경우.
  - 불가피하게 매직상수를 사용하게 되어 그 상수의 의미를 전달할 필요가 있을 때.
  - 코드만으로는 설명할 수 없는 History 가 있을 경우.  (ex: 이 코드는 ~이슈 때문에 이렇게 짰고 ~해서 작동한다)
  - 일반적이지않은 데이터 처리의 경우. (ex: 이 데이터는 ~하게 오기 때문에 ~해서 처리해야한다)
    - 예) onMeaure의 parameter들은 여러 정보가 bitwise 돼 담겨오기 때문에 View.MesureSpec 로 unpack 해서 사용해야한다.

``` kotlin
😰 // Bad Comment 

😍 /** Good Comment */
```

### ✓ Numeric literal 의 suffix 는 대문자를 사용한다.

``` kotlin
😰 private val badFloat = 1f

😍 private val goodFloat = 1F
```

### ✓ 10만 이상의 Numeric literal 을 표현할 때는 underscore 를 사용한다.

``` kotlin
😰 private val badNumber = 111111

😍 private val goodNumber = 1_111_111
```

### ✓ Brace 를 최대한 사용한다.

``` kotlin
😰 
if(condition) 
  Log.d(TAG, "bad!") 
else 
  Log.d(TAG, "also bad!")

😍 
if(condition) {
  Log.d(TAG, "GOOD!")  
} else {
  Log.d(TAG, "also GOOD!")
}
```

### ✓ 코드의 흐름은 가로보다는 세로의 흐름으로 작성한다.

- if, switch, when, for 문등은 가로로 한 줄로 쓰기보다는 세로로 작성한다.
- 예외) 단, rx operator, lamda/closure 에서는 *내부 로직이 한 줄로 작성이 가능할 경우* 한 줄로 작성한다.

``` kotlin
😰 
if(condition) { Log.d(TAG, "bad!") }

private val badOpeartorStyle = Observable.just(1, 2, 3)
  .filter {
    it == 1
  }
  .map {
    it + SOME_VALUE
  }
  
bad_lamda.setOnClickListener {
  doSomething()
}

😍 
if(condition) {
  Log.d(TAG, "GOOD!") }
else {
  Log.d(TAG, "also GOOD!")
}

private val goodOperatorStyle = Observable.just(1, 2, 3)
  .filter { it == 1 }
  .map { it + SOME_VALUE }
  
good_lamda.setOnClickListener { doSomething() }
```

### ✓ 메소드 어노테이션은 세로로 작성한다.

``` kotlin
😰 
@Provides @PerActivity
fun badAnnotating(): String { ... }

😍
@Provides 
@PerActivity
fun goodAnnotating(): String { ... }
```

### ✓ 필드 어노테이션은 너무 길어지지 않는 이상 기본적으로 가로로 작성한다.

``` kotlin
😰 
@Inject 
@field:Bad
lateinit var badAnnotating: String

😍
@Inject @field:Good lateinit var goodAnnotating: String
```

### ✓ Builder 패턴의 코드를 부를 때에는 무조건 LF한다.

``` kotlin
😰 
badBuilder.setInt(1).build()
  
badBuilder.setInt(1).setBoolean(false).build()

😍
goodBuilder
  .setInt(1)
  .build()
  
goodBuilder
  .setInt(1)
  .setBoolean(false)
  .build()
```

### ✓ 축약어를 쓰지 않는다.

``` kotlin
😰 
private val usr = User() /** bad variable */

private val std = Student() /** bad variable */

private val probIndex = 0 /** bad variable */

😍
private val user = User() /** good variable */

private val student = Student() /** good variable */

private val problemIndex = 0 /** good variable */
```

### ✓ 변수명은 그 변수가 어떤 의미를 갖는지 알 수 있도록 충분히 길게 짓는다. 

- 변수의 의미전달 때문에 너무 길어질 경우는 적당한 선에서 작성한다. 3~30 자 내외
- flag 라는 이름의 변수명은 절대 사용하지 않는다.

``` kotlin
😰 
private val index = 0 /** bad naming */

private val isCorrect = false /** bad naming */

private val isLoginDataAndNewUpdateCheckDataAndNextDataFetched = false /** bad naming */

private val flag = false /** bad naming */

😍
private val mainProblemIndex = 0 /** good naming */

private val isMainProblemCorrect = false /** good naming */

private val isAllDatasFetched = false /** good naming */
```

### ✓ 매직{넘버|스트링|...}(은)는 사용하지 않는다. 상수로 의미를 정의해서 쓴다.

``` kotlin
😰 
if(currentProblemNumber == 10) { /** bad magic number */
  completeLesson()
  navigateToResult()
}

if(problemType == "N") { /** bad magic string */
  hideHintTip()
}

😍
companion object {
  private const val MAX_PROBLEM_NUMBER = 10
  
  private const val NORMAL_PROBLEM_TYPE_PROVIDED_BY_SERVER = "N"
}

if(currentProblemNumber == MAX_PROBLEM_NUMBER) { /** good */
  completeLesson()
  navigateToResult()
}

if(problemType == NORMAL_PROBLEM_TYPE_PROVIDED_BY_SERVER) { /** good */
  hideHintTip()
}
```

### ✓ 0부터 시작하는 변수는 index 라는 변수명을 사용하고 1부터 시작하는 변수는 order/number/sequence 라는 변수명을 사용한다.

### ✓ if 조건에 여러개(3개 이상)의 조건이 필요할 경우 아래와 같이 작성한다.

``` kotlin
😰 
if(badIfCondition && badIfCondition1 && badIfCondigion2) {
  ...
}

😍
if(goodIfCondition 
    && goodIfCondition1 
    && goodIfCondition2) {
  ...
}
```

### ✓ {outer|inner|nested|sealed}클래스/인터페이스의 시작과 끝 공백라인 첨가 여부는 아래 룰을 따른다.

- 최상위 outer 클래스만 클래스의 시작과 끝에 공백을 넣고 나머지는 공백을 넣지 않는다.

``` kotlin
😰 
internal class BadOuterClass {
  private val context: Context? = null /** 윗 라인에 공백라인이 있어야함 */
  
  class BadNestedClass {
  
    private val myVarialble: Int = 0 /** 위 아래 공백라인이 없어야 함 */
  
  } 
} /** 윗 라인에 공백라인이 있어야함 */

internal interface BadInterface {

  fun doSomething() /** 위 아래 공백라인이 없어야 함 */

}

😍
internal class GoodOuterClass { /** outer 클래스이므로 시작과 끝에 공백라인이 있음 */

  private val context: Context? = null
  
  class GoodNestedClass { /** nested 클래스이므로 시작과 끝에 공백라인이 없음 */
    private val myVarialble: Int = 0 
  }
  
}

internal interface GoodInterface { /** interface 이므로 시작과 끝에 공백라인이 없음 */
  fun doSomething()
  fun doSomething()
}

internal sealed class GoodSealedClass { /** sealed class 이므로 시작과 끝에 공백라인이 없음 */
  class Class1( /** nested class 이므로 시작과 끝에 공백라인이 없음 */
    val variable: Int,
    val variable1: Int,
    val variable2: Int,
  ) : GoodSealedClass()
  
  class Class2(val variable: Int) : GoodSealedClass()
}
```

### ✓ 함수/람다|클로져의 시작과 끝에는 공백라인을 넣지 않는다.

``` kotlin
😰
private fun badFunction() {
                                        /** 불필요한 공백라인! */
  doSomething()
                                        /** 불필요한 공백라인! */
}

private fun alsoBadFunction() {
                                        /** 불필요한 공백라인! */
  doSomething()
}

private fun alsoAlsoBadFunction() {
  doSomething()
                                        /** 불필요한 공백라인! */
}

bad_lamda.setOnClickListener {
                                        /** 불필요한 공백라인! */
  doSomething()
  doNextThing()
                                        /** 불필요한 공백라인! */
}

😍
private fun goodFunction() {
  doSomething()
  doNextThing()
}

good_lamda.setOnClickListener {
  doSomething()
  doNextThing()
}
```

### ✓ 암시적 매개변수는 한 눈에 띄지 않는경우에는 명시를 해준다.

``` kotlin
😰
private val goodOperatorStyle = Observable.just(1, 2, 3)
  .filter { it == 1 }
  .map { it + SOME_VALUE }

😍
private val goodOperatorStyle = Observable.just(1, 2, 3)
  .filter { streamValue -> 
			streamValue == 1 
	}
  .map { filteredStreamValue -> 
			filteredStreamValue + SOME_VALUE 
	}
```