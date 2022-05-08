# Clean Architecture

Clean Architecture는 앱에만 국한되지 않고 서비스를 이용하는 측면에서 계층을 분리하여 관심사를 분리할 수 있어야한다.

나의 코드에 적용하여 사용자의 요구사항을 충족하고 테스트, 유지보수를 용이하게 할 수 있다.

>로버트 마튼(Robert C.Martin)으로 부터 클린 코드의 저서로 Clean Architecture의 개념이 만들어졌다.
나중에 <u>**Clean Code(클린코드)**</u> 를 꼭 읽어봐야겠다.

👋 **클린 아키텍처는 아래 그림과 같이 총 4가지의 계층으로 구성되어있다.**

<img src="https://miro.medium.com/max/875/1*wOmAHDN_zKZJns9YDjtrMw.jpeg">

## 1. Entities
- 메서드를 갖는 객체일 수도 있지만 데이터 구조와 함수의 집합일 수도 있다.
- 외부가 변경되더라도 이러한 규칙이 변경될 가능성이 적다
## 2. Use cases
- 엔티티로부터의 데이터 흐름을 조합한다.
## 3. Interface Adapters (Presenters)
- 엔티티 및 유스케이스의 편리한 형식에서 데이터베이스 및 웹에 적용할 수 있는 형식으로 변환한다.
- 이 계층에는 MVP 패턴의 Presenter, MVVMM 패턴의 ViewModel이 포함된다.
- 즉, 순수한 비즈니스 로직만은 담당하는 역할이다.
## 4. Frameworks & Drivers (Web, DB)
- 웹 프레임워크, 데이터베이스, UI, HTTP client 등으로 구성된다.

이러한 계층을 유지하기 위해서 의존성 규칙을 준수할 수 있어야한다.<br>
의존성은 외부에서 내부로 갈수록 의존성이 낮아진다.<br>
Entity는 UseCase와 Interface Adapters, Frameworks & Drivers에 대해서 아무것도 모른다.<br>
예를 들어서, 안드로이드에서의 비지니스 로직을 담당하는 ViewModel(Presenters)는 DB & Web(Frameworks & Drivers) 와 같은 사항을 관여하지 않는다.

|의존성 ↓|의존성 ↑| 
|:--:|:--:|
|외부|내부|
|저수준|고수준|


# Clean Architecture In Android
안드로이드에서도 Clean Architecture를 접목하여 깨끗하고 고품질의 코드를 작성할 수 있다. 이는 사용자의 요구사항, 이해를 돕고 유연성, 테스트, 유지보수의 용이하다.

## Android에서의 Clean Architecture 계층
<img src="https://miro.medium.com/max/875/1*q2AL8a9a1ZN6m5OxgLJMvg.png">

## 1. APP (Presentation)
- 화면과 입력에 대한 처리 등 UI 부분
- Activity, Fragment, View, ViewModel
- APP(Presentation) 계층은 Domain 계층에 대한 의존성을 가지고 있다.
## 2. DATA
- Domain 계층의 의존성을 가지고 있다.
- Domain 계층의 Repository 구현체를 포함하고 있다.
- DB, 서버 통신
- Mappers 를 통해서 변환한다.(Data 계층의 모델 -> Domain 계층의 모델)
## 3. DOMAIN
- UseCases와 Model을 포함한다.
- APP(Presentation)과 DATA 계층에 대한 의존성을 가지지 않고 독립적으로 분리되어있다.
- Repository 인터페이스도 포함된다.

예를 들어서, Android에서 Realm에서 Room으로 데이터베이스를 바꾸어야 한다. 이를 Clean Architecture를 사용하면 효율적이다. DB는 Data에서 관리하기 때문에 Data에서만 바꿔주면 된다. Data는 Domain 계층의 Repository 구현체를 포함하고 있다. 이를 Realm -> Room으로만 수정해주면 APP(Presentation)과 Domain을 신경안써도 된다.

- 유스케이스 (Usecases)
  - 각 개별 기능 또는 비즈니스 로직 단위
  - 네트워크에서 데이터를 가져오거나 데이터베이스에서 데이터를 읽는 것과 같은 기본설정
  - 원격 또는 로컬 데이터 소스에서 데이터를 가져와서 요청자에게 다시 제공

