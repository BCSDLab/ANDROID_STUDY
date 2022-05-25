# ANDROID_STUDY 2주차
## :one: Why do we use DI(Dependency Injection)
### :syringe: 종속성 주입(DI)이란?
> 클래스에는 흔히 다른 클래스 참조가 필요합니다. 이럴 때 클래스 사이에 의존성이 발생합니다.
> 예를 들어 ``` Car ```클래스는 ``` Engine ``` 클래스 참조가 필요할 수 있습니다.
> 이때 참조되는 ``` Engine ```클래스를 __종속항목__이라고 하며 Car 클래스가 실행되기 위해서는 ``` Engine ```클래스의 인스턴스가 있어야 합니다.
> 
__클래스가 필요한 객체를 얻는 방법에는 3가지가 있습니다.__
1. 클래스가 필요한 종속 항목을 구성하는 방법입니다.
``` kotlin
class Car {

    private val engine = Engine()  // 종속 항목 인스턴스 생성 및 초기화

    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val car = Car()
    car.start()
}
```
<br>

> 그러나 해당 코드의 문제점으로 ``` Engine ```이 가솔린 엔진이 아닌 전기 엔진을 넣고 싶을 경우

<br>

``` kotlin
class Car {

    // private val engine = Engine()  
    private val engine = ElectricEngine()  // 전기 엔진으로 직접 수정해주어야 합니다.

    fun start() {
        engine.start()
    }
}
```
> * 직접 사용자가 바꿔주어야 한다는 문제점이 발생합니다.
>  
> * 강력한 ``` Engine ``` 종속 항목 의존성은 테스트를 더욱 어렵게 만듭니다.

<br>

__위의 문제를 해결하기 위해 종속 항목을 삽입받게 되고 두 가지 방법으로 삽입 받을 수 있습니다.__
  * 생성자 삽입
    >  클래스의 종속 항목을 생성자에 전달합니다.

  * 필드 삽입(setter 삽입)
    > Activity 및 Fragment와 같은 특정 Android 프레임워크 클래스는 시스템에서 인스턴스화 하므로
    > 생성자 삽입이 불가능합니다. 그래서 필드 삽입을 사용하여 종속항목은 클래스가 생성된 후 인스턴스 화 됩니다.

<br>

2. 생성자 삽입 (매개변수로 제공받는 경우)
``` kotlin
class Car(private val engine: Engine){  // 매개변수 종속 항목 삽입
  fun start(){
    engine.start()
  }
}

fun main(args: Array){
  val engine = Engine()
  val car = Car(engine)
  car.start
}
```

3. 필드 삽입
``` kotlin
class Car{
  lateinit var engine : Engine

  fun start(){
    engine.start()
  }
}

fun main(args: Array){
  val car = Car()
  car.engine = Engine()  // car 인스턴스 engine 필드에 삽입
  car.start()
}
```
> * ``` Car ```클래스를 재사용할 수 있습니다. 이때 외부에서 ``` ElectricEngine ```이라는 새로운 ```  Engine ``` 서브 클래스의 인스턴스를 전달하기만 하면 됩니다.
>  
> * ``` Car ```클래스를 다양한 테스트 할 경우 그에 맞는 ``` Engine ```을 생성하여 다양한 테스트에 맞게 구성할 수 있습니다.

### :pushpin: 결론
* 클래스 재사용 가능 및 종속 항목 분리
  > 종속 항목 구현을 쉽게 교체할 수 있습니다. 컨트롤 반전으로 인해 코드 재사용이 개선되었으며 
  > 클래스가 더 이상 종속 항목 생성 방법을 제어하지 않지만, 대신 모든 구성에서 작동합니다.

* 리펙토링 편의성
  > 종속 항목은 API 노출 영역의 검증 가능한 요소가 되므로 구현 세부정보로 숨겨지지 않고
  > 객체 생성 타임 또는 컴파일 타임에 확인할 수 있습니다.


* 테스트 편의성
  > 클래스는 종속 항목을 관리하지 않으므로 테스트 시 다양한 구현을 전달하여 
  > 다양한 모든 사례를 테스트 할 수 있습니다.

### :file_folder: DI Library(Koin, Dagger, Hilt)
#### 1. Koin
> * Kotlin을 위한 DI 라이브러리 "순수 Kotlin만으로 작성"
> * Dagger에 비해 상대적으로 낮은 학습곡선을 갖는다. (쉽게 배울 수 있다)
> * Dagger와 달리 Annotation 프로세싱 및 리플렉싱이 없다
> * 런타임때 오브젝트 그래프를 그린다. (의존성 주입을 한다) 이는 앱 성능을 저하시킨다는 단점이 있다.

