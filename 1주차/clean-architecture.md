# Clean Architecture

소프트웨어를 계층화해 관심사를 분리하고 테스트를 용이하게 한다. 

![The Clean Architecture의 다이어그램](https://blog.coderifleman.com/images/the-clean-architecture/the-clean-architecture.jpg)

## 의존 규칙

위 그림에서 각 계층은 안쪽을 향해서만 의존할 수 있다(안쪽 계층은 바깥쪽 계층을 알지 못한다). 이는 바깥쪽 계층의 이름을 안쪽 계층에서는 참조해서는 안된다는 의미이기도 하다.

또한 바깥쪽 계층에서 사용하고 있는 데이터 포맷을 안쪽 계층에서 사용하지 말아야 한다.

## 구조

### Entities

대규모 프로젝트 레벨의 비즈니스 규칙을 캡슐화, 하나의 애플리케이션을 작성할 경우에는 entity는 그 애플리케이션의 비즈니스 객체가 된다.

### Use Cases

애플리케이션의 고유 비즈니스 규칙을 포함하고 시스템의 모든 use case를 캡슐화하고 구현한다. 이 계층의 변경은 entity에 영향을 주지 않음을 기대하고 framework의 변경에 영향을 받지 않음을 기대한다.

### Interface Adapters

어댑터(usecase와 entity로부터 용이한 형식으로 DB, Web 등 외부 기능에 용이한 형식으로 변환)의 집합. 이 계층은 MVC, MVP, MVVM Design pattern architecture 를 모두 내포한다.

# Links

https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html

https://blog.coderifleman.com/2017/12/18/the-clean-architecture/ (윗 글 번역본)

