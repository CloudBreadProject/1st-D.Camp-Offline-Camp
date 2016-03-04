#CloudBread 연결 문자열 처리
이 문서는 CloudBread에서 필수적인 *연결 문자열(Connection String)*을 처리하는 방안을 소개합니다.

##CloudBread에서 연결 문자열
연결 문자열은 ConnectionString으로 보통 어플리케이션이 데이터베이스나 저장소에 접근하기 위한 계정 정보와 서버 정보를 포함하는 문자열입니다.

CloudBread는 데이터 처리를 위한 기본 데이터베이스로 **Azure SQL Database**, Key/Value 기반 NoSQL 게임 로그 저장소로 **Azure Table Storage**, 인메모리(In-memory) Key/Value 기반 NoSQL 저장소로 리더보드(랭킹), 소켓서버 인증, 대량로그 처리 중간 큐 저장소의 용도로 **Redis Cache**를 이용합니다.
각 서비스들의 연결 문자열을 복사해 어플리케이션에서 설정해야 정상적으로 CloudBread 서비스가 구동됩니다.

##각각의 연결 문자열 확인
연결 문자열이 있어야 CloudBread 어플리케이션이 DB나 저장소와 연동되어 완전하게 동작하게 설정 가능합니다. 아울러, 자신의 장비에서 CloudBread를 추가/수정 등 개발하실 경우에도 반드시 이 연결 문자열이 필요합니다.

###데이터 처리를 위한 기본 데이터베이스로 Azure SQL Database 연결 문자열 확인
1. 자신의 포털에 로그인 합니다. https://portal.azure.com 로그인 후 생성한 리소스 그룹 선택
2. 형식이 *SQL 데이터베이스*로 생성된 Azure SQL Database를 선택
3. *설정* 화면에서 *데이터베이스 연결 문자열 표시* 선택
4. 우측에 나온 *ADO.NET* 연결문자열 전체를 자신의 메모장 등에 저장

Azure SQL Database 연결 문자열 확인 및 복사 과정이 완료 되었습니다.

###Key/Value 기반 NoSQL 저장소 서비스인 Azure Table Storage 연결 문자열 확인
1. 자신의 포털에 로그인 합니다. https://portal.azure.com 로그인 후 생성한 리소스 그룹 선택
2. 형식이 *저장소 계정*으로 생성된 Azure Table Storage를 선택
3. *설정* 화면에서 우측 블레이드의 *엑세스 키* 선택 후 *연결 문자열* 전체를 자신의 메모장 등에 저장

Azure Table Storage 연결 문자열 확인 및 복사 과정이 완료 되었습니다.

###인메모리(In-Memory) Key/Value 기반 NoSQL 저장소 서비스인 Redis Cache 연결 문자열 확인
1. 자신의 포털에 로그인 합니다. https://portal.azure.com 로그인 후 생성한 리소스 그룹 선택
2. 형식이 *Redis Cache*로 생성된 Azure Redis Cache를 선택
3. *설정* 화면에서 *키 - 엑세스 키 표시* 선택. 
4. 우측 블레이드의 *Primary connection string(StackExchange.Redis)* 연결문자열 전체를 자신의 메모장 등에 저장

Redis Cache 연결 문자열 확인 및 복사 과정이 완료 되었습니다.
