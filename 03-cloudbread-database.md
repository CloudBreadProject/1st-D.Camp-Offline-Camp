#CloudBread 데이터베이스 설치 스크립트 수행
이 문서는 CloudBread에서 필수적인 *데이터베이스 개체*를 생성하는 방안을 소개합니다.

##CloudBread에서 데이터베이스 개체
CloudBread는 내부적으로 데이터 처리를 위하여 Azure SQL Database를 이용합니다. 이 Azure SQL Database에서 비즈니스 로직을 처리하기 위한 여러 개체들을 자동으로 생성하는 과정을 수행합니다.
이 자동 설치를 위해서는 Windows 명령 프롬프트에서 batch 파일을 실행해 SQLCMD를 통한 설치를 수행합니다.

##각각의 연결 문자열 확인
SQLCMD는 기본 준비사항으로 알려 드렸던 **SQL Server 2014 클라이언트** 도구에 포함되어 있습니다. 이미 설치하고 오신 분은 SQLCMD를 추가 설치할 필요 없습니다.  SQL Server 2014 클라이언트는 데이터베이스 연결 후 정보를 보거나 개발 및 테스트에 이용되는 훌륭한 도구이며 필수 권장 설치 사항입니다. 만약, **SQL Server 2014 클라이언트**를 설치하지 않으셨을 경우 SQLCMD만 설치 하시면 됩니다. 
SQLCMD 다운로드 링크 - https://www.microsoft.com/en-us/download/details.aspx?id=36433 
만약, ODBC 설치가 필요하다고 할 경우에는 ODBC 설치 후 다시 SQLCMD 설치를 수행하시면 됩니다. 
ODBC 드라이버 설치 링크 : https://www.microsoft.com/en-us/download/details.aspx?id=36434

###설치 스크립트 수행을 위한 데이터베이스 접속 정보 확인
스크립트를 수행하시 전, 자신의 Azure SQL Databse 연결 문자열을 통해 "서버주소", "데이터베이스이름", "SQL로그인ID", "SQL로그인암호"가 필요합니다. https://github.com/CloudBreadProject/1st-D.Camp-Offline-Camp/blob/master/cloudbread-connection-string.md 링크에서 이미 가져와 메모장에 복사하신 Azure SQL Database 연결 문자열은 보통 다음과 같은 형식입니다.
```
connectionString="Server=tcp:**서버주소**,1433;Database=**데이터베이스이름**;User ID=**SQL로그인ID**;Password=**SQL로그인암호**;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
```
위의 코드에서 보시는 것처럼, 자신의 "서버주소", "데이터베이스이름", "SQL로그인ID", "SQL로그인암호"을 메모장 등에 복사하고 아래 절차를 수행합니다.

###설치 스크립트 수행
1. CloudBread DB 설치 리포지토리로 이동합니다. https://github.com/CloudBreadProject/CloudBread-DB-Install-Script 
2. 우측 상단의 Fork를 수행해 자신의 리포지토리로 복사합니다.
3. 자신의 리포지토리에서 download 하거나 git이 익숙하실 경우 clone해 로컬로 복사합니다.
4. 윈도우에서 *명령 프롬프트*를 수행합니다. 윈도우키를 누르시고 "cmd" 라고 치시면 바로 보입니다.
5. 명령 프롬프트에서 다음 명령을 수행해 해당 폴더로 이동합니다.
```
cd "CloudBread-DB-Install-Script 다운로드 후 압축을 푼 경로"
```
6. 다음 명령을 수행해 Azure SQL 서버에 정상 접속되는지 체크합니다.
```
ping-test.bat 서버주소 SQL로그인ID SQL로그인암호 데이터베이스이름
```
ping 명령 수행시 서버의 version 정보가 보이면 정상 연결입니다. 만약, SQLCMD를 찾을 수 없다고 나오면 "C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\110\Tools\Binn" 경로에 일반적으로 SQLCMD가 존재합니다.(설치 경로는 저와 다를 수 있습니다) 다음 명령으로 path를 등록하고 다시 ping 명령을 수행합니다.
```
path "C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\110\Tools\Binn"
```
정상적으로 결과가 나오면 성공입니다.

7. 다음 명령을 수행해 CloudBread DB 개체를 자동 설치합니다.
```
install-CloudBreadDB-object-create.bat 서버주소 SQL로그인ID SQL로그인암호 데이터베이스이름
```
아무 오류 메세지가 없을 경우 정상 수행된 것입니다.

데이터베이스 개체 생성 과정이 완료 되었습니다.

다음으로, CloudBread 설정 과정이 필요합니다.

