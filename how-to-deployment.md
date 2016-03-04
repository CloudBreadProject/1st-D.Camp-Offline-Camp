## 배포
이 문서는 ARM을 통해 리소스 배포를 하기 위한 방법을 기술합니다.
배포는 [CloudBread-ARM](https://github.com/CloudBreadProject/CloudBread-ARM) 에서 하실 수 있습니다.

### 준비사항
Microsoft 계정

### 배포 과정
1.CloudBread-ARM 프로젝트에서 fork를 수행.

2.[Deploy to Azure] 버튼을 클릭.

3.파라미터 입력.
 - postfix App Name : 생성하는 모든 리소스들의 고유이름
 - SQL server login ID : SQL 로그인에 필요한 ID
 - SQL server login password : SQL로그인에 필요한 비밀번호

**파라미터 입력시 주의할 점**

파라미터|주의사항
---|---|
postfix App Name|16자 이내의 소문자와 숫자
SQL server login ID|'admin' 이란 ID 사용 불가
SQL server login password|8자 이상. 특수문자를 하나 이상 포함

4.Resource group 생성. 또는, 이미 만들어 놓은게 있다면 사용.

5.배포 약관검토 동의하기.

6.[Create] button으로 배포.
