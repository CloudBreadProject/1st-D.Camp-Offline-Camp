# 1st-D.Camp-Offline-Camp
This repo is 1st offline camp content repo of CloudBread team at D.Camp

##CloudBread란?
Cloudbread는 클라우드기반의 무료 오픈소스 프로젝트이고, 모바일 앱 또는 모바일 게임에 최적화된 게임 서버 엔진 입니다.


###CloudBread 특징
게임 클라이언트 개발자가 채팅이나 게임 아이템 결제, 소셜인증, 푸쉬알람 등의 API구현하고 서버를 빠른시일 내로 완성할 수 있을까요?  
유저경향 분석을 위해 통계를 낼때 데이터를 저장한다고 하면, 발생하는 수많은 트랜잭션들을 잘 처리해 줄 수 있는 DB를 클라이언트 개발자가 빠르게 구현할 수 있을까요?  
또한, 서버를 구매했다 하더라도 사용자가 늘어나 성능을 업그레이드 할때 scale out, scale up을 하기 쉽지 않죠.  

CloudBread는 클라이언트 개발자가 필요로 하는 백엔드 개발시간 단축, 복잡한 트랜잭션 처리, 스케일 조정 등을 손쉽게 할 수 있도록 도와드립니다.

###사용 방법

1. CloudBread 프로젝트를 직접 실습해 보기 위해선 준비해둬야 하는 사항이 있습니다.  
[준비사항 확인](./00-cloudbread-requirement.md)


2. 모두 준비가 되었다면, 가장 먼저 해야할 일은 리소스들(Web app, Mobile app, Database 등)을 Cloud를 통해서 배포해야 하는 일입니다.  
[Cloud 배포](./how-to-deployment.md)

3. 저희는 방금의 배포로 아무것도 들어있지 않은 빈 리소스를 배포했습니다.  
모든 리소소들을 앞으로 하나씩 하나씩 채워보도록 하겠습니다.  
먼저 자신의 프로젝트를 외부로 배포하고 사용 할 수 있도록 연속배포를 수행합니다.  
[프로젝트 연속배포](./01-cloudbread-deploy.md)

4. 다음으로 어플리케이션이(Web app, Mobile app) DB나 저장소에 접근하기 위한 방법을 살펴보겠습니다.  
[연결문자열 처리](./02-cloudbread-connection-string.md)

5. 접근을 하기 위한 키들을 모두 기록해 놓으셨을 것입니다.  
그렇다면 이 모든 키를 이용해서, 프로젝트의 필수적인!! 데이터베이스 개체를 만들어 보도록 하겠습니다.  
[데이터베이스 개체 생성](./03-cloudbread-database.md)

6. 만들었다면, 이번엔 CloudBread Core 서버 app에서 데이터베이스와 저장소에 접근하도록 해보겠습니다.  
[CloudBread 설정](./04-cloudbread-config.md)

7. 지금까지 데이터베이스와 저장소를 모두 연동했고, 배포까지 완료되었습니다.  
이번엔 실시간 통신을 위해, Web app에 CloudBread-Socket 프로젝트를 설치해 보도록 하겠습니다.  
[Socket 설치](./how-to-install-socket.io.md)

8. 소켓으로 연동을 완료했다면 웹에서 채팅을 테스트해 보실 수 있습니다.  
다음으로 통계를 내기위해 데이터를 쌓는 스케쥴러 서비스를 배포해 보겠습니다.  
[Cloud Scheduler 배포](./06-cloudbread-scheduler-eploy-config.md)

9. 통계 데이터는 admin web 사이트에서 확인 하실 수 있습니다.  
그럼 admin web사이트도 연속배포를 수행해 볼까요?  
[admin WebApp 연속배포](./07-cloudbread-adminweb-deploy.md)
