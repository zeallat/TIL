#RESTful API 서버 구현하기

##RESTful API
###예시
#####요청
```GET [호스트]/api/users/1```
#####응답
```json
{
    "Error": false,
    "Message": "Success",
    "Users": [
        {
            "user_id": 1,
            "user_email": "zeallat@naver.com",
            "user_password": "81dc9bdb52d04dc20036dbd8313ed055",
            "user_join_date": "2016-01-27T06:53:47.000Z"
        }
    ]
}
```

###필요성
백엔드와 프론트엔드의 분리를 위해 필요합니다.  

###중심규칙
 - URI는 정보의 자원을 표현해야 한다.
 - 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)으로 표현한다.

###특징
####Stateless
세션, 쿠키정보 필요없습니다.    
서비스의 자유도 및 기타 유연한 설계가 가능합니다.

####URI를 이용
REST는 모든 유일한 오브젝트에 대해 유일하고 직관적인 URI을 통해 접근하도록 한다.  
e.g.)네이버 회원중 유저아이디가 1234인 회원의 정보를 조회  
http://www.naver.com/user/1234  

####HTTP 메소드를 사용
REST는 HTTP에서 제공하는 GET, PUT, POST, DELETE 4개의 메소드(여기에 HEAD나 OPTION을 추가하는 경우도 있다)를 이용해서 서비스를 제공합니다. 보통 DB의 CRUD와 같은 기능을 합니다.
그러나, CRUD로 분류할 수 없는 유형의 요청에 대해서는 URI를 잘 정의해야함으로 주의해야합니다.

##프로젝트 생성
###디렉토리 생성
```bash
$ mkdir test && cd test
$ npm init
```

###git설정 및 Package.json초기화
```shell
$ git init
$ npm init
$ git add *
$ git commit -m “first commit”
$ git remote add origin [레포지토리 ssh 주소]
$ git push -u origin master
```

####SSH Key 등록
#####ssh key 생성
```shell
$ ssh-keygen -t rsa -b 4096 -C "zeallat94@gmail.com"
```
#####ssh-agent 확인
```shell
$ eval "$(ssh-agent -s)"
```
#####ssh-agent에 ssh key 설정
```shell
$ ssh-add ~/.ssh/id_rsa
```
#####ssh key를 GitHub에 등록
```shell
$ cat ~/.ssh/id_rsa.pub 
```
ssh key를 복사 후 github > Settings > SSH keys에 가서 등록
#####ssh 연결 테스트
```shell
$ ssh -T git@github.com
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

###테이블 생성

```sql
CREATE TABLE user_info(
  user_id INT NOT NULL AUTO_INCREMENT,
    user_email VARCHAR(100) NOT NULL,
    user_password VARCHAR(200) NULL,
    creation_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (user_id),
    UNIQUE INDEX user_email_UNIQUE (user_email ASC)
);

CREATE TABLE message(
  message_id INT NOT NULL AUTO_INCREMENT,
    user_id_fk INT NOT NULL,
    body VARCHAR(2000) NULL,
    creation_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (message_id),
    FOREIGN KEY (user_id_fk) REFERENCES user_info(user_id)
);
```

###server.js
```javascript
var express = require("express");
var mysql   = require("mysql");
var bodyParser  = require("body-parser");
var md5 = require('MD5');
var rest = require("./rest.js");
var app  = express();

//객체 생성
function REST(){
    var self = this;
    self.connectMysql();
};

//DB 설정 및 연결
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

//Express 프레임워크 설정
REST.prototype.configureExpress = function(connection) {
      var self = this;
      app.use(bodyParser.urlencoded({ extended: true }));
      app.use(bodyParser.json());
      var router = express.Router();
      app.use('/api', router);
      var rest_router = new rest(router,connection,md5);
      self.startServer();
}

//서버 시작 함수
REST.prototype.startServer = function() {
      app.listen(3000,function(){
          console.log("All right ! I am alive at Port 3000.");
      });
}

//서버 중지 함수
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
  
```shell
$ npm start
```
이제 ```[호스트]:3000/api```로 접근하면 메세지가 뜨는것을 확인할 수 있습니다.  



