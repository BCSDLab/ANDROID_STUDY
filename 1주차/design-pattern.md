# Android Design Architecture Pattern (MVC, MVP, MVVM)

## Design Pattern

프로그래밍할 때 마주칠 수 있는 다양한 문제에 대한 재사용 가능한 해결책 -> 문제를 해결하기 위한 최선의 방법을 공식화

### Categories

- **Creational patterns**: Object를 생성하는 방법에 대해 다룸
  - Builder, Dependency Injection, Singleton, Factory
- **Structural patterns**: Object를 구성하는 방법에 대해 다룸
  - Adapter, Facade, Decorator, Composite
- **Behavioral patterns**: Object 간 상호작용에 대해 다룸
  - Command, Observer, Strategy, State

### App Architecture

느슨하게 결합된 코드 베이스를 구조화하는 데 중요한 역할을 한다. App architecture는 다음과 같은 이점을 가져다 준다.

- 테스트 용이
- 확장성
- 코드 관심사 분리

또한 아래와 같은 전반적인 코드 작성 규칙을 나타내기도 한다.

- 클래스의 역할
- 폴더 구조
- 코드 구조(Network calls, Responses, Errors)

#### Android App Architecture

Android에서는 크게 3가지의 대표적인 Architecture Design Pattern이 있다.

- MVC (Model + View + Controller) 
- MVP (Model + View + Presenter)
- MVVM (Model + View + ViewModel)

### MVC (Model + View + Controller) 

<img width="883" alt="스크린샷 2022-05-09 오후 10 21 49" src="https://user-images.githubusercontent.com/23609119/167419149-a7f7b57f-64b0-4e34-b0b5-8ee283f25d16.png">

- **Model**: Data + State + Business logic, Application의 데이터와 상태, 비즈니스 로직을 포함하고 있다. View와 Controller에 종속되지 않으며 이런 이유로 여러 곳에서 재사용이 가능하다.
- **View**: Model을 표시하는 방법(Representation) 이다. UI를 렌더링하며 사용자와 애플리케이션의 상호작용이 있을 때 Controller와 통신한다.
  View는 멍청하다(Model의 상태를 이해하지 못하고, 사용자와의 상호작용을 어떻게 처리해야 하는지 모른다). 이렇게 하는 이유는 Model과의 결합도를 낮춰 코드 변화에 더 유연하게 대처할 수 있다.
- **Controller**: Model과 View를 이어주는 다리 역할을 한다. 사용자와 View의 상호작용(버튼을 누르는 등)을 Controller에 알리면 Controller는 Model과 적절히 상호작용한다. 또한 Model의 상태 변화에 따라 Controller는 View를 적절히 업데이트한다.
  Android에서는 Controller가 주로 Activity, Fragment로 표현된다.

#### 특징

- Model과 View의 분리: Model에서 View를 분리해냄으로써 Model의 종속성을 제거하고 테스트를 용이하게 한다.

#### 문제점

MVC의 Controller는 몇 가지 문제점이 존재한다.

- Controller가 Android API에 종속: Unit Test가 어려워짐
- Controller가 View와 강하게 결합: View가 변경될 때 Controller도 같이 변경해야 할 가능성이 높음
- 많은 코드가 Controller에 모임: 유지보수성이 낮아짐

### MVP (Model + View + Presenter)

<img width="938" alt="스크린샷 2022-05-09 오후 10 22 12" src="https://user-images.githubusercontent.com/23609119/167419210-3634a5f4-653b-479f-8610-adde9da00dbc.png">

- **Model**: MVC의 Model과 동일 (Data + State + Business logic)
- **View**: MVC와 비슷하지만 Android에서는 MVC와 달리 Activity/Fragment가 View의 일부로 간주된다. 다만 Android API와 의존성을 낮추기 위해 View interface를 만들고 Activity/Fragment가 이를 구현하도록 하는 것이 좋다.
- **Presenter**: MVC의 Controller와 역할이 비슷하지만 Controller가 View에 연결되는 것에 비해 단순한 Interface로 표현한다. 따라서 Presenter는 Android API의 의존성을 제거할 수 있다.

