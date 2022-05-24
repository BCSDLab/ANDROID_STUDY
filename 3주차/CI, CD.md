# CI
CI 는 Continuous Integration(지속적인 통합)을 의미한다. 간단히 요약하자면 **빌드 및 자동화 과정**이라고 할 수 있다. 애플리케이션의 변경된 코드가 정기적으로 통합되어 지속적으로 빌드 및 테스트된다면 동시 코드를 작성할 때 충돌을 막을 수 있다.<br>
커밋할 때마다 빌드와 일련의 자동 테스트가 이루어져 동작을 확인하고 변경으로 인해 문제가 생기는 부분이 없도록 보장한다.

# CD
CD 는 Continuous Delivery(지속적인 서비스 제공) 또는 Continuous Deployment(지속적인 배포)를 의미한다. 이 두 용어는 상호 교환적이다. 간단히 요약하자면 **배포 자동화 과정**이다.<br>
코드 변경이 파이프라인의 이전 단계를 모두 성공적으로 통과하면 수동 개입 없이 해당 변경 사항이 프로덕션에 자동으로 배포<br>
간단한 코드 변경이 정기적으로 마스터에 커밋되고, 자동화된 빌드 및 테스트 프로세스를 거치며 다양한 사전 프로덕션 환경으로 승격되며, 문제가 발견되지 않으면 최종적으로 배포

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FckMQmf%2FbtreLLPDmsL%2FtoxwM0zsTV38PrftEhmbt1%2Fimg.png">

# CI, CD 종류
- Jenkins

- CircleCI

- TravisCI

- Github Actions

# Reference
[#1 CI, CD]<br>
https://seosh817.tistory.com/104