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

##기본 구성 및 데이터 베이스 연결
###server.js
```javascript
var express = require("express");
var mysql   = require("mysql");
var bodyParser  = require("body-parser");
var md5 = require('MD5');
var rest = require("./rest.js");
var app  = express();

function REST(){
    var self = this;
    self.connectMysql();
};

REST.prototype.connectMysql = function() {
    var self = this;
    var pool      =    mysql.createPool({
        connectionLimit : 100,
        host     : 'mysql1.cow8tthhqbik.ap-northeast-1.rds.amazonaws.com',
        user     : 'zeallat',
        password : 'rmfnrltnf123',
        database : 'test_db',
        debug    :  false
    });
    pool.getConnection(function(err,connection){
        if(err) {
          self.stop(err);
        } else {
          self.configureExpress(connection);
        }
    });
}

REST.prototype.configureExpress = function(connection) {
      var self = this;
      app.use(bodyParser.urlencoded({ extended: true }));
      app.use(bodyParser.json());
      var router = express.Router();
      app.use('/api', router);
      var rest_router = new rest(router,connection,md5);
      self.startServer();
}

REST.prototype.startServer = function() {
      app.listen(3000,function(){
          console.log("All right ! I am alive at Port 3000.");
      });
}

REST.prototype.stop = function(err) {
    console.log("ISSUE WITH MYSQL n" + err);
    process.exit(1);
}

new REST();
```

###rest.js
```javascript
function REST_ROUTER(router,connection,md5) {
    var self = this;
    self.handleRoutes(router,connection,md5);
}

REST_ROUTER.prototype.handleRoutes= function(router,connection,md5) {
    router.get("/",function(req,res){
        res.json({"Message" : "Hello World !"});
    })
}

module.exports = REST_ROUTER;
```
  

이제 ```[호스트]:3000/api```로 접근하면 메세지가 뜨는것을 확인할 수 있습니다.  


