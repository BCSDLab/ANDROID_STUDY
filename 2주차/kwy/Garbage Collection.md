## 왜 GC를 알아야 하는가

- GC는 런타임에서 메모리가 부족할 때 현재 사용되지 않는 객체에 할당된 메모리를 자동으로 회수함으로써 메모리를 관리해주지만, 우리가 기대하는 데로 작동하지 않는다.
- 그렇기 때문에 GC를 잘못 관리한다면 메모리 누수, ORM(Out Of Memory) 를 발생시켜서 애플리케이션이 강제 종료되기 때문에 따라서 GC에 대해 알아야 한다
- GC가 메모리를 회수하는 동안엔 앱이 Stop-The-World 상태가 되서 모든 작업이 멈춘다

> **GC의 대상**
>  1. 모든 객체 참조가 null인 경우
>  2. 객체가 블럭 안에서 생성되고 블럭이 종료된 경우
>  3. 부모 객체가 null이 된 경우, 자식 객체는 자동적으로 GC 대상이 된다.
>  4. 객체가 Weak 참조만 가지고 있을 경우
>  5. 객체가 Soft 참조이지만 메모리 부족이 발생한 경우
> 

## 메모리누수란?

애플리케이션에서 **사용하지 않는 객체**가 **사용중인 특정 객체**를 참조 중이라서 **사용되지 않는 객체**의 할당된 메모리를 GC가 되찾아올 수 없는 현상이다.

메모리 누수가 지속되면

1. 앱 버벅거림
    - 런타임에서 메모리가 부족할 때 GC가 작동되는데, 회수 할 수 없는 메모리가 많으면 많을수록 런타임에서 메모리가 자주 보족해서 GC를 많이 호출하게 된다. GC가 호출되는 순간 안드로이드의 UI렌더링이나 이벤트 처리를 중지시켜서 앱이 버벅거린다.
2. 앱 중단
    - 안드로이드는 앱의 응답을 Activity 매니저와 윈도우 매니저 시스템 서비스로 항상 모니터링하고 있다. 5초 이내에 입력 이벤트가 없을 때 ANR이 발생해서 앱이 강제 중단 될 것이다. 따라서 메모리 누수가 심하면 GC를 자주 호출할 것이고, 메모리 누수로 인해서 메모리 부족 현상이 일어나서 ORM(Out Of Memory)을 발생해서 앱이 강제 중단 될 것이다.

### Android에서 메모리 누수의 원인

1. **액티비티에서 inner class 사용**
2. **Static 뷰 객체를 액티비티에서 사용**
3. **handler를 액티비티가 종료된 후에도 사용**
4. **싱글톤 객체에서 Activity Context 사용**
    - Application Context 사용을 권장한다
5. **ViewModel에서 Activity의 context나 view 참조**
    - ViewModel은 Activity보다 생명주기가 살짝 더 길기 때문에 Activity가 Destory되도 잠깐 동안 살아 있기 때문에 메모리 누수가 생긴다.
6. **Fragment 안 에서 DataBinding/ViewBinding 사용시**
    - Fragment의 생명주기는 View생명주기 보다 길기 때문에 onDestroyView에서 binding에 null 값을 줘야한다
    
    ```kotlin
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
    ```
    

## 내 생각

GC가 한번 발생할 때 최대한 많은 메모리를 확보할 수 있도록, 생명주기를 비교해서 객체에 null 값을 넣어줘야겠다

### 참조
[안드로이드 개발 (5) - 메모리 누수](https://gift123.tistory.com/30)

[[Android, ORM] 메모리릭과 ORM이란?](https://black-jin0427.tistory.com/173)

[[내 맘대로 정리한 안드로이드] Garbage Collection(GC) 은 어떻게 동작하는가?](https://holika.tistory.com/entry/%EB%82%B4-%EB%A7%98%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%9C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-1-Garbage-Collection%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80)

[[Kotlin] 코틀린 내부 클래스(inner class)와 중첩 클래스(nested class) 좀 더 쉽게 이해하기 : Java와 반대(?)](https://ddolcat.tistory.com/548)
