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

Why do we use DI(Dependency Injection)
DI Library(Koin, Hilt, Dagger)
JVM, DVM, ART Difference
What is GC?
What is Memory leak?

# Dagger

# Hilt


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