# Clean Architecture

소프트웨어를 계층화해 관심사를 분리하고 테스트를 용이하게 한다. 

![The Clean Architecture의 다이어그램](https://blog.coderifleman.com/images/the-clean-architecture/the-clean-architecture.jpg)

## 의존 규칙

위 그림에서 각 계층은 안쪽을 향해서만 의존할 수 있다(안쪽 계층은 바깥쪽 계층을 알지 못한다). 이는 바깥쪽 계층의 이름을 안쪽 계층에서는 참조해서는 안된다는 의미이기도 하다.

또한 바깥쪽 계층에서 사용하고 있는 데이터 포맷을 안쪽 계층에서 사용하지 말아야 한다.

가장 바깥쪽 계층은 저수준의 구체적이고 상세한 정보를 담고 있으며 안쪽 계층으로 갈 수록 추상화 수준이 올라간다.

## 구조

### Entities

대규모 프로젝트 레벨의 비즈니스 규칙을 캡슐화, 하나의 애플리케이션을 작성할 경우에는 entity는 그 애플리케이션의 비즈니스 객체가 된다.

### Use Cases

애플리케이션의 고유 비즈니스 규칙을 포함하고 시스템의 모든 use case를 캡슐화하고 구현한다. 이 계층의 변경은 entity에 영향을 주지 않음을 기대하고 framework의 변경에 영향을 받지 않음을 기대한다.

### Interface Adapters

어댑터(usecase와 entity로부터 용이한 형식으로 DB, Web 등 외부 기능에 용이한 형식으로 변환)의 집합. 이 계층은 MVC, MVP, MVVM Design pattern architecture 를 모두 내포한다.

이 계층에서 데이터는 Entity나 Use case에 용이한 형태에서 사용하고 있는 프레임워크에 용이한 형태로 변환된다. ex) 이 계층보다 안쪽의 계층은 데이터베이스에 관해 아는 것이 없어야 한다.

### Frameworks and Drivers

DB나 Web Framework 등 일반적으로 프레임워크와 도구 등으로 구성된다. 안쪽 계층과 통신할 연결 코드 이외에는 별다른 코드를 작성하지 않는다.



이 4가지 규칙은 컨셉을 전달하기 위한 수단일 뿐 필수로 지켜야 하는 규칙은 아니다. 그러나 의존 규칙은 항상 적용된다. 소스 코드의 의존성은 항상 안쪽 계층을 향해야 한다.

## 교차 경계(Crossing boundaries)

위 그림의 오른쪽 아래를 보면 Controller -> Use case -> Presenter 의 제어의 흐름을 볼 수 있다. 그러나 소스 코드의 의존성은 항상 안쪽 계층을 향하고 있다.

이런 모습은 의존 관계 역전의 원칙 (Dependency Inversion Principle) 로 해결하는 경우가 많다. Java와 같은 언어는 인터페이스와 상속 관계를 이용해 소스 코드의 의존성과 제어의 흐름을 역전할 수 있다.

구체적으로 설명하자면 Use case는 Presenter를 직접 호출할 수가 없다(바깥쪽 계층의 이름을 안쪽 계층에서는 참조해서는 안된다). 이 문제를 해결하기 위해 Use case에서는 안쪽 계층의 인터페이스(그림의 Use case output port)를 호출하며 바깥 계층의 Presenter는 이 인터페이스를 구현하는 방법을 사용한다.

## Clean Architecture in Android

