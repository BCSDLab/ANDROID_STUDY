# GC(Garbage Collector)
GC란 Garbage Collector로써 더 이상 사용하지 않는 메모리 영역을 알아서 해제해준다. 

## Mark-and-sweep
매모리에서 자기가 사용할 것들을 마크한 뒤 마크가 안된 것들을 모두 버리는 형식이다. 프로그래밍 측면에서는 연결되지 않은 모든것들을 치우는 것이다.

## Reference Counting
참조 카운팅이라고 한다. 한 요소가 다른 요소를 참조하는데, 어떤 요소가 아무도 참조하지 않고 0이라면 그것을 치우는 것이다.

## 한계점
내가 원하는 필요없는 쓰레기 값들을 버릴려고 해서 오류가 발생할 수 있다. 이렇기 때문에 프로그래머도 메모리 관리를 해줘야 한다.

# Memory Leak
메모리 누수란, 더이상 필요하지 않은 리소스가 램에서 해제되지 않고 계속 점유하고 있는 것을 말한다.

Java에서는 메모리 누수가 발생하면, 사용하지 않는 객체가 GC(Garbage Collector)에 의해 회수되지 않고, 계속 애플리케이션에 남아 누적되어 Crash 또는 OOM(Out Of Memory)가 발생한다.

# 메모리 누수 사례

## 글로벌 변수
무분별한 전역 변수 사용으로 메모리 누수 발생<br>
전역 변수로 사용 시 함수 내에서 어떠한 키워드 사용 없이 변수를 사용하게 된다면 함수가 끝나도 해당 변수는 남아있게 된다. 그러므로 사용되어지지 않음에도, GC에 의해서 메모리가 수거되지 않는다. 즉, 메모리 누수가 발생한다.

## 잊혀진 타이머와 콜백
어떠한 값을 5초마다 실행되는 핸들러 이전에 할당되어진다. 그리고 5초마다 실행되는 핸들러에 의해 그 값은 사라지지 않고 잊혀진 채 남아있게된다.

## 클로저(Closures)
클로저는 자신을 감싸고 있는 바깥 함수의 접근할 수 있는 내부의 함수를 말한다.

## DOM에서 벗어난 요소를 참조
document에서 불러진 요소를 삭제해줘야 한다.




# Reference
## GC
[#1 Garbage Collector 란?]<br>
https://velog.io/@recordsbeat/Garbage-Collector-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B8%B0

[#2 GC 유튜브 영상]<br>
https://www.yalco.kr/04_garbage_collector/

---
## Memory Leak

[#1 Memory Leak 이란?]<br>
https://velog.io/@dhwltnoooh/Memory-leak

[#2 메모리 누수 대처법]<br>
https://velog.io/@shin6403/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%88%84%EC%88%98Memory-Leak%EC%99%80-%EB%8C%80%EC%B2%98%EB%B2%95