##API 구현

###구현 목표
| Api | Type | Description |
|-----|------|-------------|
|/users|POST|유저 추가|
|/users|GET|유저 정보 목록 반환|
|/users/[user_id]|GET|특정 유저 정보 반환|
|/users/[user_id]|PUT|특정 유저 정보 업데이트|
|/users/[user_id]|DELETE|특정 유저 정보 삭제|
|/messages|POST|메세지 추가|
|/messages|GET|메세지 목록 반환|
|/messages/[message_id]|GET|메세지 정보 반환|
|/messages/[message_id]|PUT|메세지 정보 업데이트|
|/messages/[message_id]|DELETE|메세지 삭제|
|/users/[user_id]/messages|GET|특정 유저의 메세지 목록 반환|

###유저 API 구현

####rest.js
```javascript
var mysql   = require("mysql");

function REST_ROUTER(router,connection,md5) {
    var self = this;
    self.handleRoutes(router,connection,md5);
}

REST_ROUTER.prototype.handleRoutes = function(router,connection,md5) {

    router.get("/",function(req,res){
        res.json({"Message" : "Hello World !"});
    });

    //유저 추가
    //HTTP 메소드 타입 선택
    //URI 선택
    router.post("/users", function(req, res){
      //작업 완료시 콜백되는 함수입니다.
      //쿼리문 작성
      var query = "INSERT INTO ??(??,??) VALUES (?,?)";
      var table = ["user_info","user_email","user_password",req.body.user_email,md5(req.body.user_password)];
      query = mysql.format(query, table);
      connection.query(query, function(err, rows){
        if(err){
          //에러 발생시
          res.json({"Error" : true, "Message" : "Error during executing query - "+err});
        } else {
          //정상 처리시
          res.json({"Error" : false, "Message" : "Success"});
        }
      });
    });

    //유저 목록 조회
    router.get("/users", function(req, res){
      var query = "SELECT * FROM ??";
      var table = ["user_info"];
      query = mysql.format(query, table);
      connection.query(query, function(err, rows){
        if(err){
          res.json({"Error" : true, "Message" : "Error during executing query - "+err});
        } else {
          res.json({"Error" : false, "Message" : "Success", "Users" : rows});
        }
      });
    });

    //특정 유저 조회
    router.get("/users/:user_id", function(req, res){
      var query = "SELECT * FROM ?? WHERE ??=?";
      var table = ["user_info","user_id",req.params.user_id];
      query = mysql.format(query, table);
      connection.query(query, function(err, rows){
        if(err){
          res.json({"Error" : true, "Message" : "Error during executing query - "+err});
        } else {
          res.json({"Error" : false, "Message" : "Success", "Users" : rows});
        }
      });
    });

    //특정 유저 업데이트
    router.put("/users/:user_id", function(req, res){
      var query = "SELECT * FROM ?? WHERE ??=?";
      var query = "UPDATE ?? SET ??=? WHERE ??=?"
      var table = ["user_info","user_password",md5(req.body.user_password), "user_id", req.params.user_id];
      query = mysql.format(query, table);
      connection.query(query, function(err, rows){
        if(err){
          res.json({"Error" : true, "Message" : "Error during executing query - "+err});
        } else {
          res.json({"Error" : false, "Message" : "Success"});
        }
      });
    });

    //특정 유저 삭제
    router.delete("/users/:user_id", function(req, res){
      var query = "DELETE FROM ?? WHERE ??=?";
      var table = ["user_info","user_id",req.params.user_id];
      query = mysql.format(query, table);
      connection.query(query, function(err, rows){
        if(err){
          res.json({"Error" : true, "Message" : "Error during executing query - "+err});
        } else {
          res.json({"Error" : false, "Message" : "Success"});
        }
      });
    });
}

module.exports = REST_ROUTER;
```

####테스트
이제 구현한 API를 사용하여 유저를 추가, 조회, 삭제, 업데이트가 가능합니다.  
테스트는 HTTP 통신 테스트 툴인 'Postman'을 사용해보겠습니다.   
https://goo.gl/G4YqwW 이곳에서 받을 수 있습니다.   
