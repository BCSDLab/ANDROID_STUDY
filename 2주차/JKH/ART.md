# ART
ART(Android RunTime)은 안드로이드 애플리케이션 런타임 환경으로 새로운 디버깅 기능과 더 정확한 고수준의 애플리케이션 프로파일리 기능을 제공한다.<br> 
- Android 4.4(API 19)에서 처음 등장, DVM과 선택적 사용
- Android 5.0(API 21) 이후, 기본 런타임으로 지정
- Android 7.0 이후로는 AOT + JIT

## ✍ AOT(Ahead-Of-Time)란
- 설치 시점에 이미 컴파일을 완료하여 기계어로 해석을 끝냄
- 실행시에는 해석 과정이 없이 곧바로 기계어로 실행

# DVM과 ART
## DVM
DVM은 JIT 컴파일러를 사용한다.
## ART
ART는 AOT 컴파일러를 사용한다.

||DVM|ART|
|:--:|:--:|:--:|
|컴파일러|JIT|AOT|
|실행|Dex, ODEX|OAT(ART)|
|GC|CMS 알고리즘|Customized CMS 알고리즘(기존보다 2배 빠름)|
|설치 속도|빠름|느림|
|실행 속도|느림|빠름|
|배터리 수명|짧다|길다|
|앱 설치 공간|적게 필요|많이 필요|


# JVM, DVM, ART
## JVM : 가상머신
## DVM : 가상머신
## ART : 라이브러리

# Reference

[#1 jvm, dvm, art ]<Br>
https://charlezz.medium.com/jvm-dvm-art-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-c51d10dc56e3

https://velog.io/@sery270/JVM-DVM-ART
