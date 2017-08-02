
## 목차

* [1. express기본](#express기본)


<br/>

# express기본

## express
http 모듈만 사용해서 웹 서버를 구성할 때는 많은 것들을 직접 해야 한다. 따라서 좀 더 간편하게 웹서버의 기능을 구현하기 위해
express 모듈을 사용 할 수 있다.
- 익스프레스에서 제공하는 미들웨어와 라우터를 사용하면 훨씬 편리하게 사용 가능하다.
- express 모듈은 http 모듈위에서 동작한다.

### express 서버 객체 주요 메서드
1. set(name,value) : 서버 설정을 위한 속성 지정, set()메서드로 지정한 속성은 get()메서드로 꺼내서 확인 가능
2. get(name) : 서버 설정을 위해 지정한 속성을 꺼내옴
3. use([path],function[,function...]) : 미들웨어 함수 사용
4. get([path],function) : 특정 패스로 요청된 정보 처리

<br/>

express 모듈 기본 규격
```swift
  var app = express();
  app.set('port',process.env.PORT || 3000); // 서버 설정, 
                         //process.env객체에 PORT속성이 있으면 그속성을 사용하고 없으면 3000포트 사용
  
  app.use(express.function()); // 미들웨어 사용 설정
  
  // 클라이언트 요청 처리
  app.get('/',routes.index);
  app.get('/users',user.list);
  
  //서버실행
  http.createServer(app).listen(app.get('port'),function(){
    console.log("express server on"+app.get('port'));
  });
```

### user()메서드를 사용한 미들웨어 설정
노드에서는 미들웨어를 사용하여 필요한 기능을 순차적으로 실행할 수 있다.
노드에서는 미들웨어 이외에 라우터도 사용 하는데, 미들웨어나 라우터는 하나의 독립된 기능을 가진 함수 이다.

```swift
   var express = require('express'),
   http = require('http'),
   path = require('path');
   
   var app = express();
   
   app.use(function(req,res,next){
      console.log("첫 번째 미들웨어에서 요청을 처리함");
      res.writeHead('200',{'Content-Type':'text/html;charset=utf8'});
      res.end('<h1>Express 서버에서 응답한 결과입니다.</h1>'); // 클라이언트로 보내짐
   });
   
   http.createServer(app).listen(3000,function(){
      console.log('Express 서버가 3000번 포트에서 시작됨');
   });  
```

### 여러개의 미들웨어를 등록하여 사용하는 방식  => next() 사용
```swift
   ....... // 서버 설정이  되었다고 가정
   app.use(function(req,res,next){
       console.log("첫번째 미들웨어에서 요청을 처리함");
       req.user = 'mike';  // req객체에 user 속성 추가, 
       next(); // next()를 해야 다음 미들웨어로 객체를 넘길수 있음
   });
   
   app.use('/',function(req,res,next){
       console.log("두번째 미들웨어에서 요청 처리");
       res.writeHead('200',{'Content-Type':'text/html;charset=utf8'});
       res.end('<h1>Express 서버에서'+req.user+'를 첫번째 미들웨어로 부터 받아  응답</h1>');
   });
```

### 익스프레스의 요청객체 및 응답객체
- send([body])  :  클라이언트에 응답 데이터를 보낸다. 전달할 수 있는 데이터는 html문자열, buffer객체, JSON 객체, JSON배열
- status(code) : HTTP 상태코드를 반환, 상태코드는 end()나, send()와 같은 전송메소드를 추가로 호출해야 전송할 수 있다.
- sendStatus(statusCode) : HTTP 상태코드를 반환, 상태코드는 상태 메시지와 함께 전송.
- redirect([status,]path)  : 웹 페이지 경로를 강제로 이동
- render(view[,locals],[calback]) : 뷰 엔진을 사용해 문서를 만든 후 전송
```swift
   ... // 서버 설정이 되었다고 가정
   app.use(function(req,res,next){
      res.send({name : '동준', age : 25}); // JSON 규격으로 클라이언트에 찍힘
   });
   
   app.use('/re',function(req,res,next){
      res.redirect('http://google.co.kr'); // 리다이렉트
   });
```
### 클라이언트에서 웹 서버로 파라미터를 보냈을때, 이 파라미터를 참조하는 방법
1.GET 방식으로 요청된 파라미터 참조

 http://localhost:3000/?name=dongjoon       =>   req.query.name   으로 참조가능

2. POST 방식으로  요청된 파라미터 참조 =>  req.body.name 으로 참조가능

3. URL 파라미터로 보낸것 참조     =>   req.params.name 으로 참조가능

1,2 방식은 req.param() 으로 참조 가능

### static 미들웨어
static 미들웨어는 특정 폴더의 파일들을 특정 패스로 접근할 수 있도록 만들어줌, 예를들어 [public] 폴더에 있는 모든 파일을 웹 서버의 루트패스로
접근할 수 있도록 만들고 싶으면

app.use(express.static(path.join(__dirname,'public')));  => 이렇게하면 [public] 폴더 안에 있는 파일들은 클라이언트에서 바로 접근 가능

아래와 같이 웹 브라우저에서 바로 접근 가능하다
```swift
   ExpressApp/public/index.html   =>   http://localhost:3000/index.html
   ExpressApp/public/js/main.js   =>   http://localhost:3000/js/main.js
   ExpressApp/public/images/house.png   =>   http://localhost:3000/images/house.png
```

예를 들어 public/images에 있는 house.png 이미지 파일을 웹 브라우저에서 보려면 app.js에서 아래와 같이 보내면 된다
```swift
    res.end("<img src = 'images/house.png' width = '50%'>");
```

추가로,  [public]폴더 안에 있는 파일을 사이트의 /public 패스로 접근하도록 만들고 싶으면 아래와 같이 보내면 된다.
```swift
   app.use('/public',express.static(path.join(__dirname,'public')));
   // static() 메서드로 특정 폴더를 지정하였기 때문에 요청패스와  특정 폴더가 매핑 되어 접근 가능
```

### body-parser 미들웨어 => POST로 요청했을때 요청파라미터를 확인할 수 있음
GET 방식으로 요청할 때에는 주소  문자열에 요청 파라미터가 들어간다. 하지만 POST방식은 본문인 본문영역(body 영역)에 요청파라미터가 들어가 있게 된다.
body-parser 미들웨어는 클라이언트가 POST방식으로 요청할 때 본문 영역에 있는 요청 파라미터들을 파싱하여 요청 객체와 params 속성에 넣어준다.

index.html이라고가정
```swift
    <form method="post" >
    <table>
       <tr>
          <td><label>아이디</label></td>
          <td><input type="text" name="id" /></td>
       </tr>
       <tr>
          <td><label>비밀번호</label></td>
          <td><input type="password" name="password" /></td>
       </tr>
    </table>
    <input type="submit" value="전송" name=""/>
	</form>
```

app.js 라고 가정
```swift
  ... 기타 설정이 되었다고 가정
  
   // body-parser를 이용해 application/x-www-form-urlencoded 파싱
  app.use(bodyParser.urlencoded({ extended: false }));

  // body-parser를 이용해 application/json 파싱
  app.use(bodyParser.json());
  
  // 미들웨어에서 파라미터 확인
 app.use(function(req, res, next) {
	console.log('첫번째 미들웨어에서 요청을 처리함.');

	var paramId = req.body.id || req.query.id;   // req.body.name은 Post 방식, req.query.name은 Get방식
	var paramPassword = req.body.password || req.query.password; // 둘중 하나 넘어오면 값을 할당

  // 여기서 정의한 규격만 클라이언트에 날라감 
	res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
	res.write('<h1>Express 서버에서 응답한 결과입니다.</h1>');
	res.write('<div><p>Param id : ' + paramId + '</p></div>');
	res.write('<div><p>Param password : ' + paramPassword + '</p></div>');
	res.end();  // 클라이언트로 최종 제출
});

```

### 라우터 미들웨어
사용자가 요청한 기능이 무엇인지 패스를 기준으로 구별할 수 있음
- get(path,callback)    :  GET방식으로 특정 패스 요청이 발생 했을때 사용할  콜백 함수를 지정
- post(path,callback)     :   POST방식으로 특정 패스 요청이 발생 했을때 사용할  콜백 함수를 지정
- put(path,callback)      : PUT방식으로 특정 패스 요청이 발생 했을때 사용할  콜백 함수를 지정
- delete(path,callback)     : DELETE방식으로 특정 패스 요청이 발생 했을때 사용할  콜백 함수를 지정
- all(path,callback)      :  모든 요청 방식을 처리하며, 특정 패스 요청이 발생 했을때 사용할  콜백 함수를 지정

예를 들어 클라이언트에서 GET 방식으로 요청할 때에는 익스프레스에서 get()메소드를 사용해 함수를 등록해야 하며, POST방식으로 요청할 때에는 post()메소드를 사용해서 등록 해야함   => 각기 다 맞춰야함