- 저장소(Repository)
  - 도메인 모듈에서 생성
  - 구현은 데이터 모듈에서
  - Repository 인터페이스와 앱 모듈 사이의 중재자 역할

- 매퍼 (Mappers)
  - 서버 모델을 앱 모델에 매핑하는 것이 좋다
  - 앱 모델에는 다른 어떤것도 소비하지 않는 데 필요한 데이터만 포함되어있다.


# Code
코드 작성 전 Clean Architecture를 연습하기 위해서 app, data, domain의 세가지 모듈을 만들 수 있어야 한다. 이는 멀티 모듈을 사용할 수 있어야 한다. 멀티 모듈은 코드가 구조화되지 않고 흩어져있는 것을 방지하기 위해서 사용할 수 있다. 
오픈소스는 **[Reference #3]** 참고

## Module
**[Reference #2]** 멀티모듈 참고<br>
app : Phone & Tablet<br>
data : Android Library<br>
domain : Java or Kotlin Library<br>

접근방식은 상향식으로 domain ~ app으로 이동한다.

## Domain
도메인에서는 Model, UseCase, Repository로 구성된다.
### Model
  - 개체 또는 엔터티, POJO(Plain Old Java Object)클래스 만들기
    - POJO : 객체 지향적인 원리, 환경과 기술에 종속되지 않고 재활용될 수 있는 방식으로 설계 -> 애플리케이션의 핵심로직과 기능을 담아 설계
### Repository
  - get 함수를 만들고, 해당 함수는 RxJava.Single이 위 model 클래스를 제네릭으로 갖게 만들어 상속한다.
    - Single은 오직 1개의 데이터만 발행한다.
  - RxJava : build.gradle dependencies 추가
    - rxjava는 비동기 처리를 가능하게 만든다. 더 자세한 내용은 **Reference #5**를 참조
    - ```kotlin
      implementation 'io.reactivex.rxjava3:rxjava:3.0.6'
      ```
### UseCase
  - 데이터 소스에서 데이터를 가져오는 UseCase 생성
  - 중복코드를 제거하는 것이 목적
  - Interface로 쉽게 확장 가능
  ---
  - dagger 프레임워크 사용하여 저장소 클래스를 주입
    - dagger 프레임워크 관련 DI 자료는 **Reference #6** 참고
  - 데이터를 가져오기 위해서 repo 메서드를 호출

## Data
데이터에서는 Model, Mappers, API 서비스 및 Repository 구현으로 구성
### Model
  - 서버 응답에서 데이터를 읽는다.
### API Service
- interface ApiService() 생성
### Mappers
  - 서버 데이터를 앱에 필요한 데이터 모델에 매핑하는 매퍼 클래스
### Repository
  - 데이터 가져오기 처리하는 도메인 모듈의 레포지토리 인터페이스에 대한 레포지토리 구현
### DI (Dependency Injection)
- 두 개의 클래스로 구현.
- 하나는 Network, 나머지는 API 서비스 

## App
이곳에서는 DI, Rx, UI, ViewModel, Application 으로 구성
- dagger2 framework 를 사용

# Reference
[#1 클린 아키텍처 및 안드로이드 접목]<br>
https://leveloper.tistory.com/205


[#2 멀티 모듈로 안드로이드 프로젝트 생성]<br>
https://leveloper.tistory.com/201

[#3 클린 아키텍처 및 안드로이드 접목]<br>
https://medium.com/android-dev-hacks/detailed-guide-on-android-clean-architecture-9eab262a9011

[#4 POJO 란?]<br>
https://doing7.tistory.com/81

[#5 RxJava]<br>
https://blog.yena.io/studynote/2020/10/11/Android-RxJava(1).html<br>

[#6 DI (dagger2, koin, hilt)]<br>
https://velog.io/@sysout-achieve/Android-DI-Framework-%EC%84%A0%ED%83%9D%EC%A7%80Dagger2-Koin-Hilt


