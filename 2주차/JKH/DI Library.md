# Koin
Kotlin에서 의존성 주입을 위한 대표적인 라이브러리이다. Koin은 Kotlin DSL로 만들어졌다.
## 여기서 DSL이란?
- Domain-Specific-Languages 로 도메인 특화 언어이다. 이는 관련 특정 분야에 최적화된 프로그래밍 언어이다. 일반적인 Java, C 등의 언어보다 덜 복잡한다. 추가적으로 자세한 설명은 **Reference #3** 참조

Koin은 Dagger에 비해 **러닝 커브**가 낮다.(러닝 커브가 낮다는 것은 많이 배워도, 활용할 수 있는 부분이 적다는 것이다. 반대로 러닝 커브가 높다는 것은 조금 배워도, 활용할 수 있는 부분이 많다는 것이다.)

|--|Run Time|Compile Time|
|:--:|:--:|:--:|
|Koin|의존성 주입(영향 O)|영향 X|
|Dagger|영향 X|의존성 주입|

Koin은 런 타임에 오브젝트 그래프를 그려준다. 이는 앱 성능의 저하를 발생시킨다. 따라서 Koin은 큰 규모의 프로젝트보다는 작은 규모의 프로젝트에 적절하다고 한다.

## 모듈
Koin 모듈을 이용하여 클래스들을 Koin DSL로 생성할 수 있다.
- single : 한 번 생성하면 앱이 살아있는 동안 계속 사용 가능한 객체를 생성
- factory : Inject(주입)할 때마다 인스턴스 생성
- viewModel : viewModel 인스턴스를 생성
- bind : 지정된 컴포넌트의 타입을 추가적으로 바인딩
- get : 생성된 객체 중에 알맞은 객체를 주입
- applicationContext : Context 주입



# Dagger
Dagger는 DI(의존성 주입)을 도와주는 프레임워크이다.

## Dagger 필수 개념 5가지
## Inject
@Inject : Component로 부터 의존성 객체를 주입 요청하는 annotation
```kotlin
# 생성자 의존성 주입 방식
class A()
class B() @Inject constructor(val a:A)
```
```kotlin
# 필드 의존성 주입 방식
@Inject
lateinit var a:A

@Inject
lateinit var b:B
```
## Module
@Module : Component에 연결되어 의존성 객체를 생성하는 역할<br>
@Module 은 클래스에 붙임.<br>
@Provides 는 @Module 클래스의 메소드에만 붙인다. 
```kotlin
@Module
class ModuleA{
  @Provides
  fun provideA(): A = A()

  @Provides
  fun provideB(a: A): B = B(a)
}
```
## Component
@Component : Interface 또는 abstract class에서만 사용 가능.<br>
연결된 Module을 이용하여 의존성 객체 생성, Inject로 요청받은 인스턴스에 생성한 객체 전달(주입)한다.
## SubComponent
@SubComponet : interface 또는 abstract class 로 생성 - @Component와 동일
SubComponent는 부모 Component가 있는 자식 Component라고 할 수 있다.
## Scope
@Scope : Component 별 해당 annotation으로 주입되는 객체를 관리.<br>
생성된 객체의 LifeCycle 범위 내에서 관리

# Hilt
Hilt는 Google의 Dagger를 기반으로 만든 DI 라이브러리이다. Koin이 Kotlin에 특화된 심플한 DI라이브러리이면 안드로이드 11에서 공식적으로 Jetpack에 합류한 DI 라이브러리이다.
또한, Hilt는 기존 Dagger2의 **보일러 플레이트 코드를 제거**하기 위해서 안드로이드 내 구조에 맞춰 몇가지 Annotation을 추가한 라이브러리이다.

## 보일러 플레이트 코드란?
컴퓨터 프로그래밍에서 보일러 플레이트 또는 보일러 플레이트 코드라고 부르는 것은 최소한의 변경으로 여러곳에서 재사용되며, 반복적으로 비슷한 형태를 띄우는 코드를 말한다. 자세한 내용은 **[#3 Reference]** 참조

## Application
@HiltAndroidApp
```kotlin
@HiltAndroidApp
class ToDoApplication: Application()
```

## Activity, Fragment and ViewModel
@AndroidEntryPoint
```kotlin
@AndroidEntryPoint
class ToDoActivity: AppCompatActivity(){
  private val toDoViewModel: ToDoViewModel by viewModels()
}
```
## Module
Module은 기존 Dagger2와 동일하게 작성 후 @InstallIn(ApplicationComponent::class)를 통해 @Component를 따로 만들지 않고 컴포넌트 추가 가능.

# Reference

## Koin
[#1 Koin]<br>
https://insert-koin.io/

[#2 Koin이란?]<br>
https://kotlinworld.com/119#Koin%EC%-D%B-%EB%-E%--

[#3 DSL(Domain-Specific-Languages)란?]<br>
https://www.jetbrains.com/ko-kr/mps/concepts/domain-specific-languages/

[#4 러닝 커브란?]<br>
https://defineall.tistory.com/1029

[#5 컴파일 타임과 런 타임의 차이]<br>
https://ko.strephonsays.com/compile-time-and-vs-runtime-12092

----

## Dagger
[#1 Dagger 기본개념]<br>
https://jaejong.tistory.com/125


----
## Hilt
[#1 Hilt 정리]<br>
https://developer88.tistory.com/349

[#2 Hilt 란?]<br>
https://colinch4.github.io/2021-06-04/hilt/

[#3 보일러 플레이트 코드]<br>
https://charlezz.medium.com/%EB%B3%B4%EC%9D%BC%EB%9F%AC%ED%94%8C%EB%A0%88%EC%9D%B4%ED%8A%B8-%EC%BD%94%EB%93%9C%EB%9E%80-boilerplate-code-83009a8d3297