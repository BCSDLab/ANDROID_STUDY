# DI (Dependency Injection)
DI는 Dependency Injection **의존성 주입**이라 말할 수 있다.

내부에서 객체를 생성하는 것이 아닌 외부에서 객체를 생성하여 의존성을 주입한다는 것.

우선 의존성(Dependency)을 알아야 한다. 

# Dependency
**"A가 B를 의존한다"**
> 의존대상 B가 변하면, 그것이 A에 영향을 미친다.
><br>
-토비의 스프링

즉, B의 기능이 추가되거나 변경되면 그 영향이 A에 미친다.

## 예시
햄버거 가게 **요리사**는 햄버거 **레시피**에 의존한다.<br>
이 말은 햄버거의 야채 상추가 깻잎으로 변경되면 가게의 요리사는 해당 레시피의 변경된 것을 확인하고 변경된 햄버거를 만든다.<br>

## Dependency Injection
의존성 주입이전에서는 레시피를 내부적으로 어떤 값을 가질지 직접 요리사에게 정했다.<br>
만약에 레시피를 외부에서 결정한다면, 이는 사장이 외부에서 레시피의 목록을 변경한다. 그리고 이 변경요소를 확인하여 요리사는 해당 레시피에 의존한다. 이를 의존성 주입(Dependency Injection)이라고 한다.

## DI의 장점
1. 의존성이 줄어든다.
2. 재사용성이 높은 코드
3. 테스트 용이
4. 가독성이 높아진다.


# Reference
[#1 의존관계 주입(DI)이란?]<br>
https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/

[#2 DI?]<br>
https://www.youtube.com/watch?v=0yc2UANSDiw