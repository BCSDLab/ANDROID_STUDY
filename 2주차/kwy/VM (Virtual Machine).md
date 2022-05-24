## JVM (Java Virtual Machine)

- 스택기반 가상머신이다.
- GC를 사용한다

## DVM (Dalvik Virtual Machine)

- 안드로이드 애플리케이션을 실행할 수 있는 가상 머신이다.
- JVM을 사용하지 않는 이유는 모바일은  Desktop PC 환경보다 메모리, 배터리 수명, 컴퓨팅 파워가 열악하기 때문이다
- JVM 스택 기반 모델과 다르게 레지스터 기반 모델을 사용해서 JVM보다 빠르게 동작한다.
- DVM 내부의 컴파일 방식은 JIT 방식을 사용한디. 이 방식은 앱 구동 중에 실시간 컴파일(기계어 번역)을 하기 때문에 설치 시 속도가 빠르지만 실행 시에 느리다는 단점이 있다. 또, JIT는 기계어 번역 결과를 캐쉬에 저장하여 메모리 캐시가 필요하다는 단점이 있다.

## ART (Android Runtime)

- 안드로이드 애플리케이션 런타임 환경으로 디버깅 연산 기능과 애플리케이션 프로파일링 기능을 제공한다.
- ART는 컴파일 시 AOT 방식을 사용한다. 이 방식은 설치 시에 컴파일을 하기 때문에 설치 속도가 느리지만 실행 속도가 빠르고 실행 도중에 컴파일 하지 않아서 CPU 사용이 줄어 배터리 수명이 향상되었다. 하지만 번역 정보를 파일에 저장하기 때문에 설치 용량이 많이 필요하다.

![image](https://user-images.githubusercontent.com/82709044/168722447-18881e66-f160-426b-9d4b-0175eb95e6cf.png)

### 참조
[JVM, DVM, ART 이해하기](https://charlezz.medium.com/jvm-dvm-art-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-c51d10dc56e3)

[Android - ART](https://velog.io/@xcellentbird/Android-ART)

[[Android] 컴파일 과정 (Dalvik vs ART)](https://s2choco.tistory.com/16)
