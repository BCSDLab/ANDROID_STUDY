# Clean Architecture

## 시스템 아키텍처란?

[위키피디아](https://ko.wikipedia.org/wiki/시스템_아키텍처)에 찾아보면 아래와 같은 정의를 하고 있다.

> 시스템 아키텍처(system Architecture)는 시스템의 구조, 행위, 더 많은 뷰를 정의하는 개념적 모형이다. 시스템 목적을 달성하기 위해 시스템의 각 컴포넌트가 무엇이며 어떻게 상호작용하는지, 정보가 어떻게 교환되는지를 설명한다.

## 클린 아키텍처란?

`Rober C Martin` [언클 밥](http://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)은 대부분의 아키텍처는 아래와 같은 규칙을 지켜야 한다고 한다.

- 프레임워크에 독립적이고 아키텍처는 기능이 많은 라이브러리에 의존하면 안된다.
- 비즈니스 규칙은 UI, 데이터베이스, 웹 서버 또는 다른 외부 요소 없이 테스트할 수 있다.
- UI와 독립적이여 한다. UI는 변경이 잦기 때문이다.
- Databased와 독립적이어야 한다. Oracle 또는 SQL Sever 등 변경이 될 수 있으므로 비즈니스 로직과 독립적이어야한다.
- 비즈니스 규칙은 외부 서비스에 대해서 알지 못한다.

여기서 공통적인 목표는 계층을 분리해서 관심사를 분리하는 것이며, 이런 아키텍처가 동작하기 위해서는 의존성 규칙을 지켜야 한다.

외부 Interface(Framework, Driver) ->  Controller(Interface Adaptter) -> Use Case(App 고유의 데이터, 로직) -> Entity(App에 의존하지 않는 데이터, 로직) 으로 의존성을 가지게 된다.

## 엔티티란?

- 엔티티는 시스템 내부 객체로서, 핵심 업무 데이터를 기반으로 동작하는 일련의 조금만한 핵심 업무 규칙을 구체화 한것이다.
- 엔티티의 인터페이스는 핵심 업무 데이터를 기반으로 동작하는 핵심 업무 규칙을 구현한 함수들로 구성.
- 핵심적인 개념을 구현하는 소프트웨어는 한데 모아 자동화 시스템의 나머지 모든 고려사항과 분리 시킨다.
  - 엔티티는 DB, UI, 서드파티 프레임워크에 대한 고려사항들로 인해 오염되서는 절대 안된다.
  - 업무의 대표자로 독립적으로 존재해야한다.

## 유즈 케이스란?

- 내가 만들고자 하는 시스템(혹은 서비스)을 사용하는 클라이언트가 그 시스템을 통해 하고자 하는 것
- 자동화된 시스템이 사용되는 방법을 설명.
- UI와 유스케이스는 무관하다.
- 엔티티(고수준)는 유스케이스(저수준)에 대해서 아무것도 알지 못한다. 유즈케이스는 엔티티에 대해서 알고 있다.
- 유즈케이스 객체의 구성요소
  - 애플리케이션에 특화된 업무 규칙을 구현하는 하나 이상의 함수이다.
  - 입력 데이터, 출력 데이터가 있다.
  - 유스케이스가 상호작용하는 엔티티에 대한 참조 데이터를 가지고 있다.

## 엔티티와 유즈 케이스 예시

**'영화 예매'** 라는 Use Case가 있을때 해당 로직을 처리하기 위한 구조인 '영화', '상영', '손님', '비용'이라는 객체로 구성할 수 있다. 이러한 객체들의 협력으로 만들 수 있다. 이때 '영화', '상영', '손님', '비용'과 같은 것들이 Entity라 한다.

# Architecture pattern (MVC, MVP, MVVM)

MVC, MVP, MVVM 등 다양한 패턴이 있는데 공통적인 특징과 장점들이 있다. 화면에 보여주는 로직과 실제 데이터가 처리 되는 로직을 분리한다는 점이다. 분리시킴으로서 코드의 재활용성을 높이고 불필요한 중복을 막을 수 있으며, 테스트 코드를 작성하는데 용이하다.

MVC, MVP, MVVM 패턴을 보면 M(Model)과 V(View)를 포함하고 있다.
- `M`odel : 데이터 또는 데이터를 생성하거나 업데이트한다.
- `V`iew : UI 또는 화면을 표시한다.

프로그램의 Presentation Logic과 Business Logic들을 구현함에 있어 데이터와 UI는 필수이기 때문에, M-V 사이의 의존성이 당연히 생길 수밖에 없다. 이러한 문제를 해결하기 위해 여러 패턴들이 있으며, M-V 사이의 관계를 어떻게 처리하느냐에 따라서 패턴을 구분 지을 수 가 있다.

## MVC (Model-View-Controller)

### 구조

- Model: 어플리케이션에서 사용되는 데이터와 데이터를 처리하는 부분이다.
- View: 사용자에게 보여지는 UI 부분이다.
- Controller: 사용자으 입력(Action)을 받고 처리하는 부분이다.

### 동작

1. 사용자의 Action들이 Controller에 들어오게된다.
2. Controller는 사용자의 Action를 확인하고, Model을 업데이트한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

### 장점

MVC 패턴은 널리 사용되고 있는 패턴이며, 단순하다 보니 보편적으로 많이 쓰인다.

### 단점

View와 Model 사이의 의존성이 높다. View와 Model의 높은 의존성은 어플리케이션이 커질수록 복잡해지고 유지보수가 어렵다.

## MVP (Model-View_Presenter)

- Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분이다.
- View: 사용자에서 보여지는 UI 부분이다.
- Presenter: View에서 요청한 정보로 Model을 가공하여 View에 전달해 주는 부분이다.

### 동작

1. 사용자의 Action들은 View를 통해서 들어오게 된다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

### 장점

MVP 패턴의 장점은 View와 Model의 의존성이 없다는 것이다. MVP 패턴은 MVC 패턴의 단점이었던 View와 Model의 의존성을 해결하였다.

### 단점

View와 Model 간의 의존성을 해결되었지만, View와 Presenter 사이의 의존성이 높아지는 단점이 있다.

## MVVM (Model-View-ViewModel)

ViewModel 과 View는 1:n 관계이다.

### 구조

- Model : 어플리케이션서 사용되는 데이터와 그 데이터를 처리하는 부분이다.
- View: 사용자에서 보여주는 UI 부분이다.
- ViewMode: View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내 주기 위한 Model이자 View를 나타내기 위한 데이터 처리를 하는 부분이다.

### 동작

1. 사용자의 Action들은 View를 통해 들어오게 된다
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다.
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 View Model에게 요청 받은 데이터를 응답한다.
5. View Model은 응답 받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding 하여 화면을 나타낸다.

### 장점

Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앴다.

### 단점

ViewModel의 설계가 쉽지 않다.

### MVVM에서 ViewModel과 AAC-ViewModel 차이

구글에서는 AAC-ViewModel을 MVVM 패턴을 의식해서 만든 것이 아니다. AAC ViewModel인 경우는 Android Lifecycle을 고려하여 UI 관련 데이터를 저장하고 관리하는 요소로 사용하며 일반 적인 ViewModel은 View에 필요한 데이터를 관리하여 바인딩 해주며, 비즈니스 로직을 담당해 데이터를 처리하는 요소라고 생각하면된다. AAC-ViewModel을 사용하지 않고도 MVVM패턴을 구현할 수 있으며, AAC-ViewModel을 사용했다고 해서 MVVM 패턴이 되지는 않는다.

AAC의 ViewModel로 MVVM 패턴의 ViewModel을 구현할 수 있다. MVVM 패턴의 ViewModel의 역할은 View에 필요한 데이터를 관리하여 바인딩해주는 것이므로, AAC-ViewModel을 사용하여 앞서 설명한 개념대로 구현해주면 된다. ViewModel 내에서 View를 참조 하지 않으며, ObservableField나 LiveData등을 사용하여 데이터 바인딩을 해준다면 MVVM 패턴의 ViewModel로서 사용할 수 있다.
