## 디자인 패턴과 아키텍처 차이

- 클린 아키텍처는 앱 전체를 UI와 비즈니스 로직을 분리하는 것이라면 디자인 패턴은 UI Layer에서 비즈니스 로직을 분리하는 것이다.

> **Presentation Logic**
실제 눈에 보이는 GUI 화면을 구성하는 코드를 뜻한다.
> 

> **Business Logic**
데이터를 보여주기 위해서 데이터베이스를 검색하는 코드 및 GUI 화면에서 새롭게 발생된 데이터를 데이터베이스에 저장하는 코드 등 실제적인 작업을 하는 코드를 뜻한다.
> 

## MVC (Model-View-Controller)

![image](https://user-images.githubusercontent.com/82709044/167557946-137afdbc-7ade-4c58-9756-4bb1a93917bc.png)

**동작**
1. 사용자의 Action들이 Controller에 들어온다.
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

> **MVC에서 View가 업데이트 되는 방법**
- View가 Model을 이용하여 직접 업데이트 하는 방법
- Model에서 View에게 Notify 하여 업데이트 하는 방법
- View가 **Polling*으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법
**Polling : 주기적으로 검사하여 일정한 조건을 만족할 때 송수신 등의 자료처리를 하는 방식*
> 

**장점**

- 가장 단순해서 구현하기 쉽다.

**단점**

- Activity가 Controller와 View 모두 처리하기 때문에 한 클래스에서 M-V-C 모두 처리하는 문제점이 발생한다.
- Model과 View 사이의 의존성이 발생해서 앱이 커지고 복잡해질수록 유지보수가 어렵다.

## MVP (Model-View-Presenter)

![image](https://user-images.githubusercontent.com/82709044/167557969-ace63d1b-6781-4b2a-98cc-9e9c7173809c.png)

**동작**

1. 사용자의 Action들은 View를 통해 들어오게 된다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

**장점**

- MVC의 단점을 보완한 Model과 View간의 의존성이 없는 패턴
- Presenter을 통해서만 데이터를 전달 받기 때문에 Model과 View간의 의존성이 없다.

**단점**

- Presenter가 M-V 사이에서 관리를 해줘서 M-V 사이의 의존성이 없지만 앱이 커지거나 복잡해질수록 V-P 간 의존성이 강해지는 문제점이 발생한다.

## MVVM

![image](https://user-images.githubusercontent.com/82709044/167557985-fd47accf-0444-4576-b99c-8296227de9e3.png)

**동작**

1. 사용자의 Action들은 View를 통해 들어온다.
2. View에 Action이 들어오면, Command 패턴으로 ViewModel에 Action을 전달한다.
3. ViewModel은 Model에게 데이터를 요청한다.
4. Model은 ViewModel에게 요청받은 데이터를 응답한다.
5. ViewModel은 응답 받은 데이터를 가공하여 저장한다.
6. ViewModel의 데이터가 변경되면, View는 DataBinding을 통해 화면에 데이터를 나타낸다.

**장점**

- View와 Model 사이의 의존성이 없다.
- **Command** 패턴이나 **Data Binding**을 이용해서 View-ViewModel간 의존성이 없다.
    
    **Command 패턴** : View에 입력이 들어오면 커맨드 패턴으로 ViewModel에 명령
    
    **DataBinding 패턴** : ViewModel의 값이 변하면 자동으로 UI가 업데이트
    
- 각각의 부분이 독립적이어서 모듈화가 가능하다.
- 유지보수 및 테스트 용이

**단점**

- 설계가 어렵다

## MVVM ViewModel , AAC ViewModel 차이

**MVVM에서의 ViewModel**

  MVVM 패턴에서 ViewModel은 View에 연결될 데이터와 메서드를 구현하고, 또, View는 ViewModel의 상태 변화를 옵저빙하는데, ViewModel의 상태가 변화되면 View 상태도 변경된다.

  따라서 MVVM ViewModel은 View와 Model 사이에서 데이터를 관리하고 바인딩하는 요소이다.

**AAC에서의 ViewModel**

![image](https://user-images.githubusercontent.com/82709044/167558007-ae3afbf7-d935-4e40-ae01-23df9c288639.png)

  AAC에서의 ViewModel은 Android의 수명 주기를 고려하여 UI 관련 데이터를 저장하고 관리하도록 설계되었다.

  ViewModel의 생명 주기는 위의 그림과 같아서 Activity가 파괴되기 전까지 ViewModel은 파괴되지 않고 유지된다. 그래서 화면 회전과 같이 View가 파괴되고 새로 그려지는 과정에서 데이터를 보존할 수 있다.

  따라서 Android의 수명 주기를 고려하여 UI 관련 데이터를 저장하고 관리하는 요소이다.

**정리하면** AAC ViewModel 없이 MVVM 패턴을 구현할 수 있지만, 반대로 AAC ViewModel을 사용했다고 해서 MVVM 패턴이 되지 않는다.

**하지만 왜 다른 사람들은 AAC ViewModel을 이용해서 MVVM ViewModel을 구현할까?**

바로 AAC ViewModel 에서 LiveData 등을 사용해서 View와 데이터 바인딩을 해준다면 MVVM 패턴의 ViewModel로써 사용이 가능하고, AAC ViewModel에서 생명주기를 알아서 구현해 주기 때문에 AAC ViewModel을 통해서 MVVM ViewModel을 구현한다고 생각한다.




### 참고
    
[[디자인패턴] MVC, MVP, MVVM 비교](https://beomy.tistory.com/43)

[[Android] MVVM 패턴과 AAC에서의 ViewModel](https://leveloper.tistory.com/216)
