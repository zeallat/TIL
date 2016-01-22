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


```shell
Host 'ec2-52-193-74-30.ap-northeast-1.compute.amazonaws.com' resolved to 52.193.74.30.
Connecting to 52.193.74.30:22...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

[centos@ip-172-31-29-218 ~]$ 

```

##CentOS 설정

###root계정 설정
```shell
[centos@ip-172-31-29-218 ~]$ sudo passwd root
Changing password for user root.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

##Apache 설치


- 패키지 업데이트
```shell
$ sudo yum update -y
```


- httpd 설치여부 확인
```shell
$ sudo rpm -qa httpd
$ sudo yum list httpd
```


- httpd 설치
```shell
$ sudo yum install httpd -y
```


- httpd 서비스 시작
```shell
$ sudo service httpd start
```

  
- 방화벽 해제
```shell
$ su
$ service iptables stop
$ chkconfig iptables off
$ service ip6tables stop
$ chkconfig ip6tables off
```  
    
- [옵션] SELinux 보안해제  
'enforce'를 'disabled'로 수정
```shell
$ getenforce
$ cd /etc/selinux
$ vi config
$ setenforce 0
$ getenforce
```

이제 아마존에서 제공한 서버 도메인으로 접속하면 아파치의 테스트 페이지가 표시됩니다.  

##Git
![alt tag](https://git-scm.com/images/logo@2x.png)  

###가이드
https://rogerdudler.github.io/git-guide/index.ko.html

###설치
```shell
$ sudo yum install git -y
```

##GitHub
![alt tag](https://kanbanize.com/blog/wp-content/uploads/2014/11/GitHub.jpg)  
https://github.com/  
- 회원가입
- 레포지토리 생성

###SourceTree
![alt tag](https://www.sourcetreeapp.com/images/logoSourceTree.png)  
Git의 GUI 툴입니다.
####설치
https://www.sourcetreeapp.com/
####사용
http://hackersstudy.tistory.com/41  
https://opentutorials.org/course/1492/8037  
http://notpeelbean.tistory.com/entry/Git-GUI-%EB%8F%84%EA%B5%AC-SourceTree  

#####저장소 생성
복제/생성 > 새 저장소 생성

#####GitHub 연결
저장소 > 원격 저장소 추가

#####Push
- Hello.txt생성
- 스테이지에 추가
- 커밋
- 푸쉬
  
이제 GitHub에 파일이 정상적으로 업로드 된것을 확인할 수 있습니다.

#####Pull
```shell
$ mkdir node_test
$ cd node_test/
$ git clone https://github.com/zeallat/aws_node.git
$ git pull
```
clone명령을 통해서 원격 저장소를 복제할 수 있습니다.  
pull명령을 통해서 원격 저장소의 변경사항을 업데이트 받을 수 있습니다.  
##Node.js
![alt tag](http://3.bp.blogspot.com/-4knZxjdupuA/UFbTv8YsRZI/AAAAAAAAAis/oPuhGhDz-H4/s1600/nodejs-dark.png)  
Node.js®는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.

- nvm 설치
```shell
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.1/install.sh | bash
```
- Node.js 안정화 버전 설치
```shell
$ nvm install stable
```
- 버전 확인
```shell
$ nvm ls
$ nvm --version
```

###사용하기
####Hello World
```js
console.log('Hello World');
```
커밋 후 푸쉬
#####풀 후 실행
```shell
$ node test.js
```

####웹서버 만들기
```js
var http = require('http');

var server = http.createServer(function (req, res) {
  res.writeHead(200, { 'Content-Type' : 'text/plain' });
  res.end('Hello World');
});

server.listen(8000);
```
```shell
$ node test2.js
```
이제 도메인:8000 에서 웹서버가 가동중임을 확인해볼 수 있습니다.


####express
express는 Node.js에서 가장 유명한 웹 프레임워크 모듈입니다.  
express를 이용하면 더 간단하게 웹 서버를 만들 수 있고, 다양한 템플릿 엔진과 기능들을 사용할 수 있습니다.

#####express로 웹서버 만들기
시작하기에 앞서 git 레포지토리에 생성했던 파일들을 삭제합니다.  
```js
var express = require('express')
  , http = require('http')
  , app = express()
  , server = http.createServer(app);

app.get('/', function (req, res) {
  res.send('Hello /');
});

app.get('/world.html', function (req, res) {
  res.send('Hello World');
});

server.listen(8000, function() {
  console.log('Express server listening on port ' + server.address().port);
});
```
######express 모듈 설치
```shell
$ npm install express
```
npm(Node Packaged Modules)은 Node.js로 만들어진 모듈을 인터넷에서 받아서 설치해주는 패키지 매니저입니다.



