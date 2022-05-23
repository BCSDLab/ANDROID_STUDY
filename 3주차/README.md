# Android Jetpack

Support Library

> Android Support 라이브러리는 안드로이드에서 하위 호환성과 몇몇 편의성 및 기능을 제공해주는 라이브러리다. Support 라이브러리에서 가장 중요하게 지원하는 하위 호환성이란 상위 버전의 안드로이드에서 추가된 새로운 API를 하위 버전의 안드로이드에 지원하도록 해주는 것이다.
>
> Support 라이브러리에는 몇가지 문제가 있었다.
>
> 1. Support 라이브러리는 단일 라이브러리로 제공된다. Support 라이브러리의 기능 하나만을 사용하고 싶어 Support 라이브러리를 추가하면 사용하지 않는 다른 부분들까지 추가되어 앱의 규모가 커지게 되는 문제가 있다.
> 2. 패키지명과 버전 표기법. Support 라이브러리의 패키지명은 'support-v4' 처럼 v# 표기법으로 지원하는 최소 API 레벨을 나타냈다. 하지만 Support 라이브러리의 26.0.0버전부터 모든 Support 라이브러리의 지원하는 최소 API 레벨이 14로 변경되었기 때문에 이러한 패키지명의 버전 표기가 의미가 없어졌다.

Jetpack 라이브러리

> Jetpack은 Support 라이브러리의 문제점들을 해결하고, 개발자가 권장사항을 준수하고 boilerplate 코드를 줄이고 모든 Android 버전과 기기에서 일관되게 동작하는 코드를 작성하는 것에 도움을 주는 라이브러리 모음이다.

AndroidX

> `androidx`는 Jetpack 라이브러리가 게시된 네임스페이스이다. `androidx`패키지는 안드로이드 플랫폼과 별도로 제공되고, 기존의 Support 라이브러리를 대폭 개선하여 완전히 대체했다. 
>
> * AndroidX의 모든 패키지는 `androidx`로 시작하는 일관된 네임스페이스에 있다.
> * `androidx`패키지는 별도로 유지보수되고 업데이트 된다. AndroidX 라이브러리를 프로젝트에서 독립적으로 업데이트할 수 있다.

***

# How to write test code

Android에서의 테스트 유형

