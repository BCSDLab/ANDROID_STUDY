
# 의존성

- 한 클래스가 바뀔 때 다른 클래스가 영향을 받는 경우 두 클래스간에 의존 관계를 가지고 있다.
```java
class Programmer{
    private Coffe coffee;
    public Programmer() {
        coffee = new Coffee();
    }
    void start() {
		coffee.drink();
	}
}
class Coffee {
	void drink() {}
}
main() {
	val programmer = new Programmer() // 나는 아메리카노 커피가 먹고 싶다!!
	programmer.start()
}
```
의존성이 강하다.
만약 아메리카노 커피를 프로그래머가 먹고싶다면 Programmer 클래스에서 커피를 아메리카노 커피로 바꾸는 수정 작업이 이루어 진다. 
```java
class Programmer{
    private Americano coffee;
    public Programmer() {
        coffee = new Americano();
    }
    void start() {
		coffee.drink();
	}
}
class Coffee {
	void drink() {}
}
class Americano {
}
main() {
	val programmer = new Programmer()// 나는 커피달라했는데 왜 아메리카노 줘!!
	programmer.start()
}
```
그러나 이 경우 프로그래머의 coffee변수의 타입이 바뀌기 때문에 메인에 오류가 나게 됩니다.
Coffee 클래스를 인터페이스화 시키고 아메리카노가 Coffee 인터페이스를 상속한다면 프로그래머의 cofffee변수는 타입을 변경하지 않아도 어떠한 커피든지 받아낼 수 있게 됩니다.
```java
class Programmer{
    private Coffee coffee;
    public Programmer() {
        coffee = new Americano();
    }
    void start() {
		coffee.drink();
	}
}
interface Coffee {
	void drink()
}
class Americano implement Coffee {
	void drink(){}
}
class Lattee implement Coffee {
	void drink(){}
}
main() {
	val programmer = new Programmer() // 나는 아메리카노가 싫어 라떄줘!!
	programmer.start()
}
```
만약 아메키라노를 가진 프로그래머가 아니라 라때를 가진 프로그래머가 필요할 경우 또 프로그래머 클래스 코드를 수정해야 합니다. 이를 피하기 위해 의존관계가 있는 객체를 외부에서 만들어 전달한다면 프로그래머 클래스를 수정할 필요가 없게 됩니다.
```java
class Programmer{
    private Coffee coffee;
    public Programmer(Coffee item) {
        coffee = item;
    }
    void start() {
		coffee.drink();
	}
}
interface Coffee {
	void drink()
}
class Americano implement Coffee {
	void drink(){}
}
class Lattee implement Coffee {
	void drink(){}
}
main() {
	Lattee lattee = new Lattee()
	Programmer programmer = new Programmer(lattee) // 굿
	programmer.start()
}
```
의존하는 클래스를 직접 클래스에서 생성하는 것이 아닌, 외부에서 주입해줌으로써 객체 간의 결합도를 줄이고 유연한 코드로 바뀌게 됩니다.

