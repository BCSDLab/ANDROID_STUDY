# JVM, DVM, ART

## JVM(Java Virtual Machine)

JVM(Java Virtual Machine)은 자바 바이트 코드를 실행하는 주체이다. 플랫폼에 맞는 JVM을 가지고 있으면, 자바 바이트 코드를 플랫폼에서 실행할 수 있다. -> 플랫폼에 구애받지 않고 동일한 바이트코드 실행 가능

Java(Kotlin) Code -> (Compile) -> Java Byte Code -> (Interpreter, JIT...) -> Machine Language

### JIT Compile

Interpret 방식의 실행은 매번 바이트 코드를 기계어로 변환하는 과정이 있어 속도가 느려지는 문제가 있다. 이를 해결하기 위해 적절한 시점에 바이트 코드 전체를 컴파일하여 기계어로 변환하고, 이후 실행에도 활용하는 기법을 JIT(Just-in-Time) Compile이라고 한다.

JVM에서는 바이트코드를 실행하는 시점에서 JRE(Java Runtime Environment)가 바이트 코드를 JIT 컴파일을 통해 기계어로 변환된다. 이 과정은 JVM 내부에서 자주 실행되는 메소드 위주로 컴파일이 진행된다.

### JVM 특성

1. 스택 기반의 가상 머신
2. 단일 상속 형태의 객체 지향 프로그래밍을 가상 머신 수준에서 구현
3. 포인터를 지원하지만 주소를 임의 조작하는 포인터 연산은 불가능
4. 기본 타입의 정의를 명확히 함 -> 플랫폼 독립성 보장
5. 자바 바이트코드 검증기 -> Stack overflow, 타입 규칙 위반, 초기화 전 사용하는 지역 변수 등 여러 문제를 실행 전에 검증

### JVM 구성요소

![img](https://blog.kakaocdn.net/dn/tclVx/btq4Xfml6Dy/nzb5xxlGG1fr5iBGUMv77K/img.png)

#### Class Loader

JVM 내로 `.class` 파일을 로드하고 링킹

#### Execution

클래스 로더로부터 배치된 바이트 코드를 실행시킨다.

##### Interpreter

자바 바이트 코드를 명령 단위로 읽어서 수행. 한 줄씩 수행하기 때문에 느리다는 문제가 있다.

##### JIT

Interpreter 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일하여 기계어로 사용한 뒤 이후 바이트코드 대신 사용

#### Garbage Collector

사용되지 않는 인스턴스를 찾아 메모리에서 제거

#### Runtime Data Area

프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간

##### ![img](https://blog.kakaocdn.net/dn/cEjHLD/btq4YtqCAGY/rrVrI45UWSH2LqslkP8Wg0/img.png)

##### JVM Stack Area

프로그램 실행 시 임시로 할당되었다가 메모리를 빠져나가면 바로 소멸되는 특성의 데이터를 저장, 각종 변수나 임시 데이터, 스레드, 메소드 정보 등을 저장.

##### Native Method Stack

자바 바이트 코드가 아닌 실행 가능한 기계어로 작성된 프로그램을 실행시키는 영역

##### Method Area

클래스 정보를 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 공간

##### Runtime Constant Pool

static 영역에 존재하는 영역, 상수 자료형을 저장하여 참조 -> 상수 자료의 중복을 막는다.

(Field Information, Method Information, Type Information)

##### Heap 영역

객체를 저장하는 가상 메모리 공간

![img](https://blog.kakaocdn.net/dn/mxiE4/btq4Y5pwyCR/3nO3XIf20wUUTrzMKvn5yk/img.png)

1. **Permanent Generation**: 생성된 객체 정보의 주소가 저장되는 공간, Java 8 부터 제거됨
2. **New/Young Generation**: 생명 주기가 짧은 객체를 저장하는 영역, 가비지 콜렉터에 의해 제거될 수 있음(Eden:객체 최초 생성 공간, Survivor 0, 1: Eden 영역에서 GC 이후 살아남은 객체가 존재하는 공간). Minor GC
3. **Old Generation**: 생명 주기가 긴 객체를 저장하는 영역, New/Young Generation에서 일정 시간동안 참조되고 있는 객체들이 저장됨. 가비지 콜렉터에 의해 제거될 수 있음. Major GC라고 불리며 Minor GC에 비해 속도가 느린 편

## DVM(Dalvik VM)

VM(Dalvik VM)은 .dex 파일을 실행할 수 있는 JVM이다. 안드로이드에서 JVM 라이선스 문제를 회피하면서도 Java 코드를 이용할 수 있도록 개발됨

### Android에서 DVM을 선택한 이유

1. **라이선스 회피**: Android의 대부분은 Apache 라이선스 사용, JVM의 GPL 라이선스와 부적합
2. **속도**: 하드웨어 성능이 낮은 모바일 기기에서는 JVM에 사용된 스택 기반 모델보다 DVM에 사용된 레지스터 기반 모델이 더 적은 메모리를 사용하여 빠른 성능을 낼 수 있다. 다만 JIT 컴파일을 사용하여 메모리 사용량은 여전히 높은 편

## ART(Android Runtime)

Android 4.4부터 Dalvik의 성능 개선 및 고수준의 디버깅, 프로파일링 기능을 제공하는 ART가 추가되었으며 Android 5.0부터 적용되었다.

### ART의 특징

1. **AOT 컴파일러**: 기존 DVM은 JIT 컴파일러를 탑재하여 런타임에 기계어로 번역하는 과정을 수행했는 데, 속도 저하 및 캐시 사용에 따른 메모리 사용량 증가 문제가 있다. AOT(Ahead-of-Time) 컴파일러는 실행 전(설치 단계) 에서 dex 파일을 dex2oat 도구를 사용하여 기계어로 번역하여 런타임 성능과 메모리 사용량을 개선했다. 다만 설치 시 컴파일 과정으로 설치 과정이 오래 걸리고, 디스크 사용량이 늘어난다는 문제점이 있다.
2. **GC 개선**: ART에서는 앱 성능을 저하시킬 수 있는 가비지 컬렉션을 개선함
3. **디버깅, 개발 개선**: 샘플링 프로파일러, 추가 디버깅 기능 등 제공

## Links

https://doozi0316.tistory.com/entry/1주차-JVM은-무엇이며-자바-코드는-어떻게-실행하는-것인가

https://www.charlezz.com/?p=42686