> Subject별 테스트 유형
>
> * Functional testing : 앱이 제대로 작동하는 지
> * Performance testing : 빠르고 효율적인지
> * Accessibility testing : 접근성 서비스와 잘 작동하는지
> * Compatibility testing : 모든 기기와 API 레벨에서 잘 작동하는지
>
> Scope별 테스트 유형
>
> ![Tests can be either small, medium, or big.](https://developer.android.com/training/testing/fundamentals/test-scopes.png)
>
> * Unit test : 메소드, 클래스같은 앱의 매우 작은 부분 테스트
> * End-to-end test : 앱의 전체 화면이나 사용자 흐름같은 앱의 큰 부분을 동시에 확인
> * Integration test : 둘 이상의 unit의 통합을 확인하는 테스트
>
> Instrumented test와 local test
>
> * Instrumented test : Android 기기나 에뮬레이터에서 실행되는 테스트이다. 주로 UI의 상호작용을 테스트한다.
>   ```kotlin
>   // When the Continue button is clicked
>   onView(withText("Continue"))
>       .perform(click())
>   
>   // Then the Welcome screen is displayed
>   onView(withText("Welcome"))
>       .check(matches(isDisplayed()))
>   ```
>
> * Local test : host-side test라고 불리는 개발기기나 서버에서 실행되는 테스트이다. 일반적으로 테스트 대상을 앱의 다른 부분과 분리하여 테스트한다.
>   ```kotlin
>   // Given an instance of MyViewModel
>   val viewModel = MyViewModel(myFakeDataRepository)
>   
>   // When data is loaded
>   viewModel.loadData()
>   
>   // Then it should be exposing data
>   assertTrue(viewModel.data != null)
>   ```
>
>
> Android Studio에는 실행환경에 따른 테스트 디렉토리가 2가지 있다.
>
> * `androidTest`디렉토리는 실제 장치나 가상 장치에서 실행되는 테스트가 포함된다. integration 테스트나 end-to-end 테스트, JVM만으로 검사할 수 없는 테스트가 포함된다.
> * `test`디렉토리는 unit 테스트 처럼 로컬 시스템에서 실행되는 테스트가 포함된다. JVM에서 실행되는 테스트가 포함될 수 있다.
>
> Unit Test
> 표준 모형을 따른다면 다음과 같은 경우에 Unit 테스트를 사용해야 한다.
>
> * ViewModel, presenter에 대한 Unit 테스트
> * data 레이어, 특히 repository에 대한 Unit 테스트
> * domain 레이어같은 플렛폼 독립적인 레이어에 대한 Unit 테스트
> * utility class에 대한 Unit 테스트
>
> UI Test
>
> * Screen UI test는 단일 화면에서 사용자의 상호작용을 검사하는 테스트다. 버튼 클릭, 보여지는 상태 등을 테스트한다. 한 화면당 하나의 클래스로 작성하는 것이 좋다.
> * User flow test와 Navigation test는 navigation 흐름을 통한 사용자의 움직임을 시뮬레이션한다.
>
> 테스트를 설계할 때 고려되어야 할 3가지 측면이 있다.
>
> * Scope : 얼마만큼의 범위의 코드를 테스트할 것인지에 관한 것이다. 테스트는 하나의 메소드를 테스트할 수도 있고 앱 전체를 테스트할 수도 있다.
> * Speed : 테스트가 얼마나 빠르게 실행되는 지에 관한 것이다. 테스트의 속도는 밀리초 단위에서 분 단위 까지 다양하다.
> * Fidelity : 테스트가 얼마나 실제에 가까운 지이다. 테스트할 때 실제로 그 상황일 때를 테스트할 수도 있고 거짓으로 그 상황을 만들어서 테스트를 할 수도 있다.
>
> Isolation and dependencies
>
> 테스트를 할 때 테스트하는 대상을 격리시켜야한다. 테스트 대상이 작동하려면 의존성을 제공해야하는 경우가 있다. 이런 경우 보통 test double을 만든다. test double은 앱의 구성요소 처럼 작동하지만 특정 동작이나 데이터를 제공하기위해 테스트에서 만드는 객체이다. test double을 사용하면 테스트를 더 빠르고 간단하게 작성할 수 있다.
>
> test double
>
> * Fake : 클래스의 작동에 대한 구현이 있어 테스트에 적합하지만 프로덕션으로는 적합하지 않은  test double이다. Fake는 mocking 프레임워크를 요구하지않고 가볍다.
> * Mock : 상호작용에 대한 기대를 갖고 프로그래밍한 내용에 따라 동작하는 test double이다. Mock는 상호작용의 결과와 정의한 요구사항과 다르면 테스트가 실패한다. Mock는 보통 mocking 프레임워크로 만들어진다.
> * Stub : 상호작용에 대한 기대는 없지만 프로그래밍한 내용에 따라 동작하는 test double이다. 일반적으로 mocking 프레임워크에 만들어진다. 단순함을 위해 Stub보다 Fake가 선호된다.
> * Dummy : 매개변수로 전달되는 경우 처럼 전달만 되고 사용되지는 않는 test double이다.
> * Spy : mock와 비슷한, 추가적인 정보의 추적을 유지하는 실제 객체에 대한 wrapper이다.
> * Shadow : Robolectric에서 사용되는 Fake이다.
>
>
> Local tests
>
> local test는 Android 기기나 에뮬레이터가 아닌 자체 워크스테이션에서 직접 실행된다. 즉, local JVM을 사용한다. local test는 앱의 로직을 더 빠르게 평가할 수 있도록 해준다. 하지만 안드로이드 프레임워크와 상호작용할 수 없다는 것은 테스트할 수 있는 타입을 제한한다.
> unit 테스트는 작은 코드 단위의 동작을 확인한다. 코드를 실행하고 결과를 확인하는 것이다. unit 테스트는 보통 단순한 편이지만, 테스트를 고려하지 않고 설계된 경우 설계가 힘들 수 있다.
>
> * 확인할 코드는 테스트에서 접근할 수 있어야 한다. 
> * unit 테스트를 독립적으로 실행하기 위해 테스트 대상의 의존성을 test double 처럼 제어할 수 있는 요소로 대체해야 한다.
>
> local unit 테스트는 JUnit 테스트 클래스로 작성한다. 하나 이상의 test 메소드를 가진 클래스를 생성한다. 테스트 메소드는 `@Test`어노테이션으로 시작하고 테스트 대상의 한 측면을 실행하고 확인하는 코드를 가지고 있다.
>
> ```kotlin
> import org.junit.Assert.assertFalse
> import org.junit.Assert.assertTrue
> import org.junit.Test
> 
> class EmailValidatorTest {
>   @Test
>   fun emailValidator_CorrectEmailSimple_ReturnsTrue() {
>     assertTrue(EmailValidator.isValidEmail("name@email.com"))
>   }
> }
> ```
>
> 앱의 구성요소가  예상되는 결과 값을 반환하는 지 평가하는 테스트를 작성해야 한다. junit.Assert같은 assertion 라이브러리를 사용하는걸 권장한다.
>
>
> Instrumented test
>
> instrumented 테스트는 안드로이드 기기에서 실행된다. 즉, 안드로이드 프레임워크의 API를 사용할 수 있다. 따라서 instrumented 테스트는 local 테스트보다 다소 느리더라도, 더 fidelity 하도록 해야한다. instrumented는 실제 기기에 대한 행동을 테스트해야할 떄만 사용하도록 권장된다. AndroidX Test에서 instrumented 테스트를 더 쉽게 작성할 수 있는 라이브러리를 제공한다.
>
> instrumented 테스트는 test runner를 `AndroidJUnit4`로 명시한 JUnit 테스트 클래스로 작성된다.
>
> ```kotlin
> import android.os.Parcel
> import android.text.TextUtils.writeToParcel
> import androidx.test.filters.SmallTest
> import androidx.test.runner.AndroidJUnit4
> import com.google.common.truth.Truth.assertThat
> import org.junit.Before
> import org.junit.Test
> import org.junit.runner.RunWith
> 
> const val TEST_STRING = "This is a string"
> const val TEST_LONG = 12345678L
> 
> // @RunWith is required only if you use a mix of JUnit3 and JUnit4.
> @RunWith(AndroidJUnit4::class)
> @SmallTest
> class LogHistoryAndroidUnitTest {
>     private lateinit var logHistory: LogHistory
> 
>     @Before
>     fun createLogHistory() {
>         logHistory = LogHistory()
>     }
> 
>     @Test
>     fun logHistory_ParcelableWriteRead() {
>         val parcel = Parcel.obtain()
>         logHistory.apply {
>             // Set up the Parcelable object to send and receive.
>             addEntry(TEST_STRING, TEST_LONG)
> 
>             // Write the data.
>             writeToParcel(parcel, describeContents())
>         }
> 
>         // After you're done with writing, you need to reset the parcel for reading.
>         parcel.setDataPosition(0)
> 
>         // Read the data.
>         val createdFromParcel: LogHistory = LogHistory.CREATOR.createFromParcel(parcel)
>         createdFromParcel.getData().also { createdFromParcelData: List<Pair<String, Long>> ->
> 
>             // Verify that the received data is correct.
>             assertThat(createdFromParcelData.size).isEqualTo(1)
>             assertThat(createdFromParcelData[0].first).isEqualTo(TEST_STRING)
>             assertThat(createdFromParcelData[0].second).isEqualTo(TEST_LONG)
>         }
>     }
> }
> ```
>
> 

***

# TDD, BDD

TDD(Test Driven Development)

> TDD는 테스트 주도 개발로 설계,  코드 개발 후 테스트 케이스 작성을 하는 기존의 개발 프로세스와는 다르게 테스트 케이스를 먼저 작성하고 코드를 개발하여 리펙토링하는 방식이다.
>
> TDD의 개발주기
>
> ![TDD-개발주기](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD-%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%8C%E1%85%AE%E1%84%80%E1%85%B5.png?resize=1024%2C680&ssl=1)
>
> 실패를 포함하는 유닛 테스트 코드를 먼저 작성한다. 그 후 테스트를 통과할 정도의 코드를 작성하고 Refactor 단계에서 이 코드를 개선한다. TDD에서는 이 과정을 반복하며 진행한다.
>
> TDD의 장점
>
> * 객체 지향적 코드 개발 
>   TDD는 코드의 재사용성을 보장하므로 소프트웨어 개발 시 기능별로 모듈화가 이루어진다. 소프트웨어를 의존성이 낮은 모듈로 조합된 형태로 만들며 모듈을 추가하거나 제거해도 소프트웨어의 전체적인 구조에는 영향을 미치지 않는다.
> * 재설계 시간의 단축
>   테스트 코드를 먼저 작성하기 때문에 개발자가 자신이 해야할 것을 명확하게 정의한 후 코드를 작성하게 된다. 또한 그 과정에서 설계의 오류나 다양한 예외사항들을 생각할 수 있어 코드를 개발하는 도중에 설계가 바뀌는 일을 줄일 수 있다.
> * 유지보수의 용이
>   유닛 테스팅을 하기 때문에 문제가 발생한다면 해당 문제가 어느 부분에서 발생하였는지 쉽게찾아낼 수 있다.
>
> TDD의 단점
>
> TDD의 단점은 개발 속도가 느려진다는 점이다. 코드를 작성하기 전 테스트 코드를 먼저 작성해야하고, 계속해서 코드를 리팩토링해야하기 때문이다.

BDD(Behavior Driven Development)

> BDD는 행동 주도 개발이다. TDD에서 파생된 개발 방법론으로, 개발자와 비개발자의 협업 과정을 녹여낸 방법이다. BDD는 사용자의 행위를 예상하여 결과를 테스트 하는 개발 방법이다. BDD는 사용자의 행위를 중심으로 테스트 코드를 작성한다. BDD에서 하나의 시나리오는 Given, When, Then 구조를 가지도록 권장한다.
>
> * Feature : 테스트 대상의 기능과 책임을 명시한다.
> * Scenario : 테스트 목적에 대한 상황을 설명한다.
>
> * Given : 시나리오 진행에 필요한 값, 즉 시나리오에서 주어진 환경을 설정한다.
> * When : 시나리오를 진행하는데 필요한 조건을 명시한다.
> * Then : 시나리오를 완료했을 때 도출되어야할 결과를 명시한다.

TDD와 BDD는 서로 상호보완적인 관계이다. 둘 중 하나만 선택해서 테스트 케이스를 작성하는 것이 아닌, BDD로 사용자의 행위에 대해 테스트 케이스를 작성하고 그 행위의 세부적인 부분들을 TDD로 테스트 케이스로 작성하는 것이 좋다.

***

# CI/CD

CI(Continuous Integration)

> 직역하면 지속적인 통합으로 코드의 새로운 변경사항을 정기적으로 빌드 및 테스트하여 공유 레포지토리에 통합하는 것을 의미한다. 다수의 개발자들이 현상관리 툴을 이용하여 하나의 프로젝트를 작업한다 했을 때 가능한 작은 기능 단위로 나누어 주기적이고 빈번하게 merge 해야한다. 많은 변경사항들을 한 번에 merge하게 되면 그만큼 충돌이 많이 일어나기 때문이다. merge를 자주 하기 위해선 그만큼 빌드와 테스트도 자주하게 된다. 이 빌드와 테스트를 자동화 시킨다면 개발자는 코드만 수정해서 merge하게 되면 알아서 그 코드를 검증해준다.

CD(Continuous Delivery, Continuous Deployment)

> CD는 지속적인 서비스 제공(Continuous Delivery)와 지속적인 배포(Continuous Delpoyment) 두 가지를 의미한다.
>
> 지속적인 서비스 제공이란 변경사항이 CI를 통해 빌드와 테스트가 검증되면 레포지토리에 자동으로 업로드 되는 것을 의미한다.
>
> 지속적인 배포는 지속적인 서비스 제공으로 레포지토리에 업로드된 코드를 프로덕션으로 배포0하는 과정을 자동화하는 것을 의미한다.

CI/CD

> ![CI/CD flow](https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/ci-cd-flow-desktop_edited_0.png?itok=TzgJwj6p)
>
> CI/CD는 지속적 통합, 지속적 서비스 제공만을 구축한 것을 말하기도 하고 이를 확장하여 지속적 배포까지 구축한 것을 의미하기도 한다. CI/CD는 앱 개발에 고수준의 지속적인 자동화와 모니터링을 추가한 것을 포함하는 파이프라인으로 시각화된 과정이다.

