#AWS + CentOS + GitHub + Node.js

##목표
AWS에서 호스팅받아서 Cent OS설치후 Node.js로 Http서버 구축하며 GitHub로 형상관리하는것이 목표입니다.

##개발환경
- AWS EC2 (t2.micro) (Tokyo)
- Cent OS 6.0
- Node.js v5.5.0


##AWS
![alt tag](https://mateh.id.au/wp-content/uploads/2014/07/amazon-aws-logo.jpg)  
Amazon Web Service.  
아마존에서 만든 웹서비스 인프라로 웹서비스를 운영하는데 필요한 기술들을 포괄적으로 제공하는 서비스입니다.  
줄여서 AWS라고 부르고, 가상화 기술과 사용한 만큼 비용을 지불하는 종량제를 주요한 특징으로 합니다.

##AWS 가입

##AWS instance 생성
 - Services > EC2 선택
 - 좌측에서 INSTANCES > Instances 선택
 - 'Launch Instance' 선택
 - AWS Marketplace 선택후 'centos'검색
 - 'CentOs 6 with HVM' 선택후 'Review and Launch'
 - 'Launch'
 - Create a new key pair 선택후 key 이름 입력
 - 키발급 후 안전한곳에 저장

 
##AWS instance 접속
이제 Services > EC2 > Instances 에서 위에서 만든 인스턴스를 확인 가능합니다.  
SSH연결을 위해 SSH 클라이언트를 준비합니다.  
http://www.netsarang.co.kr/xshell_download.html  

Xshell 5 실행 > 파일 > 열기 > 새로만들기 선택합니다.  
이름에는 자신이 원하는 이름을 써넣습니다.  
호스트에는 EC2 인스턴스 우클릭 > Connect에서 표시되는 Public DNS 경로를 입력합니다.  
사용자 인증 탭으로 이동 후 '방법'을 'Public Key'로 변경합니다.  
'사용자 이름'에는 'centos'를 입력합니다.  
사용자 키 > 찾아보기 > 가져오기 선택 후 위에서 저장했던 key를 선택합니다.  
'확인' > 생성한 세션 선택후 연결  
'알수없는 호스트키' 수락 및 저장 합니다.  


```bash
Host 'ec2-52-193-74-30.ap-northeast-1.compute.amazonaws.com' resolved to 52.193.74.30.
Connecting to 52.193.74.30:22...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

[centos@ip-172-31-29-218 ~]$ 

```

##CentOS 설정

###root계정 설정
```bash
[centos@ip-172-31-29-218 ~]$ sudo passwd root
Changing password for user root.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

###Apache 설치

