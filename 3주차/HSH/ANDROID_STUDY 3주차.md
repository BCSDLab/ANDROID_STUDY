# ANDROID_STUDY 3주차
## :rocket: Android Jetpack
> Jetpack은 개발자가 관심 있는 코드에 집중할 수 있도록 권장사항 준수, 상용구 코드 제거, 모든 Android 버전과 기기에서 일관되게 작동하는 코드 작성을 돕는 라이브러리 모음입니다.

* 장점
  > Android Jetpack의 장점 중 하나는 이전 Android 버전들과 호환이 된다는 것입니다.
  > Jetpack 라이브러리들은 __androidx.*__ 로 패키지화 되어있기 때문에 API로부터 분리되어 있고, 라이브러리 형태이기 때문에 자주 업데이트가 되어 항상 뛰어난 최신 버전의 Jetpack 구성요소에 액세스할 수 있습니다.


* 주의사항
  > Jetpack은 andoridx.* 패키지로 구성되어 있기 때문에 andorid.x로 마이그레이션이 필요합니다.

  * __마이그레이션:__ 한 운영환경으로부터, 대개의 경우 좀 더 낫다고 여겨지는 다른 운영환경으로 옮겨가는 과정을 의미합니다.

### Android Jetpack 구성요소
![image](https://1.bp.blogspot.com/-dwL58chu7wo/WvD1RrHln3I/AAAAAAAAFUg/cRTc0IZga_wMPTWr3CI53IZ5BwtnZMeYACLcBGAs/s1600/Screen%2BShot%2B2018-05-05%2Bat%2B11.49.30%2BAMimage1.png)

* __Architecture__
  * Data Binding : xml파일에 Data를 연결해서 사용할 수 있게 도와줍니다.
  * Lifecycles: andorid Activity 생명주기 관련 유틸리티를 제공합니다.
  * LiveData : 데이터가 변경될 때 실시간으로 View에게 알려줍니다.
  * Navigation : Activity, Fragment간 이동을 쉽게 도와줍니다.
  * Paging : 대량의 데이터를 관리해 주는 유틸리티입니다.
  * Room : Database를 보다 쉽게 사용할 수 있게 도와줍니다.
  * WorkManager : 백그라운드 작업을 보다 쉽게 도와줍니다.

* __Foundation__
  * AppCompat : 하위 안드로이드 앱에서 최신버전 sdk를 사용할 수 있도록 도와줍니다.
  * Android KTX : 코틀린 코드를 더욱 간결하게 만들어줍니다.
  * Multidex : dex 관리 관련 유틸리티입니다.
  * Test : 안드로이드 테스터 관련 유틸리티입니다.

* __Behavior__
  * Download manager : 큰 파일 다운로드를 service 차원에서 관리를 도와줍니다.
  * Media & Playback : 미디어 파일 재생 관련 유틸리티입니다.
  * Permissions : 안드로이드 권한 관련 유틸리티입니다.
  * Notification : 안드로이드 notification 관련 유틸리티입니다.
  * Sharing : ActionBar에서 데이터를 보다 쉽게 공유할 수 있도록 도와줍니다.

* __UI__
  * 앱에서 다양한 애니메이션, 이모지 또는 다양한 플랫폼(TV, Watch) 관련 유틸리티를 사용할 수 있는 컴포넌트입니다.
  * Compose : 네이티브 UI를 빌드하기 위한 Android 최신 도구 키트입니다.
  
## :memo: How to write test Code
### Test 환경 구성
* 실행 환경을 기반으로 테스트 디렉터리 구성
  * ``` androidTest ``` 디렉터리에는 실제 또는 가상 기기에서 실행되는 테스트가 포함되어야 합니다.

  * ``` test ``` 디렉터리에는 로컬 시스템에서 실행되는 테스트 (ex 단위 테스트)가 포함되어야 합니다.

* 여러 유형의 기기에서 테스트를 실행하는 경우 고려
  * 실제 기기
  * 가상 기기 (ex 에뮬레이터)
  * 시뮬레이션된 기기 (ex Robolectic)

* 테스트 더블 사용 여부 고려
  * 테스트 더블 대신 실제 개체를 테스트
    * 개체가 데이터 개체인 경우
    * 실제 개체 버전의 종속성과 통신하지 않으면 개체가 작동할 수 없는 경우
    * 개체와 종속성의 복제가 힘든 경우

  * 테스트 더블을 사용하여 테스트
    * 시간이 오래 걸리는 작업
    * 밀폐되지 않은 작업
    * 만들기 어려운 구성

### 테스트 작성
테스트 피라미드 수준

![image](https://developer.android.com/images/training/testing/pyramid.png?hl=ko)

* 소형 테스트 : 한 번에 한 클래스씩 앱 동작의 유효성을 검사하는 단위 테스트

* 중형 테스트 : 모듈 내의 스택 수준 간 상호작용 또는 관련 모듈 간 상호작용의 유효성을 검사흐는 통합 테스트

* 대형 테스트 : 앱의 여러 모듈에 걸쳐 사용자 여정의 유효성을 검사하는 엔드 투 엔드 테스트입니다.

각 카테고리의 테스트 비율은 앱의 사용 사례에 따라 다를 수 있지만, 일반적으로 __소형 70%, 중형 20% 및 대형 10%__ 로 카테고리를 나누는 것이 좋습니다.

## :detective: TDD, BDD
### TDD (Test Driven Development)
> 반복 테스트를 이용한 소프트웨어 방법론으로 작은 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현합니다.
>
> 짧은 개발 주기의 반복에 의존하는 개발 프로세스이며, 애자일 방법론 중 하나인 eXtream Programming(XP)의 Test First 개념에 기반을 둔 단순한 설계를 중요시합니다.


#### eXtream Programming (XP)
> 미래에 대한 예측을 최대한 하지 않고 지속적으로 프로토타입을 완성하고, 추가 요구사항이 생기더라도 실시간으로 반영할 수 있습니다.

#### TDD 개발주기

![image](https://hanamon.kr/tdd%eb%9e%80-%ed%85%8c%ec%8a%a4%ed%8a%b8-%ec%a3%bc%eb%8f%84-%ea%b0%9c%eb%b0%9c/tdd-%e1%84%80%e1%85%a2%e1%84%87%e1%85%a1%e1%86%af%e1%84%8c%e1%85%ae%e1%84%80%e1%85%b5/)

* RED : 실패하는 테스트 코드를 먼저 작성
* GREEN : 테스트 코드를 성공 시키기 위한 실제 코드 작성
* BLUE : 중복 코드 제거, 이반화 등의 리팩토링 수행

#### 일반 개발 방식

![image](https://hanamon.kr/tdd%eb%9e%80-%ed%85%8c%ec%8a%a4%ed%8a%b8-%ec%a3%bc%eb%8f%84-%ea%b0%9c%eb%b0%9c/%e1%84%80%e1%85%b5%e1%84%8c%e1%85%a9%e1%86%ab%e1%84%91%e1%85%b3%e1%84%85%e1%85%a9%e1%84%89%e1%85%a6%e1%84%89%e1%85%b3/)

일반 개발 방식의 단점
1. 소비자의 요구사항이 처음부터 명확하지 않을 수 있습니다.
2. 따라서 처음부터 완벽한 설계가 어렵습니다.
3. 자체 버그 검출 능력 저하 또는 소스코드의 품질이 떨어질 수 있습니다.
4. 자체 테스트 비용이 증가할 수 있습니다.

#### TDD 개발 방식

![image](https://hanamon.kr/tdd%eb%9e%80-%ed%85%8c%ec%8a%a4%ed%8a%b8-%ec%a3%bc%eb%8f%84-%ea%b0%9c%eb%b0%9c/tdd%e1%84%91%e1%85%b3%e1%84%85%e1%85%a9%e1%84%89%e1%85%a6%e1%84%89%e1%85%b3/)

> 이러한 반복적인 단계가 진행되면서 자연스럽게 버그가 줄어들고 소스코드는 간결해집니다.

#### TDD Tool
* JUnit
* xUnit

#### TDD 개발 방식의 장점
* 보다 튼튼한 객체 지향적인 코드 생산
* 재설계 시간의 단축
* 디버깅 시간의 단축
* 테스트 문서의 대체 가능
* 추가 구현의 용의함

#### TDD 개발 방식의 단점
* 생산성 저하
  처음부터 2개의 코드를 작성하고 중간중간 테스트를 하면서 수정해나가야 하기 때문입니다.

### BDD(Behavior Driven Development)
> BDD는 TDD의 한 종류로 메소드 위주의 TDD 보다 행동에 대해 조금 더 강조하는 특징이 있습니다.
> TDD에서 한 발 더 나아가 테스트케이스 자체가 요구사양이 되도록 하는 개발방법입니다.
> BDD를 통해 개발을 하게 되면 테스트 메소드의 이름을  "이 클래스가 어떤 행위를 해야한다"라는 문장으로 작성되어 행위에 대한 테스트에 집중할 수 있습니다.

#### BDD 기본패턴
> BDD는 시나리오를 기반으로 테스트 케이스를 작성하며 함수 단위 테스트에는 권장하지 않습니다.
> 시나리오는 개발자가 아닌 사람이 봐도 이해할 수 있을 정도의 레벨을 권장합니다.
> 시나리오는 Given, When, Then 구조를 가지는 것으로 기본패턴을 권장합니다.

* Feature: 테스트에 대상의 기능/책임을 명시합니다.
* Scenario: 테스트 목적에 대한 상황을 설명합니다.
* Given: 시나리오 진행에 필요한 값을 설정합니다.
* When: 시나리오를 진행하는 데 필요한 조건을 명시합니다.
* Then: 시나리오를 완료했을 떄 보장해야 하는 결과를 명시합니다.

#### BDD vs TDD

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbUzNKr%2FbtqWKY0WEpW%2FInOqM5mCiAEnwgQf10eT1k%2Fimg.png)

## :recycle: CI/CD
### CI (Continuous Integration)
> CI는 Continuous Integration 즉, 지속적인 통합이라는 의미입니다.
> 지속적인 통합이란, 어플리케이션의 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트 되어 공유 레포지토리에 통합하는 것을 의미합니다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGXdIT%2FbtqI9GkH3wP%2F5Qx2zLKYRxsYWLSoS6KH3K%2Fimg.png)

#### CI가 필요한 환경
* __다수의 개발자가 형상관리 툴을 공유하여 사용하는 환경__
  다수의 개발자가 한 팀으로 작업할 경우, 공유 레포지토리에 수많은 commit들이 쌓이게 됩니다. 그럴 때마다, 기능별로 빌드/테스트/병합까지 하기에는 상당히 번거롭습니다. 이런 상황에서 자동화된 빌드&테스트는 원천 소스코드의 충돌 등을 방어하는 Benefit을 제공할 수 있습니다.

* __MSA(Mircro Service Arhietecture) 환경__
  MSA는 작은 기능별로 서비스를 잘게 쪼개어 개발하는 형태를 의미합니다. MSA 환겨에서는 대부분 Agile방법론이 적용되기 떄문에, 기능 추가가 매우 빈번하게 발생하고 긴밀한 동작 테스트도 중요합니다. 이런 상황에서 CI의 적용은 기능 충돌 방지 등의 Benefit을 제공할 수 있습니다.

CI의 핵심 목표는 버그를 신속하게 해결하고, 소프트웨어의 품질을 개선하고, 새로운 업데이트의 검증 및 릴리즈의 시간을 단축시키는 것에 있습니다.

### CD (Continuous Delivery & Continuous Deployment)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeeSLmu%2FbtqI9pXqCN8%2FiIopSPh3KSK1SwhRjkWPf1%2Fimg.png)

> CD는 해석하자면, 지속적인 서비스 제공 혹은 지속적인 배포라는 의미입니다.
> Continous Delivery는 공유 레포지토리로 자동으로 Release하는 것을 의미하고,
> Continous Deployment는 Production 레벨까지 자동으로 deploy하는 것을 의미합니다.

<br>

### 정리   

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtVXYA%2FbtqI5zzWSYA%2F27KKvai04kcAM8jkMWnugk%2Fimg.jpg)

> CI가 새로운 소스코드의 빌드, 테스트, 병합까지를 의미했다면, 
> CD는 개발자의 변경 사항이 레포지토리를 넘어, 고객의 프로덕션 환경까지 릴리즈 하는 것을 의미합니다.

<br>

대표적인 CI/CD 툴
* Jenkins
* Travis CI
* Bamboo 등

### :eyes: Reference
[Android Jetpack](https://velog.io/@eoqkrskfk94/android-Jetpack)   
[How to write Testcode](https://developer.android.com/images/training/testing/pyramid.png?hl=ko)   
[TDD](https://hanamon.kr/tdd%EB%9E%80-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A3%BC%EB%8F%84-%EA%B0%9C%EB%B0%9C/)   
[BDD](https://beomseok95.tistory.com/293)
[CI/CD](https://artist-developer.tistory.com/24)

