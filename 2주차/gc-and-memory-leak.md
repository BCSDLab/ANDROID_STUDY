# 메모리 누수와 GC

## Garbage Collection

JVM의 가비지 컬렉터는 불필요한 메모리를 자동으로 정리하는 기능을 한다.

```Java
Person person = new Person();
person.setName("YSM");
person = null; // 가비지 발생

person = new Person();
person.setName("YSM2");
```

처음 생성된 Person 객체는 더이상 참조를 하지 않아 가비지가 된다. JVM의 가비지 컬렉터는 이런 가비지를 주기적으로 검사하여 메모리에서 제거한다.

### Minor GC & Major GC

Java의 Heap 영역은 설계 시 2가지를 전제로 설계되었다.

1. 대부분의 객체는 금방 접근 불가능한 상태가 된다(Unreachable).
2. 오래된 객체에서 새로운 객체로의 참조는 매우 적게 존재한다.

*-> 객체는 대부분 일회성이며, 메모리에 오래 남지 않는다.*

*-> 객체의 생존 기간에 따라 물리적인 Heap 영역을 Young, Old로 나눴다.*

1. **Young Generation**
   - 새롭게 생성된 객체가 할당되는 영역
   - 대부분은 금방 Unreachable 상태가 됨
   - 이 영역에 대한 가비지 컬렉션을 Minor GC라고 칭함
2. **Old Generation**
   - Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사됨
   - Young 영역보다 크게 할당 -> 영역이 큰 만큼 가비지가 적게 발생
   - 이 영역에 대한 가비지 컬렉션을 Major(Full) GC라고 칭함

Old Generation이 Young Generation보다 영역의 크기가 큰 이유는 보통 Young 영역의 객체는 큰 공간을 필요로 하지 않으며, 큰 객체들은 바로 Old 영역으로 할당하기 때문이다.

#### Card Table

Old 영역의 객체가 Young 영역의 객체를 참조할 때 그 정보를 기록하는 테이블.

Minor GC가 실행될 때 참조되지 않는 Young 영역의 객체를 쉽게 식별하여 GC 대상을 파악하는 데 필요한 오버헤드를 줄일 수 있다.

### Garbage Collection의 동작 방식

기본적인 가비지 컬렉션의 동작 방식은 다음과 같다. 다만 Young와 Old 영역은 메모리 구조가 달라 세부 동작 방식은 다르다.

1. Stop the World
2. Mark and Sweep

#### Stop the World

가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. 이 과정을 수행하는 동안에는 GC를 실행하는 스레드를 제외한 모든 쓰레드의 작업이 중지되고 GC가 완료되면 재개된다.

모든 스레드가 중단되는 것은 즉 애플리케이션이 멈춘다는 뜻이기 때문에 이 시간을 줄이는 것은 성능 개선에 도움이 된다. JVM에서도 이런 문제를 해결하기 위해 다양한 옵션을 제공한다.

#### Mark and Sweep

- **Mark**: 사용되지 않은 메모리를 식별
- **Sweep**: Mark 단계에서 식별된 메모리를 해제

Stop the World 과정을 통해 작업을 중지시키면 GC는 스택의 모든 변수, Reachable 객체를 스캔하며 참조 객체를 탐색하고 사용되고 있는 메모리를 식별하며 Mark 한다. 이후 Mark되지 않은 객체를 메모리에서 제거하는 Sweep 과정이 일어난다.

#### Minor GC 동작 방식

Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역이 있는데,

- Eden 영역: 새로 생성된 객체가 할당되는 영역
- Survivor 영역: 최소 1번 이상의 GC 과정에서 살아남은 객체가 존재하는 영역

Eden 영역이 꽉 차면 Minor GC가 발생 -> 사용되지 않는 메모리는 해제되고 남은 객체는 Survivor 영역으로 옮겨진다.

위 과정이 반복되다 Survivor 영역이 가득 차면 다른 Survivor 영역으로 이동시킨다(기존 영역은 빈 상태가 됨).

여기서 객체의 생존 횟수를 카운트하는 `age`를 Object header에 기록하고, Minor GC때 `age`를 보고 Old 영역으로 이동시킬 지 결정한다.

#### Major GC 동작 방식 

Major GC는 Old 영역의 메모리가 부족해지면 발생한다. Old 영역은 Young 영역보다 크기가 크기 때문에 Minor GC보다 시간이 오래 걸리며, 자주 실행되면 애플리케이션 동작 속도에 영향을 줄 수 있다.

## Memory Leak

컴퓨터 프로그램이 필요하지 않은 메모리를 계속 점유하고 있는 현상. 할당받은 메모리를 사용한 다음 제대로 반환하지 않는 일이 누적되면 메모리가 낭비된다.

## Memory Leak in Java

사용되지 않는 객체들이 가비지 컬렉터에 의해 회수되지 않고 누적이 되는 현상

## Links

https://mangkyu.tistory.com/118