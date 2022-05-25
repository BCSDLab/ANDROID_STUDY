# ANDROID_STUDY 1주차
## 1. Clean Architecture
### :question: Why we use Clean Architecture
> #### 1. Independent of Framework
> 아키텍처는 기능이 포함된 소프트웨어의 일부 라이브러리의 존재에 의존하지 않는다.
> 이것은 시스템을 제한된 제약조건에 집어 몰아 넣는 것 대신 이러한 프레임워크를 도구로써 사용할 수 있다. 
>
> #### 2. Testable
> 비지니스 규칙을 UI, Database, Web서버 또는 그외 외부적인 요소 없이 테스트 할 수 있다.
>
> #### 3. Independent of UI
> UI는 시스템의 나머지 부분을 변경하지 않고 쉽게 바뀔 수 있다.
>
> #### 4. Independent of Database
> 데이터베이스를 어떤 것으로 바꾸던지 간에, 비지니스 규칙은 데이터베이스에 영향을 받지 않는다
>
> #### 5. independent of any external agency
> 실질적으로, 비지니스 규칙은 바깥 세상에 대해서 전혀 모른다.

<br>

![Image](https://blog.coderifleman.com/images/the-clean-architecture/the-clean-architecture.jpg)

동심원들은 소프트웨어의 다른 분야를 표현한다.
일반적으로 바깥쪽으로 올라갈수록 소프트웨어의 수준이 높아진다.

<br>

### :exclamation: Dependency Rule
> - 위의 아키텍처가 작동하도록 하는 최우선의 규칙이다.
> - 소스코드의 종속성이 다이어그램에서 바깥쪽 원이 안쪽 원만을 가리킬 수 있다.
> - 안쪽의 원은 외부에 있는 원에 대해 알고 있으면 안된다.

<br>

### :one: Entity
> - Entity들은 가장 일반적이고 높은 수준의 규칙을 캡슐화한 것이다. 
> - Entity는 응용 프로그램의 비즈니스 개체이다. 
> - 외부에서 무언가가 변경될 때 변경될 가능성이 가장 적다. 
> - 특정 애플리케이션에 대한 운영상의 변경은 Entity 계층에 영향을 미쳐서는 안 된다.

<br>

### :two: Use Cases
> - 시스템의 모든 Use Cases를 캡슐화하고 구현한다.
> - Entity가 Use Cases의 목표를 달성하기 위해 전반적인 비즈니스 규칙을 사용하도록 한다.
> - 레이어의 변경 사항이 Entity에 영향을 미치지 않으며 외부의 영향을 받지 않는다.
> - Use Case의 세부 사항이 변경되면 이 계층의 일부 코드가 확실히 영향을 받는다.

<br>

### :three: Interface Adapter
> - 이 계층의 소프트웨어는 어댑터의 집합이다.
> - Presenter, View 및 Controller는 모두 여기에 속한다.
> - Entity나 Use Cases에 용이한 형에서, 사용하고 있는 Framework에 용이한 형으로 변환된다. 
> - 외부(외부 서비스) 형식에서 내부(Use Case, Entity) 형식으로 데이터를 변환하기 위해 필요한 기타 모든 어댑터도 둘 수 있다.

<br>

### :four: Framework and Drivers
> - 가장 바깥쪽 레이어는 일반적으로 데이터베이스, 웹 프레임워크 등과 같은 프레임워크와 도구로 구성된다.
> - 이 레이어에는 안쪽 원과 통신하는 글루 코드 외에는 많은 코드를 작성하지 않음
> - 이 레이어는 모든 세부 사항이 들어가는 곳이다.
>
> __* glue code(접착코드)__ : 본래 호환성이 없는 부분끼리 결합하기 위해 작동하는 코드

<br>

### :question: 꼭 4개의 계층으로 나누어야 하는가
> - 정확히 4가지가 아니면 안된다는 규칙은 없다.
> - 하지만 의존 규칙은 항상 적용된다.
> - 의존성은 항상 안쪽으로 향해야 한다.

<br>

### :construction: Crossing Boundary Data
> - 경계를 넘나드는 데이터는 단순한 구조의 데이터다.
> - 기본적인 구조체 또는 단순한 데이터 전송 객체(DTO)를 취향에 맞게 사용 가능
> - Entity나 데이터베이스의 행을 전달하지 않는다.
> - 경계를 넘어 데이터를 전달할 때엔 항상 안쪽의 원이 다루기 쉬운 데이터 형식이어야 한다.

<br>

## 2. Design Pattern
### 1. MVC
![image](https://www.duplicatetransaction.com/wp-content/uploads/2020/06/model-view-controller-mvc-pattern.png)
> #### 1. Model
>   - 어플리케이션이 “무엇”을 할 것인지를 정의 함
>   - 데이터 및 비지니스 로직을 처리하는 역할
> #### 2. View
>   - 화면에 “무엇” 인가를 “보여주기 위한 역할”을 함
>   - 사용자에게 보일 화면을 표현
> #### 3. Controller
>   - 모델이 “어떻게” 처리할 지를 알려주는 역할을 함
>   - 화면에서 사용자에게 요청을 받은 내용을 처리하여 Model과 View에 업데이트 요청
>   - 사용자로부터 입력을 받고 Model 또는 View의 중개인 역할
>   - Android에서는 Activity와 / Fragment 같은 View가 Controller를 갖고 있다.

<br>

#### 장점
> 1. 구현이 간단하다.

<br>

#### 단점
> 1. Controller의 코드 양 증가
> 2. 스파게티 코드 가능성 증가
> 3. 유지 보수 어려움
> 4. View와 Model의 결합도가 높다
> 5. 테스트 코드 작성 힘듦

<br>

### 2. MVP
![image](https://innovationm.co/wp-content/uploads/2018/07/MVP.png)
> #### 1. Model
>   - Data와 관련된 처리(비지니스 로직)를 담당
>   - 네트워크, 로컬 데이터 등을 포함하는 Data의 전반적인 부분을 Model에서 담당한다.
> #### 2. View
>   - 사용자의 실질적인 이벤트가 발생하고, 이를 처리 담당자인 Presenter로 전달
>   - 사용자에게 보여주는 역할만 설계, 계산 및 데이터를 가져 오는 등의 행위는 Presenter에서 처리
> #### 3. Presenter
>   - View에서 전달받은 이벤트를 처리하고, 이를 다시 View에 전달
>   - View와 Model의 인스턴스를 갖고 있어 둘을 연결하는 역할

<br>

#### 장점
> 1. View와 Model이 의존성이 없다.
> 2. MVC 패턴이 갖고 있던 View와 Modeld의 의존성을 해결하였다.

<br>

#### 단점
> 1. View와 Presenter 사이의 의존성이 높다.
> 2. 어플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해진다.

<br>

### 3. MVVM
![image](https://user-images.githubusercontent.com/1812129/68319232-446cf900-00be-11ea-92cf-cad817b2af2c.png)
> #### 1. Model
>   - ViewModel 이 요청한 데이터를 반환함
>   - Room, Realm 과 같은 DB 사용 및 Retrofit 을 통한 백엔드 API 호출(네트워킹)이 보편적 
> #### 2. View
>   - Activity / Fragment 가 View의 역할을 함
>   - 사용자의 Action을 받음 (텍스트 입력, 버튼 터치)
>   - ViewModel 의 데이터를 관찰하여 UI 갱신
> #### 3. ViewModel
>   - View가 요청한 데이터를 Model로 요청함
>   - Model로부터 요청한 데이터를 받음

<br>

#### 장점
> 1. View가 ViewModel의 Data를 관찰하고 있음으로 UI 업데이트가 간편
> 2. ViewModel 이 데이터를 홀드하고 있으므로 Memory Leak 발생 가능성 배제
>   (View 가 직접 Model에 접근하지 않아 Activity / Fragment 라이플 사이클에 의존하지 않기 때문)
> 3. 기능별 모듈화가 잘 되어 유지보수에 용이 (ViewModel 재사용 및 DB 교체 작업 용이)

<br>

#### 단점
> 1. ViewModel 설계가 쉽지 않음

<br>
   
참고 자료   
[Clean Architecture reference](https://blog.coderifleman.com/2017/12/18/the-clean-architecture/)    
[MVC reference](https://thdev.tech/androiddev/2016/10/23/Android-MVC-Architecture/)   
[MVP reference](https://thdev.tech/androiddev/2016/10/12/Android-MVP-Intro/)   
[MVVM reference](https://velog.io/@haero_kim/Android-%EA%B9%94%EC%8C%88%ED%95%98%EA%B2%8C-MVVM-%ED%8C%A8%ED%84%B4%EA%B3%BC-AAC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
