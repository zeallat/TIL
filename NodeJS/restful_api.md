#RESTFUL API 서버 구현하기

##프로젝트 생성
###디렉토리 생성 및 초기화
```bash
$ mkdir test && cd test
$ npm init
```

###Package.json 수정
```json
{
  ...
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "~4.12.2",
    "mysql": "~2.5.5",
    "body-parser": "~1.12.0",
    "MD5": "~1.2.1"
  }
}
```
서버 시작 스크립트 추가 및 의존성 관계 패키지 추가  
```script```에서는 사용할 스크립트를 정의합니다.  
```dependencies```에서는 이 프로젝트와 의존관계에 있는 패키지를 정의합니다.  
```shell
$ npm install
```
```npm install```명령어는 의존성 관계에 있는 패키지를 설치합니다.  




