# ANDROID_STUDY
## 클린아키텍처란

주 목표는 사용자에게 보여지는 UI 로직과 데이터를 관리하는 비즈니스 로직으로 관심사 분리를 하는 것이다.

> **관심사 분리란?**
> 
> 
> 클래스간의 강한 의존성을 기능단위로 모듈화를 함으로써 느슨하게 만드는 것이다.
> 
> 기능단위로 모듈화를 하면 코드가 단순해지고, 테스트 하기가 쉬워지며, 유지 보수의 자유도가 증가해서 모듈 업그레이드, 재사용 및 독립적 개발의 기회가 더 많아진다.
> 

데이터 레이어의 분리를 통해서 데이터 영역을 여러 화면에서 사용할 수 있고, 단위 테스트를 위해 UI 외부에서 비즈니스 로직을 재현할 수 있다.

구글에서 권장하는 앱 아키텍처는 UI Layer, Data Layer, Domain Layer 이다.

![image](https://user-images.githubusercontent.com/82709044/167257124-ff481914-d1f3-4c62-965f-d4e4904157fe.png)

> *This rule says that source code dependencies can only point inwards     -[Uncle Bob]-*
> 

외부에 있는 Layer는 내부 Layer를 가르킬 수 있지만, 내부 Layer에 있는 것은 외부 Layer에 있는 것을 알면 안된다. 

## UI Layer

사용자와 상호작용하는 Layer이다.

UI레이어는 데이터 레이어에서 가져온 애플리케이션 데이터를 시각적으로 나타낸다.

UI 레이어는 화면에 데이터를 렌더링하는 UI요소(View)와 데이터를 보유하고 이를 UI에 노출하며 로직을 처리하는 상태 홀더(ViewModel)로 구성된다.

## Data Layer

데이터 레이어에는 애플리케이션 데이터 및 비즈니스 로직이 포함되어 있다.

데이터 레이어에는 DataSource와 Repository로 구성된다.

- Repository
    
    다양한 유형의 데이터마다 Repository 클래스를 만들어야 한다.
    
    Repository 클래스가 담당하는 작업은 다음과 같다.
    
    - 앱의 나머지 부분에 데이터를 노출한다.
    - 데이터 변경사항을 한 곳에 집중한다.
    - 여러 데이터 소스 간의 충돌을 해결한다.
    - 앱의 나머지 부분에서 데이터 소스를 추상화한다.
    - 비즈니스 로직을  포함한다.
- DataSource
    
    각 DataSource 클래스는 파일, 네트워크 소스, 로컬 데이터베이스 같은 하나의 데이터 소스만 사용해야 한다.
    
    DataSource 클래스는 데이터 작업을 위해 애플리케이션과 시스템 간의 다리 역할을 한다.
    
- Mapper
    
    Domain Layer가 있다면 Domain Layer는 Data Layer에 대해 모르므로, Data를 Domain Layer가 알 수 있도록 Mapping 해주는 Mapper가 필요하다.

## Domain Layer

UI 레이어와 Data 레이어 사이에 있는 선택적 레이어이다.

Domain 레이어는 복잡한 비즈니스 로직, 또는 여러 ViewModel에서 재사용되는 비즈니스 로직의 캡슐화를 담당한다.

Domain 레이어 장점

- 코드 중복을 방지한다.
- Domain 레이어 클래스를 사용하는 클래스의 가독성을 개선한다.
- 앱의 테스트 가능성을 높인다.
- 책임을 분할해서 대형 클래스를 방지한다.

### 참고
[](https://developer.android.com/topic/architecture/ui-layer)

[Separation of concerns - Wikipedia](https://en.wikipedia.org/wiki/Separation_of_concerns)

[Clean Coder Blog](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

[Clean Coder Blog 한글](https://k-elon.tistory.com/38)

[Clean Architecture 구글 문서](https://namget.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-Clean-Architecture)
