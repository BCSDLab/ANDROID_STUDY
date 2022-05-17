# Dependency Injection

하나의 객체에 다른 객체의 의존성을 제공

## 의존성?

클래스간에 의존 관계가 있다 -> 한 클래스가 바뀔 때 다른 클래스가 영향을 받는다.

```Kotlin
class Car {
    private val engine = Engine()

    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val car = Car()
    car.start()
}
```

<img src="https://developer.android.com/images/training/dependency-injection/1-car-engine-no-di.png?hl=ko" alt="종속 항목 삽입이 없는 Car 클래스" style="zoom:50%;" />

`Car` 클래스에서 `Engine`을 변경하려면 직접 `Car` 클래스에서 수정을 거쳐야 한다. 이런 식으로 수정해야 할 코드가 많아질 수록 유지보수하기 어려워질 것이다.

```Kotlin
class Car {
    private val engine = ElectricEngine() // Modified

    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val car = Car()
    car.start()
}
```

 ## 의존성 주입

의존성 주입은 클래스 외부에서 객체를 생성하여 그 객체를 클래스 내부에 주입하는 것이다.

```Kotlin
class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val engine = Engine()
    val car = Car(engine)
    car.start()
}
```

<img src="https://developer.android.com/images/training/dependency-injection/1-car-engine-di.png?hl=ko" alt="종속 항목 삽입을 사용하는 Car 클래스" style="zoom:50%;" />

의존성 주입을 사용하면 다음과 같은 이점을 얻을 수 있다.

1. 객체 간 의존성을 줄이거나 없앨 수 있다.
2. 코드 리팩토링이 쉬워진다.
3. Unit Test가 쉬워진다.

## 제어의 역전(Inversion of Control, IoC)

Object 생성, 관계 설정, 사용, 제거 등 오브젝트 전반에 걸친 모든 제어권을 애플리케이션이 갖는 것이 아닌 프레임워크나 컨테이너에게 넘기는 개념

Dependency injection은 제어의 역전을 구현하는 방법 중 하나이다.

## Android의 DI Framework

### Koin

1. Kotlin DSL 사용
2. 런타임에 의존성 주입 수행 -> 컴파일 시간 단축, 런타임 퍼포먼스는 저하됨, 런타임 에러
3. 러닝커브 낮음 -> 쉽고 빠르게 DI 적용 가능
4. Service Locator 패턴과 유사 -> 의존성 구조가 복잡해질 수록 파악이 어려워짐

### Dagger2

1. 컴파일 타임에 의존성 주입 실행 -> 컴파일 시간 증가, 런타임 퍼포먼스 증가, 컴파일 에러
2. 리플렉션을 사용하지 않는다.
3. 러닝 커브가 높으며 프로젝트 설정이 까다로움

### Hilt

1. Dagger2를 기반으로 안드로이드 개발환경에서 사용하기 쉽고 표준화된 환경 제공
2. AndroidX라이브러리와 호환

## Links

https://whyprogrammer.tistory.com/610

https://developer.android.com/training/dependency-injection?hl=ko

https://beststar-1.tistory.com/33

https://blog.banksalad.com/tech/migrate-from-koin-to-hilt/

https://ahea.wordpress.com/2018/09/09/1754/

https://velog.io/@yuuuzzzin/Android-의존성-주입DI이란-with-dagger2-koin-hilt-비교