# DI Library
DI Library는 의존성 주입을 위해 개발자가 작성해야 하는 코드들을 줄여주는데 도움을 준다.
Android DI Library는 Koin. Dagger2, Hilt가 있다.
각 라이브러리의 특징은 아래 사진과 같다.
[![N](https://blog.banksalad.com/static/60a40cf9a220aa91e8c5633620a454a8/0d292/comparison_for_di.png)]
# JVM
자바소스파일(.java) -> 바이트코드(.class) - 자바 컴파일러에 의해 바이트코드로 변환
jvm은 바이트코드를 읽어들일 수 있음 그래서 바이트 코드로 바꿔주어야 함
바이트 코드를 읽는 것은 Class Loader  
JVM 구조
![](https://lh5.googleusercontent.com/qRTsOp_WOOCMp8qA_qXZ5xk4vO4qFDOG5fn3fo0UGnX4SlTZMqvpH09S3FyOW5SKNC0A07gaKF-5jtyJRnClujoMh8r2l_4spJARAYYCEfVGD6xBt8iQuEUBVCwAeHVbXru6KQATG_ViMQKB8g) 
Class Loader 
-   JVM 내로 클래스 파일을 로드

Execute Engine
   -   RunTime Data Area에 배치된 바이트 코드들을 명령어 단위로 읽어 실행 

Garbage Collector    
-   힙 메모리 영역에 생성도니 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거
    
Runtime Data Area
   -   JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역
    
Method area
-   모든 쓰레드가 공유하는 메모리 영역
-   클레스, 인터페이스, 메소드, 필드, static 변수등이 있음

Heap area  
-   모든 쓰레드가 공유하는 메모리 영역
-   new 키워드로 생성도니 객체와 배열이 생성되는 영역
-   메소드 영역에 로드된 클래스만 생성 가능
-   gc가 참조되지 않은 메모리를 확인하고 제거하는 영역
   
Stack area
-   메서드 호출 시 각각의 스택 프레임이 생성됨
-   메소드 안에서 사용하는 값들을 저장
-   호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장
-   메서드 수행이 끝나면 프레임별로 삭제
-   클래스 Person p = new Person(); 이라는 소스를 작성했다면 Person p는 스택 영역에 생성되고 new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.
-   스택영역세 생성된 p의 값으로 힙 영역의 주소값을 가지고 있어 힙영역에 생성된 객체를 가리키고(참조하고)있는것이다.
    
PC register
-   쓰레드가 시작될 때 생성되며, 생성될 때마다 생성되는 공간으로 쓰레드마다 하나씩 존재
-   쓰레드가 어떤 부분을 무슨 명령으로 실행해야 할 지에 대한 기록을 하는 부분
-   현재 수행중인 jvm명령의 주소를 갖는다
  
 Native method stack
-   자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역
# ART & DVM
Android 런타임(ART)은 Android의 애플리케이션 및 일부 시스템 서비스에서 사용하는 관리형 런타임. 
ART와 그 전의 Dalvik은 Android 프로젝트용으로 특별히 제작되었다. 
ART는 런타임으로서 Dex 바이트 코드 사양을 실행합니다.

## 변천사
Dalvik

dex file을 Dalvik machine 위에 올리는 방식

라이선스 문제로 인해 JVM 대신 Java 코드를 이용할 수 있도록 개발됨

-   Bytecode → interpret → 실행 → 개선 → 네이티브 코드로 변환(JIT, Just in TIME 컴파일 방식)
-   장점 : 다양한 하드웨어나 아키텍쳐에서 실행 할 수 있는 장점(기존 JVM의 특징)
-   단점 : 성능과 배터리에 악영향, JIT compile 된 코드가 올라가 메모리가 늘어남

ART (Android 4.4 Kitkat 이후)

-   machine 위에서 OAT file을 돌리는 방식, VM이 아닌 런타임 시 사용되는 라이브러리
-   앱을 설치할 때 완전히 네이티브 코드로 변환되어 설치됨(AOT 컴파일, Ahead-Of-Time 컴파일)
-   장점 : 코드 Interpret 및 JIT compile 시간을 제거하여 performance가 향상됨
-   단점 : 설치시점에 소스 코드를 번역하여 설치가 느리고, 파일을 따로 저장하기 때문에 용량이 커짐

# GC
https://kotlinworld.com/340?category=914495 

# Memory Leak
앱이 사용이 끝난 메모리를 반환하지 않아 메모리 누수가 발생한다. 누수된 양(사용하지 않은 메모리 양)이 증가하게 되면 Out Of Memory를 발생시켜 앱이 강제종료가 일어날 수 있다.
메모리 릭의 주된 원인
- static view, context, activity를 많이 사용하는 경우
- Listener를 등록하고 해제하는것을 잊은 경우
- 잘못된 곳에 context, applicationContext를 호출하는 경우