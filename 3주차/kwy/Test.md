# Test

## 왜 Test Code ?

1. 잘못된 부분을 빠르게 확인할 수 있어서 코드 안정성과 신뢰성이 높아진다.
2. 모듈이 의도대로 동작하고 있음을 확인할 수 있어서 리팩토링 시 부담을 줄여준다.

## TDD

- 테스트를 먼저 만들고 테스트를 통과하기 위해 코드를 짜는 것을 말한다
- 장점
    - 개발 단위가 더 작아져서 문제가 생길확률이 적으며 금방 찾을 수 있다
    - 나중에 유지보수시 함수의 기능이 변경/추가 되면 테스트를 통해 함수의 유효성을 확인할 수 있다
- 단점
    - 개발 시간이 늘어나게 된다
    - 러닝커브가 높다

## BDD

비즈니스 요구사항에 집중하여 테스트 케이스를 작성하고 개발을 진행하는 개발 방식

사용자의 행위를 작성하고 결과 검증을 진행한다

함수 단위 테스트를 권장하지 않는다

- 형식
    - Given : 사용자의 행위시 주어진 환경 서술
    - When: 사용자 행위 서술
    - Then: 행위에 따른 기대 결과 서술
- 장점
    - 기획서상 요구사항을 BDD 테스트 케이스로 모두 작성한다면 빈틈없이 서비스 설계가 가능하다
    - 불필요한 테스트 케이스 작성의 비용을 줄일 수 있다.
    - 서비스의 이해가 용이하다
    
## TDD , BDD 차이
![image](https://user-images.githubusercontent.com/82709044/170069603-9b0b747c-a4b3-428b-829a-31e58c2865c0.png)
