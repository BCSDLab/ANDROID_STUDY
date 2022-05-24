# TDD
TDD 란 Test Driven Development(테스트 주도 개발) 즉, 테스트를 먼저 작성하고 테스트가 정상적으로 돌아갈 때까지 테스트를 반복하게 된다.

## 종류
1. 단위(Unit) 테스트
2. 통합(Integation) 테스트
3. E2E(End To End) 텟트
4. 회귀(Regression) 테스트
5. 성능(Performance) 테스트

# BDD
BDD 란 Behavior Driven Development(행동 주도 개발) 즉, 사용자의 행위까지 테스트하는 주도 개발이다. BDD의 상위 개념은 TDD이며 잘짠 TDD라고 한다.

- TDD에서 발생할 수 있는 문제
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuTVym%2FbtqWTYyASB2%2FHYPOQeIUGJ2MDccFK4kk9K%2Fimg.png">

- TDD에서 발생할 수 있는 문제를 BDD에서 미리 문제를 잡아냄
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdghrk4%2FbtqWU63IGo5%2FVRkt4ysk0rRvHMxoMJF2x0%2Fimg.png">

# TDD, BDD
TDD와 BDD의 관계는 상호보완적인 관계이므로 프로젝트에서 BDD는 테스트케이스로 시나리오를 검증하고, 해당 시나리오에서 사용하는 모듈을 TDD의 테스트케이스로 검증하는 방법이다.
<IMG SRC="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc3Tov3%2FbtqWTYrQNbz%2FwLaAf4KM1NkvUlg9Ot8Lkk%2Fimg.png">

||TDD|BDD|
|:--:|:--:|:--:|
|테스트 코드의 목적은?|기능 동작의 검증|서비스 유저 시나리오 동작의 검증|
|테스트 코드의 설계중심은?|제공할 모듈의 기능 중심|서비스 사용자 행위 중심|
|테스트 코드 설계 재료는?|모듈 사양 문서(개발자가 작성)|서비스 기획서(서비스 기획자가 작성)|
|어떤 프로젝트에 적합한가?|모듈/라이브러리 프로젝트|서비스 프로젝트|
|장점은?|설계 단계에서 예외 케이스들을 확인할 수 있다.|설계 단계에서 누락된 기획안을 확인할 수 있다.|

# Reference
[#1 TDD, BDD 란?]<br>
https://mingule.tistory.com/43

[#2 TDD,BDD 카카오 영상]<br>
https://tv.kakao.com/channel/3693125/cliplink/414004682