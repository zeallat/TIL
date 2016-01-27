#RESTFUL API 서버 구현하기

##프로젝트 생성
###디렉토리 생성 및 초기화
```bash
$ mkdir test && cd test
$ npm init
```

###Package.json 수정
서버 시작 스크립트 추가 및 의존성 추가
```json
{
  "name": "RESTful-API",
  "version": "0.0.1",
  "scripts": {
    "start": "node Server.js"
  },
  "dependencies": {
    "express": "~4.12.2",
    "mysql": "~2.5.5",
    "body-parser": "~1.12.0",
    "MD5": "~1.2.1"
  }
}
```






