의존성(Dependency)

>클래스 사이의 의존성이란 어떤 A라는 클래스가 B라는 클래스를 이용하여 B의 메소드나 멤버가 바뀌면 B를 사용하는 A에 그 영향이 미칠 때 A가 B를 의존한다라고 한다. 이렇게 클래스 사이에 의존성이 생기게되면 B가 바뀌는 경우 그를 이용하는 A도 그에 맞게 바꿔 주어야하기 때문에 유지보수 측면에서 많은 불편을 겪어야하며 , Unit Test 또한 작성하기 힘들어진다. 이러한 문제들을 해결하기 위해서 의존성 주입(DI)이란 기법을 이용한다.

DI(Dependency Injection)

>DI의 주 개념은 의존해야하는 클래스를 클래스 내부에서 생성하는 것이 아닌 클래스의 외부에서 생성하고 클래스에 주입하고 의존성 역전원리를 이용하여 클래스간의 의존성을 분리시키는 것이다.
>
>![Dependency Injection](https://www.tutorialsteacher.com/Content/images/ioc/DI.png)
>
>Injector가 Service 객체를 생성하여 클라이언트 객체에 주입시켜 서비스 객체의 생성에 대한 책임을 클라이언트 객체로부터 분리시킨다. Injector는 3가지 방식으로 의존성을 주입한다.
>
>* Constructor Injection : 클라이언트 객체의 생성자를 이용해서 주입하는 방식
>* Property(Setter) Injection : 클라이언트의 public 프로퍼티(세터)를 이용해서 주입하는 방식
>* Method Injection : 의존성이 필요한 메소드를 정의하는 인터페이스를 클라이언트가 구현하고, Injector는 이 인터페이스를 이용해 클라이언트에 의존성을 주입하는 방식이다.
>
>DI를 이용하면 다음의 장점을 가질 수 있다.
>
>* 클래스간의 의존성을 줄일 수 있다.
>* 코드의 재사용성을 높여준다.
>* 각 객체의 테스트가 용이해진다.



Koin in Android

> Koin은 코틀린 개발에서 사용할 수 있는 의존성 주입 라이브러리다.
>
> 안드로이드에서 <code>Application</code>을 상속받는 클래스에서 <code>startKoin</code>를 사용하고, <code>androidContext</code>를 통해 Android context를 주입할 수 있다.
>
> ```kotlin
> class MainApplication : Application() {
> 
>  override fun onCreate() {
>       super.onCreate()
> 
>       startKoin {
>           // Koin Android logger
>           androidLogger()
>           //inject Android context
>           androidContext(this@MainApplication)
>          // use modules
>           modules(myAppModules)
>      }
> 
>  }
> }
> ```
>
> <code>Application</code>이 아닌 다른 안드로이드 클래스에서 Koin을 시작해야한다면 <code>startKoin</code>를 사용하고 Android <code>Context</code> 인스턴스를 주입할 수 있다.
>
> ```kotlin
> startKoin {
>  //inject Android context
>  androidContext(/* your android context */)
>  // use modules
>  modules(myAppModules)
> }
> ```
>
> <code>KoinApplication</code> 인스턴스 에는 Koin logger를 안드로이드에서 구현한 <code>AndroidLogger()</code>를 사용하는 <code>androidLogger</code> 가 있다. 
>
> ```kotlin
> class MainApplication : Application() {
> 
>  override fun onCreate() {
>      super.onCreate()
> 
>      startKoin {
>          //inject Android context
>          androidContext(/* your android context */)
>          // use Android logger - Level.INFO by default
>          androidLogger()
>          // use modules
>          modules(myAppModules)
>      }
>  }
> }
> ```
>
> <code>androidContext()</code>, <code>androidApplication()</code> 함수는 Koin module에서 <code>Context</code>를 얻을 수 있게 하여 <code>Application</code>가 필요한 표현식을 단순하게 작성할 수 있도록 해준다.
>
> ```kotlin
> val appModule = module {
> 
>  // create a Presenter instance with injection of R.string.mystring resources from Android
>  factory {
>      MyPresenter(androidContext().resources.getString(R.string.mystring))
>  }
> }
> ```
>
> Activity, Fragment, Service는 KoinComponents 표현으로 확장된다.
> ```kotlin
> val androidModule = module {
>  // a factory of Presenter
>  factory { Presenter() }
> }
> 
> // by injecy() : Koin container에 의해 lazy evaluation되는 인스턴스
> class DetailActivity : AppCompatActivity() {
> 
>  // Lazy injected Presenter instance
>  override val presenter : Presenter by inject()
> 
>  override fun onCreate(savedInstanceState: Bundle?) {
>      super.onCreate(savedInstanceState)
>      // ...
>  }
> 
> // get() : Koin container에 의해 즉시 가져와지는 인스턴스
> class DetailActivity : AppCompatActivity() {
> 
>  override fun onCreate(savedInstanceState: Bundle?) {
>      super.onCreate(savedInstanceState)
> 
>      // Retrieve a Presenter instance
>      val presenter : Presenter = get()
>  }
> ```
>
> 안드로이드 요소는 수명주기에 의해 관리되기 때문에 Koin module을 통해서 설명할 수 없다. 이런 수명주기를 존중하면서 의존성을 주입해야 한다. 구성요소를 3가지로 분류한다.
>
> * long live components (Service , Data Repository 등) : 여러화면에서 사용될 수 있고, dropped되지 않는 것들
> * medium live components (user sessions 등) : 여러화면에서 사용될 수 있고, 일정 시간 뒤 dropped되야 하는 것들
> * short live components (views) : 하나의 화면에서만 사용되고, 화면이 끝나면 dropped되야하는 것들
>
> long live components는 <code>single</code>로 쉽게 정의할 수 있다. medium, short live components는 몇몇 접근 방식을 이용할 수 있다. MVP 패턴의 Presenter로 예를 들면, <code>Presenter</code>는 short live component이므로 화면이 보여질 때마다 생성되어야하고, 화면이 끝날 때마다 drop되어야 한다.
>
> ```kotlin
> class DetailActivity : AppCompatActivity() {
> 
>     // injected Presenter
>     override val presenter : Presenter by inject()
> ```
>
> ```kotlin
> val androidModule = module {
> 
>     // Factory instance of Presenter
>     factory { Presenter() }
> }
> ```
>
> <code>factory</code>는 <code>by inject()</code>나 <code>get</code>로 호출될 때마다 새로운 인스턴스를 제공한다.
>
> ```kotlin
> val androidModule = module {
> 
>     scope<DetailActivity> {
>         scoped { Presenter() }
>     }
> }
> ```
>
> <code>scoped</code>는 <code>scope</code>에서 정해진 범위 내에서만 유효한 인스턴스를 제공한다.
>
> 
>
> Koin은 Activity나 Fragment에 대해 선언된 범위를 직접 사용할 수 있도록  <code>ActivityScope</code>, <code>FragmentScope</code>클래스를 제공한다.
>
> ```kotlin
> class MyActivity : ActivityScope() {
>     
>     // MyPresenter is resolved from MyActivity's scope 
>     val presenter : MyPresenter by inject()
> }
> ```
>
> 이런 클래스를 사용하지 않는다면 <code>AndroidScopeComponent</code> 인터페이스를 이용하고 <code>scope</code> 프로퍼티를 구현해야 한다. 이 프로퍼티는 다음 4가지 위임과 함께 사용할 수 있다.
>
> * <code>activityScope()</code> : Activity의 생명 주기를 따른다.
> * <code>activityRetainedScope()</code> : Activity의 ViewModel의 생명주기를 따른다.
> * <code>fragmentScope()</code> : Fragment의 생명주기를 따르고, 부모의 scope에 연결된다.
> * <code>serviceScope()</code> : Service의 생명주기를 따른다.
>
> ```kotlin
> class MyActivity : AppCompatActivity, AndroidScopeComponent {
>     // get current Activity's scope
>     override val scope : Scope by activityScope()
>     // MyPresenter is resolved from MyActivity's scope 
>     val presenter : MyPresenter by inject()
> }
> ```
>
> ```kotlin
> class MyActivity : AppCompatActivity, AndroidScopeComponent {
>     // use Activity Retained Scope
>     override val scope : Scope by activityRetainedScope()
>     val presenter : MyPresenter by inject()
> }
> ```
>
>
> ViewModel 주입
>
> ```kotlin
> val appModule = module {
> 
>     // ViewModel for Detail View
>     viewModel { DetailViewModel(get(), get()) }
> 
> }
> ```
>
> <code>get()</code>함수를 이용해 의존성을 주입할 수 있다. <code>viewModel</code>키워드는 ViewModel의 인스턴스 factory를 정의하는 것을 도와준다. 이 인스턴스는 ViewModelFactory에 의해서 처리되고 필요할 때 ViewModel 인스턴스를 다시 연결한다. 
>
> ViewModel을 <code>Activity</code>, <code>Fragment</code>, <code>Service</code>에 주입할 때는 <code>by viewModel()</code>, <code>getViewModel</code>를 사용한다. 
>
> ```kotlin
> class DetailActivity : AppCompatActivity() {
> 
>     // Lazy inject ViewModel
>     val detailViewModel: DetailViewModel by viewModel()
> }
> ```
>
> 하나의 뷰모델이 Fragment들이나 부모 Activity에서 공유될 수 있다. 이런 shared ViewModel을 주입할 때에는 <code>by sharedViewModel()</code>, <code>getSharedViewModel()</code>를 사용할 수 있다.
>
> ```kotlin
> val weatherAppModule = module {
> 
>     // WeatherViewModel declaration for Weather View components
>     viewModel { WeatherViewModel(get(), get()) }
> }
> ```
>
> ```kotlin
> class WeatherActivity : AppCompatActivity() {
> 
>     /*
>      * Declare WeatherViewModel with Koin and allow constructor dependency injection
>      */
>     private val weatherViewModel by viewModel<WeatherViewModel>()
> }
> 
> class WeatherHeaderFragment : Fragment() {
> 
>     /*
>      * Declare shared WeatherViewModel with WeatherActivity
>      */
>     private val weatherViewModel by sharedViewModel<WeatherViewModel>()
> }
> 
> class WeatherListFragment : Fragment() {
> 
>     /*
>      * Declare shared WeatherViewModel with WeatherActivity
>      */
>     private val weatherViewModel by sharedViewModel<WeatherViewModel>()
> }
> ```
>
> ViewModel의 state를 다루기 위해 생성자에 <code>SavedStateHandle</code>를 추가할 수 있다.
>
> ```kotlin
> class MyStateVM(val handle: SavedStateHandle, val myService : MyService) : ViewModel()
> ```
>
> Koin module 에서는 <code>get()</code>이나 parameters를 이용할 수 있다.
>
> ```kotlin
> viewModel { MyStateVM(get(), get()) }
> // or
> viewModel { params -> MyStateVM(params.get(), get()) }
> ```
>
> state ViewModel을 <code>Activity</code>, <code>Fragment</code>에 주입할 때에는 <code>by stateViewModel()</code>와 <code>getStateViewModel()</code>를 사용한다. shared state ViewModel을 주입할 때에는 <code>by sharedStateViewModel()</code>와 <code>getSharedStateViewModel()</code>를 사용한다.
>
> <code>SavedStateHandle</code>을 ViewModel에 넘겨 Bundle 인수를 전달하는 걸 도울 수도 있다. <code>stateViewModel</code>함수에 <code>state</code>을 이용하면 된다.
>
> ```kotlin
> val myStateVM: MyStateVM by stateViewModel( state = { Bundle(...) })
> ```
>
>
> Koin에서는 <code>Fragment</code>주입을 돕는 <code>KoinFragmentFactory</code> 직접적으로 가져올 수 있다.
>
> KoinApplication에서 <code>fragmentFactory()</code>키워드로  <code>KoinFragmentFactory</code> 인스턴스를 가져올 수 있다.
>
> ```kotlin
>  startKoin {
>     // setup a KoinFragmentFactory instance
>     fragmentFactory()
> 
>     modules(...)
> }
> ```
>
> <code>Fragment</code>인스턴스를 정의하기 위해 <code>fragment</code> 키워드를 이용한다.
>
> ```kotlin
> val appModule = module {
>     single { MyService() }
>     fragment { MyFragment(get()) }
> }
> ```
>
> <code>Activity</code>에서  `setupKoinFragmentFactory()`로 fragment factory를 가져온다.
>
> ```kotlin
> class MyActivity : AppCompatActivity() {
> 
>     override fun onCreate(savedInstanceState: Bundle?) {
>         // Koin Fragment Factory
>         setupKoinFragmentFactory()
> 
>         super.onCreate(savedInstanceState)
>         //...
>     }
> }
> ```
>
> 그 후 `FragmentManager`를 이용한다.
>
> Koin Activity의 Scope에서 사용하고 싶다면 `scope`로 fragment를 선언하면 된다.
>
> ```kotlin
> val appModule = module {
>     scope<MyActivity> {
>         fragment { MyFragment(get()) }
>     }
> }
> ```
>
> 그리고 `setupKoinFramentFactory(lifecycleScope)`로 Fragment factory를 지정해준다.

Dagger & Android

> Dagger는 Java, Kotlin, Android를 위한 완전히 정적인 컴파일 시간 의존성 주입 프레임워크다.
>
> Activity 주입
>
> 1. application에 `AndroidInjectionModule`을 설치하여 기본 타입들이 필요한 binding을 이용할 수 있도록 한다.
>
> 2. `AndroidInjector<YourActivity>`를 구현하는  `@Subcomponent`와 `AndroidInjector.Factory<YourActivity>`를 구현하는 `@Subcomponent.Factory`를 작성한다.
>
>    ```java
>    @Subcomponent(modules = ...)
>    public interface YourActivitySubcomponent extends AndroidInjector<YourActivity> {
>      @Subcomponent.Factory
>      public interface Factory extends AndroidInjector.Factory<YourActivity> {}
>    }
>    ```
>
> 3. subcomponent를 정의한 후에는, subcomponent factory를 bind하는 모듈을 정의하고 `Application`을 주입하는 component에 추가하여 subcomponent를 component 계층에 추가한다.
>    ```java
>    @Module(subcomponents = YourActivitySubcomponent.class)
>    abstract class YourActivityModule {
>      @Binds
>      @IntoMap
>      @ClassKey(YourActivity.class)
>      abstract AndroidInjector.Factory<?>
>          bindYourAndroidInjectorFactory(YourActivitySubcomponent.Factory factory);
>    }
>          
>    @Component(modules = {..., YourActivityModule.class})
>    interface YourApplicationComponent {
>      void inject(YourApplication application);
>    }
>    ```
>
>    
>
> 4. `Application`이 `HasAndroidInjector`를 구현하게 하고, `androidInjector()`메소드가 `@Inject DispatchingAndroidInjector<Object>`를 반환하도록 한다.
>
>    ```java
>    public class YourApplication extends Application implements HasAndroidInjector {
>      @Inject DispatchingAndroidInjector<Object> dispatchingAndroidInjector;
>          
>      @Override
>      public void onCreate() {
>        super.onCreate();
>        DaggerYourApplicationComponent.create()
>            .inject(this);
>      }
>          
>      @Override
>      public AndroidInjector<Object> androidInjector() {
>        return dispatchingAndroidInjector;
>      }
>    }
>    ```
>
>    
>
> 5. 마지막으로 `Activity`의 `onCreate()`에서 `super.onCreate()`를 호출하기 전에 `AndroidInjection.inject(this)`를 먼저 호출한다.
>
>    ```java
>    public class YourActivity extends Activity {
>      public void onCreate(Bundle savedInstanceState) {
>        AndroidInjection.inject(this);
>        super.onCreate(savedInstanceState);
>      }
>    }
>    ```
>
>
> Fragment 주입
>
> `Fragment`를 주입하는 것은 `Actvity`를 주입하는 것과 같은 방식으로 subcomponent를 정의한다. `Activity`에서 `onCreate()`에서 주입하는 것 대신, `Fragment`에서는 `onAttach()`에서 주입한다. `Actvity`와 달리 `Fragment`에서는 module을 설치할 위치를 선택할 수 있다. `Fragment` component를 다른 `Fragment`, `Activity`, `Application` component의 subcomponent로 만들 수 있다. component의 위치를 결정한 후에 해당 타입이 `HasAndroidInjector`를 구현하도록 한다.
>
> `DispatchingAndroidInjector`는 런타임에 적절한 `AndroidInjector.Factory`를 찾기 때문에 기본 클래스들이 `HasAndroidInjectort`를 구현하고 `AndroidInjection.inject()`를 호출할 수 있다. 각 하위 클래스들이 수행해야하는 모든 작업은 해당 `@Subcomponent`를 binding하는 것이다. 이를 수행하기 위해 Dagger에서는 클래스 계층 구조가 복잡하지 않은 경우 `DaggerActivity`, `DaggerFragment`, `DaggerApplication`와 같은 기본 타입들을 제공한다.

Hilt

> Hilt는 Dagger의 의존성 주입을 Android에 통합하는 표준 방법을 제공한다. Hilt의 목적은 다음과 같다.
>
> * Android용 Dagger 관련 인프라를 단순화한다.
> * 앱 간의 코드 공유와 가독성 및 이해를 쉽게 하기 위해 표준 component 및 범위를 생성한다.
> * 다양한 빌드 유형에 서로 다른 binding을 공급하기 쉬운 방법을 제공한다.
>
> Hilt를 사용함으로써 얻을 수 있는 장점
>
> * boierplate 코드를 줄일 수 있다.
> * build 의존성을 분리할 수 있다.
> * 구성을 단순화할 수 있다.
> * 테스트 모듈 변경이나 바인딩을 쉽게 할 수 있다.
> * 구성 요소 계층을 표준화할 수 있다.
>
>
> Hilt 구성요소 계층
>
> Hilt를 사용할 때에는 Dagger와 달리 구성요소를 직접 정의하거나 인스턴스하지 않고 Hilt에서 Android의 다양한 수명 주기에 자동으로 통합되는 기본 구성 요소를 제공한다.
>
> ![Hilt Component Hierarchy](https://dagger.dev/hilt/component-hierarchy.svg)
>
> 각 요소의 어노테이션은 해당 요소의 바인딩 범위를 지정하는데 사용되는 범위 지정 어노테이션이다. 자식 요소는 상위 요소의 바인딩에 종속될 수 있다.
>
> injection을 위한 요소
>
> | Component                       | Injector for                        |
> | ------------------------------- | ----------------------------------- |
> | **`SingletonComponent`**        | `Application`                       |
> | **`ViewModelComponent`**        | `ViewModel`                         |
> | **`ActivityComponent`**         | `Activity`                          |
> | **`FragmentComponent`**         | `Fragment`                          |
> | **`ViewComponent`**             | `View`                              |
> | **`ViewWithFragmentComponent`** | `View` with `@WithFragmentBindings` |
> | **`ServiceComponent`**          | `Service`                           |
>
> Android 클래스를 주입하기 위해 `@AndroidEntryPoint`같은 Hilt API를 사용할 때에는 표준 Hilt component가 injector로 사용된다. Injector로 사용된 component는 Android 클래스를 보여줄 binding을 결정한다.
>
> Component의 수명
>
> Component의 수명은 두 가지 방식으로 binding의 수명과 관련이 있다.
>
> 1. component가 생성되고 파괴되는 사이의 binding 수명을 제한한다.
> 2. 주입된 값의 멤버가 사용될 수 있을 때를 나타낸다.
>
> | Component                       | Scope                     | Created at               | Destroyed at              |
> | ------------------------------- | ------------------------- | ------------------------ | ------------------------- |
> | **`SingletonComponent`**        | `@Singleton`              | `Application#onCreate()` | `Application#onDestroy()` |
> | **`ActivityRetainedComponent`** | `@ActivityRetainedScoped` | `Activity#onCreate()`    | `Activity#onDestroy()`    |
> | **`ViewModelComponent`**        | `@ViewModelScoped`        | `ViewModel` created      | `ViewModel` destroyed     |
> | **`ActivityComponent`**         | `@ActivityScoped`         | `Activity#onCreate()`    | `Activity#onDestroy()`    |
> | **`FragmentComponent`**         | `@FragmentScoped`         | `Fragment#onAttach()`    | `Fragment#onDestroy()`    |
> | **`ViewComponent`**             | `@ViewScoped`             | `View#super()`           | `View` destroyed          |
> | **`ViewWithFragmentComponent`** | `@ViewScoped`             | `View#super()`           | `View` destroyed          |
> | **`ServiceComponent`**          | `@ServiceScoped`          | `Service#onCreate()`     | `Service#onDestroy()`     |
>
> component의 수명은 일반적으로 일치하는 Android 클래스의 인스턴스의 생성과 소멸로 제한된다.
>
> Scoped binding과 unscoped binding
>
> Dagger에서는 기본적으로 모든 bidning이 unscoped binding이다. 따라서 binding이 요청될 때마다 Dagger는 새로운 binding 인스턴스를 만든다. 하지만 Dagger에서도 특정 component의 scoped binding을 허용한다. scoped binding은 지정된 범위의 component당 한 번만 생성된다.
>
> ```kotlin
> // This binding is "scoped".
> // Each request from the same component instance for this binding will
> // get the same instance. Since this is the fragment component, this means
> // each request from the same fragment.
> @FragmentScoped
> class ScopedBinding @Inject constructor() {
> }
> ```
>
> module에서 scoped 지정
>
> ```kotlin
> @Module
> @InstallIn(FragmentComponent.class)
> object FooModule {
>   // This binding is "scoped".
>   @Provides
>   @FragmentScoped
>   fun provideScopedBinding() = ScopedBinding()
> }
> ```
>
> binding의 scoped 여부를 결정할 때 일반적인 규칙은 코드의 정확성을 위해 필요할 때만 scoped binding을 사용하는 것이다. 단지 성능을 이유로 scoped binding을 사용해야한다고 생각할 때는 `@Reusable`을 대신 사용할 것을 고려해야한다.
>
> | Component                       | Default Bindings                                          |
> | ------------------------------- | --------------------------------------------------------- |
> | **`SingletonComponent`**        | `Application`[2](https://dagger.dev/hilt/components#fn:2) |
> | **`ActivityRetainedComponent`** | `Application`                                             |
> | **`ViewModelComponent`**        | `SavedStateHandle`                                        |
> | **`ActivityComponent`**         | `Application`, `Activity`                                 |
> | **`FragmentComponent`**         | `Application`, `Activity`, `Fragment`                     |
> | **`ViewComponent`**             | `Application`, `Activity`, `View`                         |
> | **`ViewWithFragmentComponent`** | `Application`, `Activity`, `Fragment`, `View`             |
> | **`ServiceComponent`**          | `Application`, `Service`                                  |
>
> 각 Hilt component에는 커스텀 binding에 의존성으로 주입될 수 있는 기본 binding이 제공된다.
>
> Hilt Application
> Hilt를 사용하는 모든 앱은 `@HiltAndroidApp` 어노테이션을 사용한 Application 클래스를 포함해야 한다. `@HiltAndroidApp`은 Hilt component의 code generation을 시작하고 생성된 component들을 사용하는 애플리케이션의 기본 클래스들도 생성한다. code generation이 모든 모듈에 접근해야 하기 때문에 `Application`을 컴파일하는 대상도 추의 의존성에 모든 Dagger 모듈이 필요하다. 다른 Hilt 안드로이드 타입처럼  Application에도 멤버들이 주입될 수 있다. 즉,  Application에서 필드를 주입하여 `super.onCreate()`가 호출된 후 사용할 수 있다.
>
> ```kotlin
> @HiltAndroidApp
> class MyApplication : MyBaseApplication() {
>   @Inject lateinit var bar: Bar
> 
>   override fun onCreate() {
>     super.onCreate() // Injection happens in super.onCreate()
>     // Use bar
>   }
> }
> ```
>
>
> Android 타입
>
> `Application`에서 멤버들을 주입하면, `@AndroidEntryPoint` 를 사용하는 다른 Android 클래스에 멤버를 주입할 수 있다.  `@AndroidEntryPoint`는 다음 5가지 클래스에 사용할 수 있다.
>
> 1. Activity
> 2. Fragment
> 3. View
> 4. Service
> 5. BroadcastReceiver
>
> ```kotlin
> @AndroidEntryPoint
> class MyActivity : MyBaseActivity() {
>   // Bindings in SingletonComponent or ActivityComponent
>   @Inject lateinit var bar: Bar
> 
>   override fun onCreate(savedInstanceState: Bundle?) {
>     // Injection happens in super.onCreate().
>     super.onCreate()
> 
>     // Do something with bar ...
>   }
> }
> ```
>
> Hilt는 `ComponentActivity`를 상속하는 activity와 androidx의 `Fragment`를 상속하는 fragment만 지원한다.
>
> Retained Fragment
>
> fragment의 `onCreate()`에서 `setRetainInstance(true)`를 호출하면 구성 변경 시 소멸되었다가 다시 생성되는 것이 아니라 인스턴스를 유지하게 된다. Hilt fragment는 component에 대한 참조를 가지고 있고, component가 이전 Activity에 대한 참조를 가지고 있기 때문에 retained 되서는 안된다. 또, Hilt fragment가 retained되면 fragment에 주입된 scoped binding과 provider가 메모리 누수의 원인이 될 수 있다. Hilt fragment가 retained되는 것을 막기 위해 retained 된 것이 감지되면 구성 요소가 변화할 때 런타임 예외가 발생할 것이다. Hilt activity에 연결된 non Hilt fragment는 retained 될 수 있지만, 자식으로 Hilt fragment를 가진다면 런타임 예외가 발생할 것이다.
>
> Fragment binding이 있는 View
>
> 기본적으로는 `SingletonComponent`와 `ActivityComponent`binding만 view에 주입할 수 있다. fragment binding을 view에 주입하고 싶다면 `@WithFragmentBindings`어노테이션을 추가하면 된다.
>
> ```kotlin
> @AndroidEntryPoint
> @WithFragmentBindings
> class MyView : MyBaseView {
>   // Bindings in SingletonComponent, ActivityComponent,
>   // FragmentComponent, and ViewComponent
>   @Inject lateinit var bar: Bar
> 
>   constructor(context: Context) : super(context)
>   constructor(context: Context, attrs: AttributeSet?) : super(context, attrs)
> 
>   init {
>     // Do something with bar ...
>   }
> 
>   override fun onFinishInflate() {
>     super.onFinishInflate();
> 
>     // Find & assign child views from the inflated hierarchy.
>   }
> }
> ```
>
> Hilt ViewModel
> Hilt ViewModel은 생성자가 Hilt에 의해 주입되는 Jetpack ViewModel이다. Hilt로 ViewModel의 주입을 가능하게하려면 `@HiltViewModel` 어노테이션을 사용하면 된다.
>
> ```kotlin
> @HiltViewModel
> class FooViewModel @Inject constructor(
>   val handle: SavedStateHandle,
>   val foo: Foo
> ) : ViewModel
> ```
>
> Activity와 Fragment에서 `@AndroidEntryPoint` 어노테이션을 사용하면 `ViewModel`의 인스턴스를 `by viewModel()`로 받아올 수 있다.
>
> ```kotlin
> @AndroidEntryPoint
> class MyActivity : AppCompatActivity() {
>   private val fooViewModel: FooViewModel by viewModels()
> }
> ```
>
> `SavedStateHandel`는 모든 Hilt ViewModel에서 사용가능한 기본 binding이다. `ViewModelComponent`와 그 부모 component의 의존성만 ViewModel에 제공할 수 있다.
>
> ViewModel Scope
>
> 모든 Hilt ViewModel은 `ViewModel`과 생명주기가 같은 `ViewModelComponent`에 의해 제공된다. 즉, 구성이 변경되어도 살아남는다. ViewModel의 의존성에 범위를 지정하려면 `@ViewModelScoped` 어노테이션을 사용한다.
> `@ViewModelScoped`타입은 범위 지정된 타입의 single 인스턴스가 Hilt ViewModel에 주입된 모든 의존성에 걸쳐 제공되도록 한다. `ViewModel`의 다른 인스턴스는 scoped 인스턴스를 요청하면 다른 인스턴스를 받게될 것이다. `ViewModelComponent`에 범위를 지정하는 것은 ViewModel이 구성이 변경되어도 살아남고, 생명주기가 activity나 fragment에 의해 제어되기 때문에 유연하고 세분화된 범위를 지정할 수 있다. single 인스턴스가 다양한 ViewModel에 공유되어야 한다면 `@ActivityRetainedScoped`또는 `@Singleton`을 이용할 수 있다.
>
> ```kotlin
> @Module
> @InstallIn(ViewModelComponent::class)
> internal object ViewModelMovieModule {
>   @Provides
>   @ViewModelScoped
>   fun provideRepo(handle: SavedStateHandle) =
>       MovieRepository(handle.getString("movie-id"));
> }
> 
> class MovieDetailFetcher @Inject constructor(
>   val movieRepo: MovieRepository
> )
> 
> class MoviePosterFetcher @Inject constructor(
>   val movieRepo: MovieRepository
> )
> 
> @HiltViewModel
> class MovieViewModel @Inject constructor(
>   val detailFetcher: MovieDetailFetcher,
>   val posterFetcher: MoviePosterFetcher
> ) : ViewModel {
>   init {
>     // Both detailFetcher and posterFetcher will contain the same instance of
>     // the MovieRepository.
>   }
> }
> ```
>
>
> Hilt Module
>
> Hilt Module은 module을 설치할 Hilt component를 정하는 어노테이션인 `@InstallIn`이 추가된 표준 Dagger module이다. Hilt component가 생성될 때, `@InstallIn` 어노테이션을 가진 module은 `@Component#modules` 또는 `@Subcomponent#modules`를 통해 component나 subcomponent에 설치된다. Dagger에서 처럼 component에 module을 설치하는 것은 component 계층에서 component나 자식 component의 의존성에 binding이 접근할 수 있도록 한다. `@AndroidEntryPoint`에도 접근할 수 있다. 또한, component에 설치되는 것은 binding이 해당 component에 scoped되도록 한다.
>
> module은 `@InstallIn`어노테이션을 추가하여 Hilt component에 설치된다. 이 어노테이션은 Hilt를 사용하는 모든 Dagger module에서 요구되지만, 선택적으로 비활성화할 수 있다. module에 `@InstallIn`어노테이션이 없으면 module은 component에 속하지 않게되고, 컴파일 오류가 생길 수 있다.
> `@InstallIn`어노테이션에 적절한 component 타입을 넣어 module을 설치할 Hilt component를 지정한다.
>
> ```kotlin
> @Module
> @InstallIn(SingletonComponent::class) // Installs FooModule in the generate SingletonComponent.
> internal object FooModule {
>   @Provides
>   fun provideBar(): Bar {...}
> }
> ```
>
> 각 component에는 component의 수명에 대한 binding을 저장하는데 사용될 범위 지정 어노테이션을 제공한다.
>
> ```kotlin
> @Module
> @InstallIn(SingletonComponent::class)
> object FooModule {
>   // @Singleton providers are only called once per SingletonComponent instance.
>   @Provides
>   @Singleton
>   fun provideBar(): Bar {...}
> }
> ```
>
> 각 component는 기본적으로 사용할 수 있는 binding을 가진다.
>
> ```kotlin
> @Module
> @InstallIn(SingletonComponent::class)
> object FooModule {
>   // @InstallIn(SingletonComponent.class) module providers have access to
>   // the Application binding.
>   @Provides
>   fun provideBar(app: Application): Bar {...}
> }
> ```
>
> module은 다수의 component에 설치될 수 있다. 예를 들어 `ViewComponent`와 `ViewWithFragmentComponent`에 대한 binding이 있는데, module을 복제하기 싫다면, `@InstallIn({ViewModelComponent.class, ViewWithFragmentComponent.class})`로 두 component에 동시에 module을 설치할 수 있다.
> 다수의 component에 module을 설치할 때에는 규칙이 있다.
>
> * component들이 동일한 scope 어노테이션을 지원할 때만 provider의 범위를 지정할 수 있다.
> * provider는 component들이 해당 binding에 접근할 수 있을 때만 binding을 주입할 수 있다.
> * 자식 component와 조상 component는 같은 module을 설치할 수 없다.(조상 component에만 module을 설치해도 자식 component가 binding에 접근할 수 있다.)

***

JVM(Java Virtual Machine)

> JVM은 자바 바이트 코드를 실행할 수 있는 주체이다. 자바 바이트코드는 플랫폼에 독립적이고, JVM에 의해 실행된다. 플랫폼과 관계없이 JVM만 동작한다면 다시 컴파일할 필요 없이 바이트 코드를 실행할 수 있다.
>
> ![img](https://miro.medium.com/max/1218/0*qwg6yiZ2Kud1DD9g.png)
>
> JVM의 특성
>
> * 스택 기반의 가상 머신이다.
> * 단일 상속 형태의 객체 지향 프로그래밍을 가상 머신 수준에서 구현한다.
> * 포인터를 지원하지만, 주소 값을 임의로 조작하는 포인터 연산은 지원하지 않는다.
> * Garbage Collection을 사용한다.
> * 기본 타입의 정의를 명확히하여 플랫폼 독립성을 보장한다.
> * 데이터 흐름 분석에 기반한 자바 바이트 코드 검증기를 이용하여 많은 문제를 실행 전에 검증하여 실행 시 부담을 줄여준다.

DVM(Dalvik Virtual Machine)

> Dalvik은 Android 시스템이 실행되는 가상 머신이다. 안드로이드에서는 자바의 .class 파일을 dx툴을 이용해서 .dex파일로 변환하고 Dalvik에서 이를 실행한다. 따라서 안드로이드에서는 JVM을 사용하지 않는다.
>
> DVM은 레지스터 기반 모델을 사용하여 스택 기반 모델을 사용하는 JVM보다 안드로이드에서 더 빠르게 동작이 가능하다. Dalvik은 메모리가 작은 모바일 환경에서 JVM보다 최적화 되어 있고, 단일 인스턴스를 사용하는 JVM과 달리 다중 인스턴스를 실행할 수 있도록 설계되어있다.
>
> 안드로이드 2.2부터 JIT(Just In Time) 컴파일러가 도입되어 앱 실행 시 자바 코드의 한 부분을 한번에 변환하여 메모리에 올려 작업하게 되었다. 성능이 향상되었지만, 몇가지 문제점이 발생했다.
>
> * JIT가 동작중일 때 하드웨어의 부하가 커져, 배터리 소모량이 심각하게 늘어났다.
> * 애플리케이션의 실행 부분 전체를 메모리에 올려두어야 하기 때문에, 각 앱이 훨씬 많은 메모리를 사용하게 되었다.

ART(Android Runtime)

> 안드로이드의 실행 시간 환경으로 Dalvik의 JIT의 문제점을 해결하기위해 만들어져 AOT(Ahead Of Time) 컴파일러를 도입하였다. AOT는 애플리케이션 설치시 앱을 한 번에 컴파일해두고, 앱을 실행할 때에는 미리 컴파일 해둔 것을 읽어오는 방식이다. 앱을 실행할 때 컴파일할 필요가 없기 때문에 성능이 훨씬 좋아지고 배터리 수명이 향상되었다. AOT에도 단점은 있다.
>
> * 애플리케이션의 설치 공간이 좀 더 많이 필요하다.
> * 애플리케이션을 설치할 때 컴파일하기 때문에 설치 시간이 오래걸린다.
>
> 안드로이드 7.0부터는 AOT의 단점을 보완하기 위해서 애플리케이션의 최초 설치 시에는 JIT를 사용하여 설치 시간과 용량을 적게 소모하고, 차후 상황에 따라 JIT와 AOT를 유연하게 사용하도록 했다.

***

GC(Garbage Collection)

> GC는 메모리 관리 방법 중 하나로, 시스템에서 더이상 사용하지 않는 동적 할당된 메모리 블럭을 찾아 자동으로 다시 사용 가능한 자원으로 회수하는 방법이다. 시스템에서 GC를 수행하는 부분을 가비지 컬렉터라고 부른다.
>
> weak generational hypothesis
>
> * 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
> * 오래된 객체는 참조될 일이 드물다.
>
> GC의 동작 과정
>
> ![Alt JVMHeap](https://mirinae312.github.io/img/jvm_gc/JVMObjectLifecycle.png)
>
> JVM에서 Heap 메모리는 그림과 같이 나누어져 있다.
> 객체가 처음 생성되면 Young Generation인 Eden 영역에서 위치한다. Minor GC가 발생하면 다른 곳에서 참조되지 않는 객체는 메모리에서 제거되고, 살아남은 객체들은 Survivor 구역으로 이동한다. Survivor구역은 두 구역으로 나뉘는데 Minor GC가 일어날 때마다 살아남은 객체는 두 구역을 번갈아가며 이동한다. 객체가 두 구역을 오가는 횟수를 기록하여 이 횟수가 설정된 횟수보다 커지면 Old Generation 구역으로 이동한다. Old Generation 구역의 객체들은 Major GC가 일어날 때 사용되지 않는 객체들이 메모리에서 제거된다.

***

Memory leak

> 메모리 누수란 사용했던 메모리를 반환하지 않아 가용 메모리가 줄어드는 현상이다.
>
> GC를 사용하는 언어에서도 메모리 누수가 일어날 수 있다. 사용중인 객체가 사용되지 않는 객체를 참조하는 경우 사용되지 않는 객체는 GC로 인해 제거되지 않기 때문에 메모리 누수가 일어날 수 있다. 메모리 누수가 일어나게 되면 GC가 더욱 자주 일어나게되고 GC가 일어날 때에는 UI의 동작이 멈추기 때문에 사용자가 버벅거림을 느낄 수 있다. 또한, 메모리 누수로 인해 메모리가 부족하게 되면 Out Of Memory 상태가 되어 앱이 종료될 수도 있다.
> 안드로이드의 경우 Activity의 `onDestroy()`메소드가 완료되었지만 Activity가 강한 참조로 연결되어 있는 경우 Activity가 메모리에서 제거되지 않아 메모리 누수가 일어날 수 있다. 안드로이드에서 메모리 누수가 일어나는 예를 들면
>
> 1. Activity에서 정적 뷰를 사용한 경우 이 뷰는 Activity의 생명 주기를 따르지 않기 때문에 Activity가 종료되어도 Activity를 메모리에서 제거할 수 없게되어 메모리 누수가 일어난다.
> 2. Activity에서 inner class를 사용하고 정적 참조로 유지한다면 메모리 누수가 일어난다.
> 3. Activity에서 익명 클래스를 사용해도 익명 클래스가 Activity에 참조를 유지하기 때문에 메모리 누수가 일어난다.
> 4. ViewModel에서 Activity의 context를 참조하면 ViewModel은 Activity와 생명주기가 다르기 때문에 메모리 누수가 일어날 수 있다.
>
> LeakCanary 라이브러리를 사용하면 메모리 누수를 검출할 수 있다. 이 라이브러리는 Activity가 종료되어도, 회수되지 않은 메모리가 검출되면  분석해서 결과를 리포트해준다.
