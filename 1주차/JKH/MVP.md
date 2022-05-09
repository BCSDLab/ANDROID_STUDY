# MVP
MVP(Model-View-Presenter)는 MVC에서 Controller의 역할을 Presenter가 해줄 수 있다고 보면 된다. 그러면 MVC와 MVP가 똑같은 것이 아니냐라고 할 수 있다. 하지만 엄연히 다르다. MVC는 Model과 View가 서로 연결되어 의존관계를 갖는다. 하지만 MVP는 Model과 View가 분리되어 있고 오직 Presenter를 통해서 상태나 변화를 알려줄 수 있다.

<img src="https://github.com/iamchiwon/RxSwift_In_4_Hours/blob/master/docs/mvp.jpeg?raw=true">

## Model
- 프로그램 내부적으로 쓰이는 데이터를 저장, 처리하는 역할(비즈니스 로직)
- View 또는 Presenter에 의존하지 않고 독립적
## View
- UI 담당
- 행동이나 상태의 변화를 인식하여 Presenter에 전달하는 역할
- Presenter를 이용하여 데이터를 주고받기 때문에 Presenter에 매우 의존적
## Presenter
- Model과 View 사이의 매개체

## 정리
MVP모델에서 View와 Presenter는 1:1관계를 갖는다. View에서 입력 1을 Presenter로 보내면 Presenter는 1+1 = 2를 연산하여 2를 화면에 그리라고 View에게 알려준다. 여기서 단점이 나오는데, View 하나를 그릴 때마다 Presenter를 매번 만들어야 한다.(공통 로직을 따로 처리한다고 하더라도)



# Reference
[#1 MVP 패턴 설명]<br>
https://fomaios.tistory.com/entry/Design-Pattern-MVP-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80

[#1-1 MVP 패턴 설명]<br>
https://frtt0608.tistory.com/94

[#2 MVC, MVP, MVVM 유튜브 설명]<br>
https://www.youtube.com/watch?v=bjVAVm3t5cQ

[#2-1 github 자료]<br>
https://github.com/iamchiwon/RxSwift_In_4_Hours