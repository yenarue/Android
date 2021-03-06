CircleCI
=====

[번역중...](https://medium.com/@ayushigupta_2225/continuous-integration-and-deployment-through-circle-ci-fbe56ea316dc)


> 자연스러운 번역을 위해 약간의 생략이 이루어졌으며 약간의 의역이 포함되어있습니다.

# CircleCI를 통해 지속적으로 통합하고 배포하기
소규모 프로젝트의 경우에는 프로젝트 빌드, 테스트 및 배포와 관련하여 개발자가 직접 매뉴얼로 작업할 수 있습니다. 하지만 이 작업은 당신이 복잡한 포인트에 도달하거나 큰 개발팀과 함께 일하게 되었을 때 굉장히 큰 두통을 빠르게 유발할 것 입니다.

어떤 단계든지 실수로 놓치게 되면 여러 가지 문제들을 유발할 수 있습니다. Merge를 잘못하거나 코드 블록이 누군가에 의해 변경되고 다른 Component들이 깨져버리거나 유닛 테스트가 제대로 돌아가지 않거나 배포 중에 업로드할 파일을 하나 빼먹는다거나 (젠장!) 하는 일들은 전체 프로젝트 빌드를 중단시켜버릴 수 있습니다. ~~끔찍~~

우리는 이전부터 [Buddy Build Integration](https://docs.buddybuild.com/)와 같은 다양한 도구들을 사용해왔지만 더 높은 API 버전으로 마이그레이션함에 따라 쓸모없게 되어버렸습니다. 그래서 동일한 기능을 하면서도 동시에 전용 서버가 필요하지 않으며 무료인 도구의 필요성이 생겼습니다.

CircleCI는 이러한 모든 요구 사항에 가장 잘 어울리는 제품이었습니다. 자 그럼 CircleCI의 동작 흐름을 확인해보도록 하죠:

![](https://cdn-images-1.medium.com/max/800/1*bji3oKORQWSdwc7W4fGaPw.png)

* 개발자가 개발을 완료하고 저장소에 푸쉬를 합니다.
* 저장소는 CircleCI 시스템에 웹 훅(WebHook) 요청을 보냅니다.
* CircleCI 서버는 완벽한 환경 설정, 캐쉬, 테스트 빌드 apk, Slack에 업로드 하기(필요에 따라 Google Drive 또는 Firebase가 될 수 있다) 등의 프로세스를 실행합니다.
* CI 서버가 작업을 실행합니다. (테스트, 커버리지, 구문 체크 등...)
* CI 서버가 테스트를 위해 저장된 Artifact를 릴리즈 합니다.
* 빌드나 테스트가 실패하면 CI 서버는 팀에게 경고를 날립니다.
* 팀이 그 이슈를 해결합니다.

Jenkins, Travis CI 등과 같이, 지속적인 통합과 배포를 위한 도구들이 많이 존재합니다. 하지만 짧은 시간 내에 통합을 완료하길 원하는 경우, CircleCI의 일부 흥미로운 기능들이 당신에게 잘 맞을 것 입니다.

* CircleCI는 클라우드 기반의 시스템으로서 전용 서버를 설정할 필요가 없습니다. 추가 DevOps 요구사항이 없다는 뜻입니다.
* 무료입니다. 심지어 Business Account도요. 한달에 약 1500 분의 빌드 시간을 무료로 사용할 수 있습니다.
* Rest API : 프로젝트, 빌드, Artifact에 대한 접근성을 가지고 있습니다. 빌드의 결과물은 artifact가 됩니다. artifact는 Application이나 실행파일(예를 들면, 안드로이드 APK)이나 메타데이터(예를 들면, 테스트가 성공했다는 정보)로 컴파일 될 수 있습니다.. 
* Circle CI는 요구사항 설치를 캐싱합니다. 필요한 환경을 지속적으로(변함없이? 불변적으로?) 설치(Constant Installation)하는 대신 초기에 모든 써드파티 디펜던시들을 체크합니다.