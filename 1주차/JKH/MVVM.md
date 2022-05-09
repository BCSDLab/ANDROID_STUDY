# MVVM
MVVM(Model-ViewModel-View)의 목표로는 비즈니스 로직과 프레젠테이션 로직을 UI로부터 분리하는 것이다. 이는 테스트, 유지보수, 재사용을 용이하게 해준다.

<img src="https://github.com/iamchiwon/RxSwift_In_4_Hours/blob/master/docs/mvvm.jpeg?raw=true">

## Model
- 앱에서 사용할 데이터에 관련된 행위와 데이터를 관리한다.

## ViewModel
- View가 사용할 메서드와 필드를 구현한다. 이는 View에게 상태 변화를 알리는 것이다. (View는 ViewModel을 옵저빙한다.)
- 일반적으로 View와 ViewModel은 일대다 관계를 형성
- ViewModel은 View가 쉽게 사용할 수 있도록 Model의 데이터를 가공하여 View에게 전달한다.

## View
- UI에 관련된 것을 다루는 것
- 사용자가 스크린을 통해서 보는 것에 대한 구조, 레이아웃, 형태를 정의하는 것이다.
- 애니메이션과 같은 UI로직을 포함할 수 있다.

## 정리
뷰와 뷰모델관계에서는 뷰모델은 뷰를 신경쓰지 않는다. 그래서 뷰는 화면에 그려야할 요소가 필요하기 때문에 뷰모델을 지켜본다.(구독) 뷰모델에서 어떤 불형태의 함수가 false -> true로 변경됨을 처리하면 뷰에서는 그걸 인식하여 화면에 그릴 수 있다.

# Reference
[#1 디자인 패턴 및 MVVM 설명]<br>
https://velog.io/@k7120792/Model-View-ViewModel-Pattern

[#2 MVC, MVP, MVVM 유튜브 설명]<br>
https://www.youtube.com/watch?v=bjVAVm3t5cQ

[#2-1 github 자료]<br>
https://github.com/iamchiwon/RxSwift_In_4_Hours