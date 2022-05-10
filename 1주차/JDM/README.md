# 클린아키텍처


## 클린아키텍처를 하려는 이유
 - UI, 데이터베이스, 프레임워크 등의 독립성을 만들기 위해
 - 기능 변경 및 확장의 용이성 -> 유지보수 쉽게 하기 위해
 - 테스트를 용이하게 하기 위해
## 클린아키텍쳐란?
 - 계층별로 관심사를 분리하고 비즈니스 로직을 캡슐화하는 소프트웨어 설계 철학
## 계층별, 관심사?
[![N](https://www.charlezz.com/wordpress/wp-content/uploads/2021/09/www.charlezz.com-mvvm-clean-architecture-diagram.png)]
 - Presentation
    - UI에만 관심
 - Domain
    - UseCase(비즈니스 로직) 에만 관심
    - 다른 두 계층에 대해 의존하지 않는 계층
 - Data
    - 데이타를 가져오거나 저장하는데만 관심

 UI, 데이터베이스, 프레임워크 등의 독립성을 만들기 위해 -> 관심사를 분리했기에 독립성 있게 구현
 기능 변경 및 확장의 용이성 -> 각 계층이 관심있는 부분을 정했기에 개발자가 원하는 기능을 구현할 때 관심있는 부분만 가져와 조립할 수 있다.
 테스트를 용이하게 하기 위해 -> 테스트가 필요한(관심있는)부분만 가져와 조립하여 테스트를 할 수 있다.
 

## Architecture pattern (MVC, MVP, MVVM)
### MVC 
MVC는 Model 과 View, Controller 구조를 가지는 디자인 패턴입니다. 
- Model
    - 데이터와 상태, 비즈니스 로직을 구현
    - View 와 Controller 에 종속되지 않고 여러 곳에서 재사용 될 수 있다. 
- View
    - 화면을 구현하는 컴포넌트입니다. 
    - Model 에 대한 이해없이 오로지 사용자의 인터렉션을 받아 반응한다. 
    - View 와 Model 은 최대한 종속되지 않고 독립적이게 구현해야 한다. 
- Controller
    - View 와 Model 을 연결하는 역할
    - View 에서 사용자와의 인터렉션이 발생하면 컨트롤러가 이것을 인지하고 모델을 갱신하는 역할
안드로이드에서 Controller 는 주로 액티비티에 해당하고 View 는 xml 파일이 되며 Model 은 데이터 클래스 등이 될 수 있다.안드로이드에서 View 와 Controller 가 액티비티에 같이 구성되어 있어 사실상 하나의 모습을 보인다.
### MVP

MVP는 Model, View, Presenter 구조를 가지는 디자인 패턴입니다. 
- view
    - MVP 에서는 액티비티를 View 의 일부로 간주하여, 액티비티에 View 인터페이스를 구현한다
- presenter 
    - Presenter 는 MVC 의 Controller 와 역할이 거의 같지만 View 와 연결되어 있지 않다. 
    - View 와 Model 을 완전히 분리시켜줌으로써 서로의 독립성을 보장한다는 점이 장점
- Model
    - 데이터와 상태, 비즈니스 로직을 구현
    - View 와 Presenter 에 종속되지 않고 여러 곳에서 재사용 될 수 있다.
### MVVM

MVVM은 Model, View ViewModel 구조를 가지는 디자인 패턴입니다.
MVVM 은 DataBinding 을 이용함으로써 모듈화를 쉽게하고 뷰와 모델을 연결하는 코드를 줄일 수 있는 장점을 가진 디자인 패턴
- View
    - view가 ViewModel 에 의해 바인딩되어 ViewModel 의 값이 변경되면 View 가 갱신되는 구조
- ViewModel
    - Model 을 Wrapping 하는 구조이며 동시에 View 가 Model 에 이벤트를 전달할 수 있는 훅 기능도 한다.
- Model
    - 데이터와 상태, 비즈니스 로직을 구현
    - View 와 Presenter 에 종속되지 않고 여러 곳에서 재사용 될 수 있다.