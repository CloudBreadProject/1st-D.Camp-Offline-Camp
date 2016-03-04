#CloudBread 설정
이 문서는 CloudBread를 클라우드로 배포 후 설정하는 과정을 소개합니다.

###설정이 필요한 이유
CloudBread core 서버 어플리케이션에서 데이터베이스와 저장소에 접근하기 위한 계정 정보와 서버 정보를 CloudBread core 서버에 설정하는 과정을 수행해야 정상 동작합니다.

연결문자열 수집을 안하셨을 경우 아래 과정을 통해 수행하세요.
연결 문자열 수집 절차 : https://github.com/CloudBreadProject/1st-D.Camp-Offline-Camp/blob/master/02-cloudbread-connection-string.md

###설정 방법
두 방법 중 하나를 택해서 수행하세요.
- 옵션1. Azure 모바일 앱의 *응용 프로그램 설정* 항목 - *연결 문자열* 항목과 *앱 설정* 항목에 설정 하는 방법
이번 유니티 캠프에서 이용하실 것을 권장합니다.
- 옵션2. CloudBread 프로젝트를 git clone 또는 다운로드 후 Visual Studio의 루트에 위치한 web.config 파일을 수정후 바로 자신의 Azure 모바일 앱으로 배포 하는 방안이며, 서버 개발자이거나 개발을 희망하실 경우 추천.

1회 유니티 캠프에서 권장하는 - Azure 포털에서 설정하는 절차는 아래를 수행하세요.

###CloudBread는 설정 항목 리스트
- 필수적으로 구성해야할 연결문자열 설정. CloudBread가 배포된 *Azure 모바일 앱*의 *응용 프로그램 설정* 항목 - *연결 문자열* 항목과 *앱 설정*으로 이동.
- **연결 문자열** 부분에 아래 값을 입력 - 메모장에 복사해둔 연결 문자열 활용
- **필수 설정 - 연결문자열 설정**

설정이름|연결문자열 값|추가 설정|항목 설명
---|---|---|---
CloudBreadDBConString|Azure SQL Database 연결문자열|SQL 데이터베이스|데이터베이스 연결에 사용
CloudBreadStorageConString|Azure Table Storage 연결문자열|사용자 지정|저장소 계정 연결에 사용

- 연결 문자열 설정 항목 바로 위의 **앱 설정** 항목에 아래 항목 입력
- **필수 설정 - 앱 설정**

설정이름|값|설명
---|---|---|---
CloudBreadSocketRedisServer|Redis Cache 연결문자열|이 키는 CloudBread-Socket 프로젝트 인증 토큰을 저장하는 Redis에 연결 할때 사용되는 값
CloudBreadRankRedisServer|Redis Cache 연결문자열|이 키는 leader board(ranking) 서비스 연결에 사용
CloudBreadGameLogRedisServer|Redis Cache 연결문자열|이 키는 redis를 게임 로그 서비스로 설정할 경우(아래 CloudBreadLoggerSetting과 연동) 연결에 사용

- **선택 설정 항목 **
아래 값들은 선택 항목입니다. 기본 값이 설정되어 있으니 따로 설정하실 필요 없으며, 설정 변경시 이용되니 참고하세요.

설정이름|기본값|설명
---|---|---
CloudBreadCryptSetting|AES256|암호화 방식 설정
CloudBreadLoggerSetting|ATS|기본 로그저장 방식은 Azure Table Storage로 바로 저장. SQL, AQS, redis 설정으로 저장 위치 변경 가능. CBLogger.cs 참조
CloudBreadconRetryCount|3|DB연결 재시도 수. Production 환경에서 스로틀링 핸들을 위해 30 설정 추천. 최대 30초 간격으로 3회 시도.
CloudBreadconRetryFromSeconds|5|DB연결 재시도 대기 시간 초. Production 환경에서 throttling 핸들을 위해 3으로 설정 추천. 최대 30초 간격으로 3회 시도.
CloudBreadRankSortedSet|cbrank|Redis에 저장되는 rank 기능 처리 Redis Sorted Set 이름
CloudBreadFillRedisRankSetOnStartup|true|최초 시작시 redis cache에 rank 정보를 채우는 처리 수행 여부
CloudBreadGameLogExpTimeDays|365|redis에 로그 데이터 적재시 로그 데이터 기본 저장일(게임 로그 저장 in-memory queue 서비스로 사용되는 것이며, Scheduler에 의해 batch로 Azure table Storage로 저장하는 패턴)

CloudBread 설정이 완료 되었습니다. 다음으로 CloudBread Socket 배포와 설정이 필요합니다.
