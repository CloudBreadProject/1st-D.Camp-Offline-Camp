#CloudBread Scheduler 서비스 배포
이 문서는 CloudBread에서 Schduler Batch 역할을 담당하는 CloudBread-Schduler 배포 절차를 설명합니다.

##CloudBread-Scheduler 배포 절차 소개
CloudBread Scheduler는 Azure의 WebJob을 이용해 구현되었습니다. 2016년 3월 현재 WebJob의 가장 효율적인 방법은 Visual Studio를 이용해 *Publish as Azure WebJob*을 CloudBread-Admin-Web에 붙여서 배포하는 방안입니다.
따로 앱을 추가로 생성하지 않고, 이렇게 앱 서비스와 함께 배포하는 방식으로 처리됩니다.

###CloudBread-Scheduler를 분리해서 설치
위의 CloudBread-Admin-Web에 붙여서 배포하는 방식은 추천하는 개발/테스트 방안이며 만약, 대규모 배치를 수행하는 Scheduler 서비스일 경우 하나의 빈 웹 앱을 추가 후 웹 요청 인입이 없도록 web.config를 구성 후, CloudBread Scheduler 전용으로 구성할 것을 권장합니다. 웹 앱의 앱 서비스 계획에 따라 batch 서비스의 크기도 같이 스케일업/스케일아웃 됩니다.(singleton 설정 참조)

WebJob은 **Trigger**에 의해 동작하게 되는 구조로, WebJob 내부에서 *Timer  Trigger*를 이용해 cron 방식으로 trigger가 발생해 해당 시각에 작업할 내역을 바로 **Azure Queue Storage**라는 queue 저장소(이름은 cloudbread-batch)로 메세지를 보냅니다. 이 queue에 메세지가 도착하면, WebJob의 queue trigger에 의해 자동으로 queue의 메세지를 가져와 해당 메세지의 JobID를 읽어 batch를 수행합니다. Timer 트리거를 이용해 이 batch들은 모두 Singleton 작업으로 수행되며(2016년 3월) 수행이 완료되면 알림 서비스를 이용해 자동으로 Slack(기본) 또는 GMail로 전달하실 수 있으며, Slack을 이용하는 방식을 권장해 드립니다. 2016년 3월 현재, GMail은 "신뢰할 수 없는 기기에서 로그인 시도" 메세지가 발생합니다.

### Slack Webhook 구성
먼저, 이미 가입하고 오신 Slack에 로그인 합니다.
다음 링크에서 Incoming Web Hook을 구성합니다. https://my.slack.com/services/new/incoming-webhook/
즉, batch가 끝나면 자동으로 메세지가 인입될 slack 채널을 선택합니다.
처리하면 Channel webhook URL이 생성되고 이 URL과 채널을 메모장에 복사해 두세요. 전체 URL을 그대로 이용합니다.
```
https://hooks.slack.com/services/abcd/abcd/abcdefghgh
```
Slack Incoming Webhook 참고 링크 https://api.slack.com/incoming-webhooks

###설정 방법
1회 유니티 캠프에서 권장하는 - Visual Studio에서 설정하는 절차는 아래를 수행하세요.
1. https://github.com/CloudBreadProject/CloudBread-Scheduler 리포지토리에서 우측 상단의 Fork를 수행해 자신의 리포지토리로 가져옵니다.
2. CloudBread Scheduler 리포지토리의 Branch에서 ==**2.0.x 브랜치**==를 선택하고,
3. 해당 Branch를 git clone 또는 다운로드
4. Visual Studio의 솔루션 파일을 열고, 루트에 위치한 App.config 파일을 아래 절차대로 수정
5. *CloudBread AdminWeb*웹 앱에 WebJob으로 배포 하는 방안입니다. 배포 방안이 몇가지 추가로 존재하나 현 시점에서 가장 효율적인 방법으로 추천해 드립니다. 다른 방식처럼, Azure Portal에서 구성하시면 Visual Studio config 설정보다 우선하게 됩니다.
6. **필수 설정 - 연결문자열 설정** 과정 
설정이름|연결문자열 값|추가 설정|항목 설명

---|---|---|---
AzureWebJobsDashboard|Azure Table Storage 연결문자열|사용자 지정|저장소 계정 연결에 사용
AzureWebJobsStorage|Azure Table Storage 연결문자열|사용자 지정|저장소 계정 연결에 사용
CBSchedulerDBConnectionString| Azure SQL Database 연결문자열|SQL 데이터베이스|데이터베이스 연결에 사용
메모장에 복사해 두셨던 연결 문자열을 재사용해 구성합니다.

7. **필수 설정 - 앱 설정** 과정
설정이름|설명

---|---
CBNotiEmailSenderID| Gmail ID
CBNotiEmailSenderPassword|GMail PWD
CBNotiEmailSendToEmail|메일수신자 Email 주소 - batch 작업 후 팀이나 관리자 그룹메일로 설정
CBNotiSlackWebhookURL|위에서 생성한 Webhook URL 선택
CBNotiSlackChannel|Webhook을 받도록 구성한 채널 설정
CBNotiSlackUserName|메세지 보낸사람으로 표시할 Slack User Name


8. **선택 설정 항목** 처리 과정
아래 값들은 선택 항목입니다. 기본 값이 설정되어 있으니 따로 설정하실 필요 없으며, 설정 변경시 이용되니 참고하세요. 
설정이름|기본값|항목 설명

---|---|---
CloudBreadconRetryCount|3|DB연결 재시도 수. Production 환경에서 스로틀링 핸들을 위해 30 설정 추천. 최대 30초 간격으로 3회 시도.
CloudBreadconRetryFromSeconds|5|DB연결 재시도 대기 시간 초. Production 환경에서 throttling 핸들을 위해 3으로 설정 추천. 최대 30초 간격으로 3회 시도.

이 모든 구성을 완료하셨다면, Visual Studio에서 **Publish as Azure WebJob**을 수행해 배포합니다.

- 유휴 상태에서도 실행
Web Job을 수행하다가 로그를 보면 이런 메세지가 나오며 스케쥴러 서비스가 멈출 수 있습니다.
```
[03/01/2016 17:37:22 > 5a3d2c: SYS INFO] WebJob is stopping due to website shutting down
[03/01/2016 17:37:22 > 5a3d2c: SYS INFO] Status changed to Stopping
[03/01/2016 17:37:23 > 5a3d2c: SYS INFO] Status changed to Success
[03/01/2016 17:37:23 > 5a3d2c: SYS INFO] Status changed to Stopped
[03/01/2016 18:11:27 > 5a3d2c: SYS INFO] Status changed to Starting
[03/01/2016 18:11:27 > 5a3d2c: SYS WARN] 'Always On' doesn't appear to be enabled for this Web App. To ensure your continuous job doesn't stop running when the SCM host is idle for too long, consider enabling 'Always On' in the configuration settings for your Web App. Note: 'Always On' is available only in Basic, Standard and Premium modes.
```
위에서 보시는 것처럼, 유휴 상태일 경우 서비스가 종료되었다는 이벤트가 뜨면서 서비스가 중지됩니다. 앱 설정의 *응용 프로그램 설정* - *항상 사용* 설정을 켜시면 계속 서비스가 유지됩니다. *항상 사용* 설정은 기본 인스턴스 크기 이상부터 지원합니다.

Azure Scheduler 구성과 배포 과정이 완료 되었습니다. 다음으로 CloudBread Admin Web 배포 과정을 수행합니다.
