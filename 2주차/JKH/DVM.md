# DVM
<img src="https://velog.velcdn.com/images%2Fsery270%2Fpost%2Fb935d9f4-072e-49db-91cb-780a3afd1d8e%2Fimage.png">
JVM과 동일하게 모바일 환경에서 최적화된 런타임을 제공하기 위해 가상머신이며 JVM을 대체하여 나온 것이 DVM이다. DVM 은 Dalvik Virtual Machine이다.

## JIT
인터프리터 방식을 대체하기 위해서 나온 JIT(Just In Time)방식을 사용한다. 인터프리터 방식으로 실행하다 적절한 시점에서 바이트 코드 전체를 컴파일하여 네이티브 코드 변경 후 더 이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식이다.<br>
해당 방식은 한 번만 실행되는 코드라면 더 오래 걸릴 수 있으므로 JVM 메소드를 확인 후 적절한 방법을 사용한다.

### ✍ trace-JIT 란?
한계점을 초과하면 바이트 코드를 기계어로 해석한다. 즉, IF문이나 FOR문이 특정 횟수 이상 반복되면 컴파일

### ✍ Interpreter(인터프리터) 란?
자바 바이트 코드를 명령어 단위로 읽어서 실행 => 단점으로는 느리다(한 줄씩 읽기 때문에)

# JVM과 DVM
<img src="https://velog.velcdn.com/images%2Fsery270%2Fpost%2F27f49b0d-f5aa-4c33-a0e9-01be0488f623%2Fimage.png">

## ✍JVM이 아닌 DVM을 사용하는 이유는?
- 라이선스 문제
- 모바일 환경 최적화 : 배터리, 메모리 제약 문제


# Reference
[#1 JVM, DVM, ART]<BR>
https://velog.io/@sery270/JVM-DVM-ART