* Koin DSL (도메인 특화 언어)
  > * ``` module ``` : koin 모듈을 정의한다.
  > 
  > * ``` viewModel ``` : Activity나 Fragment에 각 ViewModel을 주입한다.
  > 
  > * ``` factory ``` : 의존성 주입(inject / get) 시점마다 새로운 객체를 매번 생성한다.
  > 
  > * ``` single ``` : 해당 객체를 싱글톤으로 생성한다. (App Lifecycle 동안 단일 인스턴스)
  > 
  > * ``` bind ``` : 생성할 객체를 ㅏ른 Type으로 바인딩 (Class, Interface 상속관계 필요)
  > 
  > * ``` get() ``` : Component 내에서 알맞은 의존성을 주입
  > 
  > * ``` inject() ``` : get과 같이 알맞은 의존성 주입 (by inject() 방식, val에만 가능하다)

* Koin 동작 방식
  > 1. Module 선언(생성)
  > 2. Application 단위 Class에서 startKoin()으로 Koin 실행
  > 3. 의존성 주입 - 구성요소 (Activity, Fragment, Class 등)

* __inject__와 __get__ 방식
  > ``` var 변수명 : Type = get() ``` 형식 : Koin이 해당 변수의 Type을 보고 알맞는 의존성 주입
  >
  > ``` var 변수명 = get<Type>() ```형식 : Koin이 get() 함수에 선언된 Type에 맞는 의존성 주입

  > ``` inject ``` : Lazy 방식의 주입, 해당 객체가 사용되는 시점에 의존성 주입
  > 
  > ``` get ``` : 바로 주입, 해당 코드 실행시간에 바로 객체를 주입

#### 2. Dagger
> * Android 앱에서 수동 종속 항목 삽입 또는 서비스 로케이터는 프로젝트의 크기에 따라 문제가 될 수 있어 Dagger를 사용하여 종속 항목을 관리해야 함.
> 
> * Dagger는 개발자가 직접 작성했을 코드를 모방하는 코드를 자동으로 생성한다.

* Dagger의 5가지 필수 개념
  > * Inject : 필드, 생성자, 메서드에 붙여 Component로 부터 의존성 객체를 주입 요청하는 annotation
  >
  > * Component : 노출하는 유형을 충족하는 데 필요한 모든 종속 항목이 있는 컨테이너를 생성하도록 Dagger에 지시 (Interface 또는 abstract class에만 사용 가능하다)
  >
  > * SubComponent : 부모 Component가 있는 자식 Component (Inject로 의존성 주입을 요청받으면 SubComponent에서 먼저 의존성을 검색하고, 없으면 부모로 올라가면서 검색합니다.)
  >
  > * Module : Component에 연결되어 의존성 객체를 생성하는 역할을 합니다. (클래스에만 사용 가능) ``` @Provides ``` ``` @Bind ```메서드를 통해 관리
  >
  > * Scope : 생성된 객체의 Lifecycle 범위입니다. PerActivity, PerFragment 등으로 생명주기와 맞춰서 사용합니다.