![img](https://blog.kakaocdn.net/dn/dtdUxw/btq4eKUibVC/tfFFiZPFfFzX8VSJnHY3LK/img.jpg)

![img](https://blog.kakaocdn.net/dn/cY2NoK/btq4hI2DCeg/rD2eaAc9teUV4bfNeH1OF0/img.jpg)

Android에 Clean Architecture를 적용할 때는 일반적으로 Presentation, Domain, Data의 3가지 계층으로 나눈다. 각 계층은 Presentation -> Domain, Data -> Domain 방향으로 의존성을 가지고 있다.

### Presentation Layer

Data를 화면에 표시하며 화면의 입력과 처리, 상호작용 등을 담당. Activity, Fragment, View 그리고 Presenter, ViewModel 등은 이 계층에 포함되어 있다.

### Domain Layer

Application의 Business logic을 담당한다. UseCase와 Model, Repository Interface는 이 계층에 포함되어 있다. 어떤 다른 계층에 의존성을 갖지 않는 독립적인 계층이며 안드로이드의 의존성을 갖지 않고 순수 Java, Kotlin 코드로만 구성한다(Kotlin Standard Library https://kotlinlang.org/api/latest/jvm/stdlib/ 나 일부 DI 라이브러리는 허용).

### Data Layer

Repository Implementation, Local and Remote Data Source를 포함하고 있으며 DB, Application의 데이터 관리를 담당한다. 또한 Data 계층의 Model을 Domain 계층의 Model로 변환하는 역할을 담당한다.

### 핵심

![img](https://blog.kakaocdn.net/dn/b3P9FR/btq4jYKC0Ev/t8RwN8XpTJ1GGWKLtEDHr1/img.png)

핵심은 Domain 계층이 다른 계층과 독립적이라는 것이다. 따라서 Domain 계층은 다른 외부 계층의 이름에 접근하지 않아야 한다.

### 실제 Android 프로젝트에 Clean Architecture 적용하기

#### 1. CA Layers in a single module

![img](https://miro.medium.com/max/1400/1*u1_5RzcpjsKqQrgGNbxmog.png)

Layer를 패키지화해 하나의 `app` 모듈에 추가한다.

![img](https://miro.medium.com/max/460/1*sSVHCPuupV6p-CDiqnYkgA.png)

이 방법에는 하나의 문제가 있는데, Layer간의 의존성을 확인할 수 없다는 것이다. 이는 곧 Domain layer에 Data 및 Presentation layer의 클래스를 적용하는 실수를 할 가능성이 높아진다.

한 모듈에 전체 코드가 들어가는 것은 소스 코드가 많아지는 것을 고려하면 바람직하지 않다.

#### 2. One CA layer per module

![img](https://miro.medium.com/max/1400/1*1NOM7f3QboTIHlsYBKfjaw@2x.png)

Layer 분리 및 위에서 언급한 실수를 줄이기 위해서 각 Layer마다 각각 모듈(Gradle subprojects)을 만든다.

![img](https://miro.medium.com/max/470/1*iKr-JayOLCtkjyHLrZUDnw.png)

Layer를 모듈로 분리하면서 은 layer 간 의존성을 체크할 수 있고 확장성을 고려한 설계를 적용할 수 있게 된다.

그러나 여전히 확장성 문제가 존재하는 데, 기능 구현 및 삭제에 여러 모듈을 건너야 한다. 이는 코드가 기능 단위로 결합될 수 있으며 이는 곧 추적 및 유지 관리가 어려울 수 있는 기능 간 종속성을 추가한다.

![img](https://miro.medium.com/max/698/1*nJVzrF3nxTmd3L5oHwJiIA.png)

각 계층에 기능별로 많은 코드가 추가될수록 코드 가독성이 저하되며 컴파일 시간이 늘어난다.

#### 3. CA layers inside the feature module

![img](https://miro.medium.com/max/1400/1*kLzPElQ8pf98m-iUKObQFw@2x.png)

위 문제를 해결하기 위해 기능 모듈 내에 각 기능별로 모듈을 만든 후 그 모듈에 모든 Layer를 포함시킬 수 있다. 각 모듈을 `app` 모듈이 연결한다.

![img](https://miro.medium.com/max/500/1*imbgXViirxQTe-NfqmtISg.png)

기능 단위의 적절한 코드 분리가 되었지만 이번에는 각각의 모듈의 CA Layer 간 의존성을 체크할 수 없는 문제점이 존재한다. 이는 Custom Lint Rules(https://github.com/googlesamples/android-custom-lint-rules) 적용 등으로 어느 정도 해결할 수 있다.

또한 공통 기능을 갖는 모듈(`BaseActivity`, `BaseFragment` 등)

#### 4. CA layers per feature modules

![img](https://miro.medium.com/max/1400/1*Z_2kEwwv3H38D0l9wUTjcQ.png)

기능별로 CA Layer 모듈을 따로 둔다. 기능 추가에 따른 모듈의 수가 급격히 증가해 의존성 지옥(Dependency hell) 에 빠질 위험이 높으며 Gradle이 느려질 수 있다고 한다.

# Links

https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html

https://blog.coderifleman.com/2017/12/18/the-clean-architecture/ (윗 글 번역본)

https://leveloper.tistory.com/205

https://ddangeun.tistory.com/138

https://proandroiddev.com/multiple-ways-of-defining-clean-architecture-layers-bbb70afa5d4a