#### MVC와의 차이(Android)

MVC Controller 중 View의 상태를 변경하는 부분을 MVP에서는 View의 일부로 간주한다. 또한 Presenter의 Android API 의존성을 없애기 위해 별도의 View Interface를 만들어 관리한다.

#### 특징

- Presenter가 Android API와의 의존성을 가지지 않기 때문에 Unit test가 쉬워진다.

#### 문제점

- Controller와 마찬가지로 Presenter 또한 프로젝트가 커짐에 따라 코드가 방대해지는 경향이 있다.

### MVVM (Model + View + ViewModel)

<img width="939" alt="스크린샷 2022-05-09 오후 10 22 26" src="https://user-images.githubusercontent.com/23609119/167419235-62e5b212-6a27-4d93-819f-c6b31c68f390.png">

- **Model**: MVC 및 MVP의 Model과 동일 (Data + State + Business logic)
- **View**: 애플리케이션의 Presentation을 담당한다. 애니메이션, 버튼 처리 등의 UI 로직을 포함하되 비즈니스 로직은 포함하지 않는다.
- **ViewModel**: View가 사용할 Method를 구현하고, View에게 상태 변화를 알린다(View는 ViewModel의 상태 변화를 옵저빙한다)

#### View와 ViewModel의 관계

일반적으로 1:N 관계를 형성한다. View에서 서로 다른 두 모델을 활용한 데이터가 필요할 경우 View에서 Model의 값을 조작하지 않고 ViewModel에서 Model의 값을 가공한 뒤 View로 넘긴다.

#### MVVM ViewModel vs AAC ViewModel

먼저 MVVM의 ViewModel과 AAC(Android Architecture Components) 의 ViewModel은 관련이 없다. 둘의 역할은 개념적으로 차이가 있다.

- **MVVM ViewModel**: View에 연결될 Data와 Method를 구현하고 상태 변화를 이벤트를 통해 View에게 알린다(Observer pattern).

- **AAC ViewModel**: Android의 Lifecycle을 고려하여 UI 관련 데이터를 저장하고 관리한다.

  ![img](https://blog.kakaocdn.net/dn/VC28E/btrgWEVdrSo/lKNSDgXb625gBEZXLkNkPK/img.png)
  
  AAC ViewModel의 생명주기는 위의 그림과 같으며 Activity의 생명주기 동안 AAC ViewModel은 유지되기 때문에 Activity의 화면 회전과 같은 상황에서도 데이터를 보존할 수 있다.

두 개념은 명백히 다르다(Android에서 AAC ViewModel을 사용하지 않고 MVVM 패턴을 구현할 수 있으며 AAC ViewModel을 사용한다고 MVVM 패턴을 구현하는 것이 아니다). **그러나 AAC ViewModel을 이용하여 MVVM의 ViewModel을 구현할 수 있다**(AAC ViewModel에서 ObservableField, LiveData를 사용하여 구현). 

#### MVVM + Data binding in Android

- **Model**: MVC, MVP와 동일 (Data + State + Business logic)
- **View**: Application의 Presentation을 담당한다. Data binding을 사용하면 View에 표시할 데이터, View에서 할 Action 등을 ViewModel과 연결하는 코드를 줄일 수 있다.
- **ViewModel**: AAC ViewModel + Observable data(LiveData)

#### 특징

- View의 의존성 제거: Unit Test 용이

#### 문제점

- 초기 세팅이 복잡 -> 단순한 프로젝트에서는 오히려 부적절할 수 있음, 단 프로젝트의 확장 가능성 등을 고려하여 적용을 검토

## Links

https://jroomstudio.tistory.com/20

https://www.raywenderlich.com/18409174-common-design-patterns-and-app-architectures-for-android

https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/

https://academy.realm.io/kr/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/

https://velog.io/@k7120792/Model-View-ViewModel-Pattern

https://leveloper.tistory.com/216