* :chart_with_upwards_trend: 의존성 주입 Flow
![image](https://blog.kakaocdn.net/dn/bk9fSB/btqD2F4VTcc/V2bY8N7DkiPKa8qlHUr4Bk/img.png)

  > 1. ``` @Inject ``` annotation이 붙은 Type 체크 (인스턴스 종류)
  > 
  > 2. ``` SubComponent ``` 에서 먼저 해당 Type 반환 의존성 있는지 검사 (Component에 등록된 Module의 ``` @Provides ``` ``` @Binds ``` 메서드의 반환 Type 동일 여부)
  > 
  > 3. SubComponent에 없다면 ``` 상위 Component ```에서 의존성 검사   
  >   <br>
  >> Module에서 동일 Type 반환하는 의존성 메서드 발견 시 Scope 등록 여부에 따라 인스턴스를 새로 생성하거나 기존의 것을 그대로 Return 할 지 결정합니다.

#### 3. Hilt
> * Hilt는 프로젝트에서 수동 종속 항목 삽입을 실행하는 상요구를 줄이는 Android용 종속 항목 삽입 라이브러리 입니다. 
> 
> * Hilt는 Android 클래스에 컨테이너를 제공하고 수명 주기를 자동을 관리함으로써 애플리케이션에서 DI 사용하는 표준 방법을 제공합니디.
>
> * Hilt는 Dagger가 제공하는 컴파일 시간 정확성, 런타임 성능, 확장성 및 Android 스튜디오 지원의 이점을 누리기 위해 Dagger를 기반으로 빌드되었습니다.
> * Hilt는 Java 8 기능을 사용합니다.

* Andorid 클래스에 종속 항목 삽입


  > ``` Application  ```클래스에 Hilt를 설정하고 애플리케이션 수준 구성요소를 사용할 수 있게 되면 Hilt는 ``` @AndroidEntryPoint ```주석이 있는 다른 Android 클래스에 종속 항목을 제공할 수 있습니다.
  ``` kotlin
  @AndroidEntryPoint
  class ExampleActivity : AppCompatActivity() { ... }
  ```

* Android 지원 클래스
  > * Application
  >
  > * Activity
  >
  > * Fragment
  > 
  > * View
  > 
  > * Service
  > 
  > * BroadcastReceiver
  
  > Android 클래스에 ``` @AndroidEntryPoint ``` 로 annotation을 지정하면 이 클래스에 종속된 Android 클래스에도 주석을 지정해야 합니다. ``` @AndroidEntryPoint ```는 프로젝트의 각 Android 클래스에 관한 개별 Hilt Component를 생성합니다. Component에서 종속 항목을 가져오려면 다음과 같이 ``` @Inject ```주석을 사용하여 필드 삽입을 실행합니다.

  ``` kotlin
  @AndroidEntryPoint
  class ExampleActivity : AppCompatActivity() {
    @Inject lateinit var analytics : AnalyticsAdapter
  }
  ```

  Hilt 결합 정의
  > 필드 삽입을 실행하려면 Hilt가 해당 구성요소에서 필요한 종속 항목의 인스턴스를 제공하는 방법을 알아야 합니다.
  >
  > Hilt에 결합 정보를 제공하는 한 가지 방법은 생성자 삽입입니다. annotation이 지정된 클래스 생성자의 매개변수는 그 클래스의 종속 항목입니다.
  ``` kotlin
  class AnalyticsAdapter @Inject constructor(
    private val service AnalyticsService
  ){ ... }
  ```

:hammer: Hilt 모듈
> Hilt모듈은 ``` @Module ```로 주석이 지정된 클래스입니다. 그러나 Dagger 모듈과 달리, Hilt 모듈에 ``` @InstallIn ```주석을 지정하여 각 모듈을 사용하거나 설치할 Android 클래스를 Hilt에 알려야 합니다.

<br>

* ``` @Binds ```를 사용하여 인터페이스 인스턴스 삽입

    > ``` @Binds ``` annotation은 인터페이스의 인스턴스를 제공해야 할 때 사용할 구현을 Hilt에 알려줍니다.
    > 
    > annotation이 지정된 함수는 Hilt에 다음 정보를 제공합니다.
    >
    > * 함수 반환 유형은 함수가 어떤 인터페이스의 인스턴스를 제공하는지 Hilt에 알려줍니다.
    > * 함수 매개변수는 제공할 구현을 Hilt에 알려줍니다.

* ``` @Provids ```를 사용하여 인스턴스 삽입

    > 클래스가 외부 라이브러리에서 제공되므로 클래스를 소유하지 않은 경우 (Retrofit, OkHttpClient 또는 Room 데이터베이스와 같은 클래스) 또는 빌더패턴으로 인스턴스를 생성해야 하는 경우도 생성자 삽입이 불가능합니다.
    >
    > annotation이 달린 함수는 Hilt에 다음 정보를 제공합니다.
    >
    > * 함수 반환 유형은 함수가 어떤 유형의 인스턴스를 제고하는지 Hilt에 알려줍니다.
    > * 함수 매개변수는 해당 유형의 종속 항목을 Hilt에 알려줍니다.
    > * 함수 본문은 해당 유형의 인스턴스를 제공하는 방법을 Hilt에 알려줍니다. Hilt는 해당 유형의 인스턴스를 제공해야 할 때마다 함수 본문을 실행합니다.

* Hilt의 사전 정의된 한정자

  > Hilt는 몇 가지 사전 정의된 한정자를 제공합니다. 예를 들어 애플리케이션 또는 Activity의 ``` Context ```클래스가 필요할 수 있으므로 Hilt는 ``` @ApplicationContext ``` 및 ``` @ActivityContext ```한정자를 제공합니다.

<br>

:sparkles: Android 클래스용으로 생성된 구성요소
> 필드 삽입을 실행할 수 있는 각 Android 클래스마다 ``` @InstallIn ```주석에 참조할 수 있는 관련 Hilt 구성요소가 있습니다.
> 
  * Hilt는 다음 구성요소를 제공합니다.

  | __Hilt 구성요소__ | __인젝터 대상__ | __범위__|
  | ----------------- | --------------- |---------|
  | ApplicationComponent | Application | @Singleton |
  | ActivityRetainedComponent      | ViewModel   | @ActivityRetainedScope |
  | ActivityComponent      | Activity   | @ActivityScoped |
  | FragmentComponent      | Fragment   | @FragmentScoped |
  | ViewComponent      | View   | @ViewScoped |
  | ViewWithFragmentComponent      | @WithFragmentBindings 주석이 지정된 View   | @ViewScoped |
  | ServiceComponent      | Service   | @ServiceScoped |

  * 구성요소 계층 구조

    > 구성요소에 모듈을 설치하면 이 구성요소의 다른 결합 또는 구성요소 계층 구조에서 그 알래에 있는 하위 구성요소의 다른 결합의 종속 항목으로 설치된 모듈의 결합에 액세스할 수 있습니다.

  ![image](https://developer.android.com/images/training/dependency-injection/hilt-hierarchy.svg)


:vs: Hilt 및 Dagger
  > * 수동으로 생성해야하는 Dagger와 달리 __Android 프레임워크 클래스를 통합하기 위한 구성요소__
  > * Hilt가 자동으로 생성하는 구성요소와 함께 사용할 __범위 annotation__
  > * ``` Application ```또는 ``` Activity ```와 같은 Android 클래스를 나타내는 __사전 정의된 결합__
  > * ``` @ApplicationContext ```및 ``` @ActivityContext ```를 나타내는 __사전 정의된 한정자__

## :two: JVM, DVM, ART Difference
### 1. JVM (Java Virtual Machine)
> 자바 바이트 코드를 실행할 수 있는 주체입니다.
> * 모든 기본 타입의 정의를 명확히 함으로써 __플랫폼 독립성__을 보장해줍니다.
> * 데이터 흘름 분석에 기반한 __자바 바이트코드__ 검증기를 통해 많은 문제를 실행 전에 검증하여 실행 시 안전을 보장하고 별도의 부담을 줄여줍니다.
> * GC(Garbage Collection) 사용
>
>   * StackOverFlow
>   * 명령어 피연산자의 타입 규칙 위반
>   * 지역 변수의 초기화 전 사용
>   * 필드 접근 규칙 위반 등

*  _자바 바이트 코드란?_

    >   * 플랫폼에 독립적
    >   * JVM에 의존적
    >   * 한번의 빌드로 여러 플랫폼에서 실행가능 (__WORA: Write Once Read Anywhere__)

### 2. DVM (Dalvik Virtual Machine)
![image](https://velog.velcdn.com/images%2Fsery270%2Fpost%2Fb935d9f4-072e-49db-91cb-780a3afd1d8e%2Fimage.png)

> * 초기 Android Run Time으로, Android 2.2 이후 trace JIT 컴파일러로 처음 적용되었습니다.
> * 모바일 기기 특상 배터리 수명, 컴퓨팅 파워 및 메모리가 PC에 비해 열악합니다. 그렇기 때문에 모바일 기기 환경에 맞게 최적화 되어 나온 가상머신이 바로 DVM 입니다.

#### :alarm_clock: JIT (Just In Time)
> * 자주 사용하는 부분에 대해 미리 컴파일하여 기계어로 해석해 놓기 때문에 실행 성능을 향상시킬 수 있습니다.
> * 실행시에 인터프리팅 시작을 시작합니다.
> * 인터프리터 방식의 단점을 보안하기 위해 도입되었습니다.
> * JIT컴파일러가 컴파일하는 과정은 바이트코드를 인터프리팅하는 것보다 훨씬 오래걸립니다.
> * 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행합니다.

*  _trace-JIT 이란?_

    > Threshold를 초과하면 바이트코드를 기계어로 해석합니다. 즉 어떤 구간 (ex. if문, for문)이 특정 횟수 이상 반복되면 컴파일을 수행합니다.

  
#### :vs: JVM과 DVM
![image](https://velog.velcdn.com/images%2Fsery270%2Fpost%2F27f49b0d-f5aa-4c33-a0e9-01be0488f623%2Fimage.png)

> JVM이나 DVM이나 모두 Java를 사용하는 VM이기 때문에 비슷한 특성을 갖고 있습니다.

* 왜 DVM을 사용할까?

  >   1. 라이선스 문제
  >   2. 모바일 환경에 대한 최적화 (배터리, 컴퓨팅파워, 메모리 제약 문제 등)


### 3. ART (Android Runtime)
> * Android 런타임(ART)은 Android의 애플리케이션 및 일부 시스템 서비스에서 사용하는 관리형 런타임입니다.
> * AOT(Ahead of time) 컴파일 도입
> * 가비지 컬렉션 개선
> * 개발 및 디버깅 개선

*  _AOT(Ahead-of-time) 이란?_

    > * 설치 시점에 이미 컴파일을 완료하여 기계어로 해석을 끝냅니다.
    > * 실행시에는 해석 과정 없이 바로 기계어를 실행합니다.

#### :vs: DVM과 ART
> Dalvik용으로 개발된 앱은 ART로 실행 시에도 작동해야 합니다. 그러나 Dalvik에서 작동하는 기술 중 일부는 ART에서 작동하지 않습니다.

|   | DVM | ART |
| ------ | --- | ----- |
| 컴파일러 | JIT | AOT |
| 실행 | Dex, ODEX | OAT(ART) |
| GC | CMS 알고리즘 | Customized CMS 알고리즘 |
| 설치 속도 | 빠름 | 느림 |
| 실행 속도 | 느림 | 빠름 |
| 배터리 수명 | 짧다 | 길다 |
| 앱 설치 공간 | 적게 필요 | 많이 필요 |

## :three: What is GC?
> GC란 Garbage Collection의 약자로 동적 할당된 메모리 영역(힙 메모리) 가운데 더이상 사용할 수 없게 된 영역을 탐지하여 자동으로 해제하는 기법입니다.

__:muscle: GC 동작방식__
* 2가지 가정 (weak generational hypothesis)

  > 1. 대부분의 객체는 금방 unreachable 상태가 된다. (최근생긴 객체일수록 앞으로 사용되지 않을 확률이 크다.)
  > 2. 오래된 객체에서 새로 생긴 객체로의 참조는 거의 없다.

* Heap 메모리 구조
  > * __Young Generation (= Young 영역)__
  >   * 새로운 객체가 생성되는 위치, 이 영역에서 메모리 회수하는 것을 __Minor GC__가 발생헀다고 합니다.
  > * __Old Generation (= Old 영역)__
  >   * Young 영역에서 Tenuring Threashold(임계치)보다 오래 살아남은 객체들이 넘어오는 영역입니다.
  >   * 이 영역에서 일어나는 GC를 Major GC 또는 Full GC라고 합니다. 이때 STW(Stop The World)가 발생하여 JVM이 GC 스레드 빼고 모두 일시적으로 멈추는 현상이 발생합니다.
  >   * GC를 튜닝한다는 것은 Old 영역체서 발생한느 STW 시간을 단축시키는 것을 의미합니다.
  >
  > * Young 영역에서 Old영역으로 객체가 이동하는 것을 <mark> Promotion 되었다 </mark>고 합니다.
  > * Old 영역 중에서도 오래 살아남은 객체들은 __Permanent Generation__ 으로 이동하게 됩니다. 이곳 또한 Major GC가 발생하기 때문에 STW가 발생합니다.
  > * Old 영역에는 __card Table__ 이 존재합니다. (512 Byte)

  <center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSvFWl%2FbtqCSXAg6BF%2FtiDTwszghUjVSmavUFK3fK%2Fimg.png" width="400" height="400"></center>
  <center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFQRui%2FbtqCWLZet5i%2Fs7ZBTbNieFDpKyOrxvnh31%2Fimg.png" width="400" height="400"></center>

* 여러가지 GC 동작 방식

  Major GC를 수행할 때 어디에 초점을 맞추느냐에 따라 2가지 방식으로 분류
  >   * Throughput Collector  : GC 수행시 모든 리소스를 투입해서 GC를 빨리 끝내 STW를 단축시키는 것이 목적
  >     * Serial GC, Parallel GC, Parallel Old GC
  >
  >   * Low Pause(Latency) Collector : STW가 발생을 분산시켜 체감 pause timed을 줄이는 것이 목적. GC수행 동시에 작업도 수행할 수 있다.
  >     * CMS, G1 GC

  5가지 GC 알고리즘
    > * Serial GC 
    >   Minor GC & Major GC를 하나의 스레드로 작업합니다.
    >
    > * Parallel GC 
    >   Minor GC는 멀티스레드로 작업하고, Major GC는 하나의 스레드로 작업합니다.
    >
    > * Parallel Old GC 
    >   Minor GC & Major GC 모두 멀티 스레드로 작업합니다. 그렇기에 Multi CPU에 유리하고 Old GC처리량이 늘어납니다.
    >
    > * CMS GC (Concurrent Mark & Sweep)
    >   Low Latency GC로 Compaction을 기본적으로 수행하지 않다가 단편화(Fragmentation)이 심해지면 수행합니다.
    >
    > * G1GC
    >   CMS의 문제점을 개선하기 위해 만들어진 GC로 위의 4가지 방법과 구조가 다르다.

## :four: What is Memory leak?
> 메모리 누수란 어플리케이션에서 사용하지 않는 객체가 사용중인 어떤 객체를 참조중이라서 사용되지 않는 객체의 할당된 메모리를 GC가 회수해 갈 수 없는 현상입니다.

<br>

:droplet: 메모리 누수 발생시 일어나는 현상
  > 1. 앱 버벅거림
  >   GC는 Android 런타임에서 메모리가 부족할 때 작동이 됩니다. 이때 memory leak가 많으면 많을수록 Android 런타임에서 메모리가 계속 부족해 GC를 많이 호출하게 되고 이는 안드로이드의 UI 렌더링이나 이벤트처리를 중지시킵니다.
  >
  > 2. 앱 중단
  >   안드로이드 앱의 응답을 액티비티 매니저와 윈도우 매니저 시스템 서비스가 항상 모니터링하고 있습니다. 이때 5초 이내에 입력이벤트가 없거나 BroadCastReceiver가 10초 내에 실행을 완료하지 않을 때 ANR이 발생합니다.

:no_entry_sign: 메모리 누수 원인
1. 액티비티에서 inner class 및 익명 class 사용
    > Class에서 익명 class를 만들경우 익명 클래스의 instance는 outer class의 instance에 대해 reference를 가지게 됩니다. 그래서 이를 해결하고자 static inner class의 instance를 넘기면 됩니다. 그리고 weakReference로 activity를 참조해야 합니다.

2. static 뷰 객체를 액티비티에서 사용
    > activity, view, context에 대해서 static 변수를 사용하지 않아야합니다.
    ``` 
    static TextView tv = new TextView(this)
    ``` 

3. handler를 액티비티가 종료된 후에도 사용
    > handler를 static으로 선언한 후 weakReference를 이용해야 한다.

4. 싱글톤 객체에서 액티비티 Context 참조
    > ``` OnDestory ```에서 해당 객체를 null을 하거나 가장 좋은 건 application Context를 사용하는 것입니다.

5. ViewModel에서 Activity의 context나 Activity View 참조
    > ViewModel에서 특정 액티비티 인스턴스보다 더 오랜 수명을 가질 수 있어 메모리 누수가 발생할 수 있습니다.

6. ViewBinding 사용시 Fragment에서 누수
    > 해당 코드를 사용해주지 않으면 Fragment보다 Fragement 내에 View의 생명주기가 더 오래 유지될 수 있습니다.
    ``` kotlin
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
    ```

### :eyes: Reference   
[DI란?](https://developer.android.com/training/dependency-injection)   
[Koin이란?](https://jaejong.tistory.com/153)   
[Dagger2](https://jaejong.tistory.com/125?category=888864)   
[Dagger2 Android 공식문서](https://developer.android.com/training/dependency-injection/dagger-android?hl=ko)    
[Hilt 안드로이드 공식 문서](https://developer.android.com/training/dependency-injection/hilt-android)   
[JVM, DVM, ART란](https://velog.io/@sery270/JVM-DVM-ART)   
[Android 런타임(ART) 및 Dalvik](https://source.android.com/devices/tech/dalvik)   
[GC란?](https://s2choco.tistory.com/14)   
[Memory leak란?](https://gift123.tistory.com/30)
