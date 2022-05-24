
# Android Jetpack
jetpack이란?
- 안드로이드 앱을 개발하는데 도움을 주는 라이브러리 모음

Jetpack은 4가지 카테고리로 나눌 수 있다.
- Foundation Components
- Architecture Components
- Behavior Components
- UI Components
## Foundataion Components
- 앱 호환성: 머티리얼 디자인 사용자 인터페이스 구현을 지원해준다.
-  Android KTX 제공 
- Multidex : 앱에 여러 dex 파일을 지원합니다. 
## Architecture Components
- Data Binding
- Lifecycles
- LiveData
- Navigation
- Paging
- Room
- ViewModel
- WorkManager
들이 속해있다.
## Behavior Components
- Download Manager
- Media & playback
- Notifications
- Permissions
- Preferences
- Sharing
- Slices
들이 속해있다.
## UI Components
- Animation and transitions
- Auto
- Emoji
- Fragment
- Layout
- Pallete
- TV
- Wear
들이 속해있다.
# How to write test code
테스트 범위
코드의 변경으로 인해 영향받는 모든 기능
로직에 대한 검증 - MVVM이라면 ViewModel의 로직, MVP라면 Presenter의 로직
View에 대한 테스트는 필수적이지는 않다. -> View로 보여주는 데이터 값들이 올바른지를 테스트함으로써 대체 ->
View로 보여줄 데이터들을 ViewModel, Presenter에 선언하여 로직에 대한 검증으로 테스트

## 테스트 작성 법칙
-   Given - 특정 상황이 주어지고. ex) 잘못 된 이메일을 입력한다.
-   When - (테스트하려는) 특정 액션이 발생했을 때. ex) 로그인 버튼을 클릭한다.
-   Then - 변화 된 상태나 수행되는 행동을 검증. ex) 이메일 검증 실패 메시지를 보여준다.

# CI/CD
CI란?
애플리케이션에 대한 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 원격 레포지토리에 통합되어 여러명의 개발자가 협업 시 최신 코드상태를 베이스로 하여 서로 충돌할 수 있는 확률을 낮출 수 있다.
CD란?
배포 자동화를 의미한다. 배포 과정에서 인간은 실수를 할 수 있지만 컴퓨터는 정해진 명령을 따르기에 실수를 하지 않는다. 그렇기에 배포 실수를 줄일 수 있다.
