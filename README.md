# Nodejs의 원리부터 깊이있게 다시한번 공부하며 정리
- 막연하게 사용하였던 Nodejs에 관해서 심도있는 학습을 하며 내용정리 & 간단한 프로젝트 업로드 예정
- 2017.7.25개설
- 간단한 예제 파일은 각 branch별 관리
- 각 branch와 master는 연관(상관)이 없습니다. 별도의 내용을 정리한 저장소로 활용합니다.

### name :  Dongjoonlee 
### nation : south korea
### date of birth : 1993.04.06
### univ : gachon university
### email : ehdwns46@naver.com

<hr/>
<br/>

## 목차

* [1. 노드기본](#노드기본)
* [2. 노드의기본기능](#노드의기본기능)
* [3. 이벤트기본(EventEmitter)](#이벤트기본)
* [4. 파일다루기(fs)](#파일다루기)
* [5. 웹서버기본](#웹서버)
* [6. 웹클라이언트요청](#웹클라이언트요청)


<br/>

# 노드

## 1.노드의 비동기 입출력 방식
노드는 하나의 요청처리가 끝날때까지 기다리지않고 다른 요청을 동시에 처리할 수 있는 비동기 입출력(논블로킹 입출력 방식)이다
- 파일 시스템에 읽기 요청을 한 후에 프로그램이 대기하지 않고 다른 작업을 진행 한다.
- 프로그램에서 해당 파일의 내용을 처리할 수 있는 시점이 되면 콜백함수가 호출된다.
- 파일시스템은 파일처리가 끝나면 자동으로 콜백 함수를 호출한다. 

## 콜백함수
자바스크립트에서는 변수에 함수를 할당할 수 있다. (1급 객체의 특성) 따라서 변수에 할당된 함수를 다른 함수의 파라미터로 전달할 수 있다.
이렇게 파라미터로 전달된 함수를 다른 함수의 내부에서 호출하는 것이 콜백 함수를 뜻한다.

## 이벤트 기반 입출력 모델
- 파일 읽기가 완료 되었을 때, 파일 시스템에서 콜백 함수를 호출하는데, 파일 시스템이 이벤트와 함께 호출하는 방식이면 이벤트 기반 입출력 모델이라 부름
- 파일 시스템에서 파일 읽기가 완료 되었다는 이벤트만 전달하면 프로그램에서는 그 이벤트를 받아 콜백함수를 실행할 수 있으며, 콜백 함수는 이벤트가 발생하기
전에 미리 등록 한다.

<br/>

## http 프로토콜을 이용한 데이터 송수신 개념 => 추후 업로드를 통해 더 깊이 있게 들어갈 예정

```swift
   http.request(options,function(res){  // 외부 웹서버에 요청
     res.on('data',function(chunk){   // 외부 웹서버로 부터 응답
         console.log('body:'+chunk); // 응답 전송
     });
   });
```

- http객체는 http 프로토콜로 웹서버에 데이터를 요청할 수 있는 기능이 있다.
- request()함수를 호출하여 웹서버에 데이터를 요청할 수 있고, 응답을 받으면 자동으로 콜백함수가 호출 된다.
- 이때 응답을 처리할 수 있는 콜백함수는 res라는 이름의 객체를 전달 받는다.

> 자바스크립트에서는 on() 메서드를 사용해 이벤트를 콜백함수와 바인딩 할 수 있다.
  바인딩은 [객체].on([이벤트이름],[함수객체]); 의 형태로  => res.on('data',function(){...});
  
<br/>

## 노드의  모듈 사용 방식
노드에서는 필요한 기능을 별도의 자바스크립트로 만든후 필요할 때마다 불러올 수 있다. 다만 이 형태는 CommonJS 표준 스펙을 따른다.
- CommonJS는 자바스크립트를 브라우저뿐만 아니라 서버쪽 프로그램이나 PC용 프로그램에서도 사용하려고 조직한 자발적인 워킹 그룹이다
- CommonJS 표준스펙은 자바스크립트 코드를 별도의 모듈 파일로 분리시키고 필요할 때 불러와 사용할 수 있는 방식을 정의해둔 표준.

```swift
  // 메인 코드라고 가정하고 이렇게 모듈화 시켜놓은 자바스크립트를 불러와 사용 가능하다
  var module1 = require('./module1');
  var module2 = require('./module2');
  var module3 = require('./module3');
  
  module1.함수; // 사용자 정의에 따른 동작
  module1.변수;
```

노드에서는 이렇게 공통으로 사용하는 기능들은 모듈로 분리하여 구성하는 것이 일반적이며, 또 여러개의 모듈을 합쳐서 패키지로 만들어두면
다른 프로그래머들도 npm 프로그램으로 쉽게 패키지를 설치하여 사용 가능하다.

<br/>

## 모듈 분리하기의 예 (exports 전역객체)
노드는 CommonJS의 표준스펙을 따라 모듈을 사용할 수 있으며, 이 과정에서 exports 전역 객체를 사용 한다.
### 모듈을 생성 할때 : exports 객체의 속성으로 변수나 함수를 지정하면 다른 자바스크립트 파일에서 불러와 사용 가능
### 모듈을 불러올 때 : require()메소드를 사용하며, 모듈로 만들어 둔 파일의 이름을 메소드의 파라미터로 전달
exports 뿐만 아니라 module.exports를 사용할 수도 있다. module.exports는 일반적으로 객체를 그대로 할당 하며, 이렇게 할당한 객체 안에 넣어둔
변수나 함수를 메인 파일에서 사용 가능

```swift
   // exports 
   exports.add = function(a,b){
      return a+b;
   };
   
   // module.exports
   var calc = {}; // 리터럴
    calc.add = function(a,b){
      return a+b;
    };
    calc.mul = function(a,b){
      return a*b;
    };
    module.exports = calc;
```

## npm
Node Package Manager의 약자로 노드의 패키지를 사용할 수 있도록 설치 및 삭제 등을 지원하는 프로그램 

# 노드의기본기능

## 주소 문자열과 요청 파라미터
웹사이트에 접속하기 위한 주소 정보는 노드에서 URL 객체로 만들수 있다.
- url 모듈을 이용해 주소 문자열을 객체로 만들면 문자열 안에 있던 각각의 정보를 나누어 그 객체의 속성으로 보관한다.

### 주요메서드
1. parse() : 주소 문자열을 파싱하여 url객체 만들어줌
2. format() : url 객체를 주소 문자열로 변환함

```swift
  var url = require('url');
  var querystr = require('querystring');
  
  var urlObj = url.parse('https://www.~~~~~'); // 주소 문자열 -> url 객체
  console.log(urlObj);  // 객체 타입으로 찍힘
  var urlStr = url.format(urlObj);
  console.log('주소문자열 : %s',urlStr); // 주소 문자열로 찍힘
  
  // querystring 모듈을 이용해 더 쉽게 분리
  /*
    1. parse() : 요청 파라미터 문자열을 파싱하여 요청 파라미터 객체로 만들어줌
    2. stringify() : 요청 파라미터 객체를 문자열로 변환

  var param = querystr.parse(urlObj.query);
  console.log('요청 파라미터 중 query값 : %s',param.query);
  console.log('원본 요청 파라미터 : %s', querystr.stringify(param));
  */
```

# 이벤트기본
노드는 대부분 이벤트를 기반으로 하는 비동기 방식으로 처리 한다. 그리고 비동기 방식으로 처리 하기 위해 서로 이벤트를 전달.
노드에는 이벤트를 보내고 받을 수 있도록 EventEmitter라는 것이 만들어져 있다.

## 이벤트 주고 받는 과정
1. EventEitter 상속 -> 2.리스너 등록(on()) -> 3.이벤트전달(emit()) -> 4. 리스너함수호출(이벤트리스너)

## EventEmitter
1. on(event,listener) : 지정한 이벤트의 리스너를 추가
2. once(event,listener) : 지정한 리스너를 추가하지만 한번 실행한 후 자동 제거
3. removeListener(event,listener) : 지정한 이벤트에 대한 리스너를 제거

## 간단한 계산기 예제로 확인
calfunction.js 라고 가정

```swift
   var util = require('util');
   var EventEmitter = require('events').EventEmitter;    // EventEmitter는 event 모듈안에 정의되어 있다.

   
   var Calc = function(){
     var self = this;
     
     this.on('stop',function(){ // emit을 보낸 이벤트에 대한 리스너로서의 기능을 수행 할 수 있다.
                                // EventEmitter를 상속 받아서 on 메서드 사용 가능함
       console.log('Calc에 stop event 전달됨.');
     });
   };
   
   util.inherits(Calc,EventEmitter); // Calc 객체가 이벤트처리를 할 수 있도록 EventEmitter를 상속하도록 만듬
   
   Calc.prototype.add = function(a,b){ // Calc 객체에 add 함수추가
     return a+b;
   }
   
   module.exports = Calc; // 모듈을 불러들이는 쪽에서 Calc 객체 참조
   module.exports.title = 'calculator'; // title속성값으로 string 할당
```

main.js라고가정
```swift
   var Calcfunc = require('./calfunction'); // calfunction의 Calc를 사용가능
   var cal = new Calcfunc(); // 인스턴스 생성
     cal.emit('stop'); // Calc 객체가 EventEmitter를 상속하므로 인스턴스 객체의 emit()메소드 호출하여 stop이벤트 전달
   
   console.log(Calcfunc.title + '에 stop 이벤트 전달함');
```

### 결과
Calc에 stop event 전달됨.
calculator에 stop 이벤트 전달함.

<br/>

# 파일다루기
### 노드의 파일 시스템은 파일을 다루는 기능과 디렉터리를 다루는 기능으로 구성되어 있다.
### 동기식 IO 방식과 비동기식 IO 기능을 함께 제공한다.
- 동기식 IO는 파일 작업이 끝날때 까지 대기, 비동기식 IO는 파일 작업을 요청만 하고 그 다음작업을 바로 수행
- 동기식 IO와 비동기식 IO를 구분하기 위해 동기식 IO는 Sync라는 단어를 붙임

동기식
```swift
    var fs = require('fs');
    var data = fs.readFileSync('./README.md','utf8'); // 파일을 동기식으로 읽음
    console.log(data); // 읽어들인 데이터 출력
```

비동기식
```swift
  var fs = require('fs');
  fs.readFile('./README.md','utf8',function(err,data){ // 파일을 비동기식으로 읽음
     console.log(data); // 읽은 데이터 출력
  });
```

보통 비동기식 방식을 더 많이 쓴다.

### fs모듈 메서드정리
1. readFile(filename,[encoding],[callback])   : 비동기식 IO로 파일을 읽어옴
2. readFileSync(filename,[encoding])   : 동기식 IO로 파일을 읽어옴
3. writeFile(filename,encoding='utf8',[callback])   : 비동기식 IO로 파일을 작성
4. writeFileSync(filename,data,encoding='utf8')   : 동기식 IO로 파일을 작성

```swift
  var = require('fs');
  fs.writeFile('./text.txt','HelloWorld',function(err){ // 파일에 데이터  쓰기
    if(err){
       console.log(err);
    }
  });
```

### 스트림 단위로 파일 읽고 쓰기
파일을 읽거나 쓸때 데이터 단위가 아니라 스트림 단위로 처리 할 수도 있다.
1. createReadStream(path[,options])    : 파일을 읽기 위한 스트림 객체를 만듬
2. createWriteStream(path[,options])     : 파일을 쓰기 위한 스트림 객체를 만듬
옵션으로는 flags,encoding,autoClose 속성이 들어 있는 자바스크립트 객체를 전달 할 수 있다.

```swift
  var fs =require('fs');
  var inname = './output.txt';  
  var outname = './output2.txt';
  
  fs.exists(outname,function(exists){ // 기존에 output2.txt가 존재하면 제거
    if(exists){ // 존재하면
      fs.unlink(outname, function(err){ // 이전 파일삭제
        if(err) throw err;
        console.log('기존파일 ['+outname+'] 삭제함');
      });
    }
    
    var infile = fs.createReadStream(inname, {flags : 'r'}); 
    var outfile = fs.createWriteStream(outname, {flags : 'w'});
    
    infile.pipe(outfile); // pipe() 메서드로 두개의 스트림을 붙임
                          // WriteStream 타입 객체와 ReadStream 타입의 객체를 붙여주면 스트림간에 데이터를 알아서 전달함
    console.log('파일복사['+inname+'] -> ['+outname+']');
  });
  
```

### http 모듈로 요청받은 파일 내용을 읽고 응답
```swift
   var fs = require('fs');
   var http = require('http');
   
   var server = http.createServer(function(req,res){
      var instream = fs.createReadStream('./output.txt'); // 파일을 읽어 응답 스트림과 pipe로 연결
      instream.pipe(res); // 파일에서만든 스트림객체와 웹서버에서 만든 스트림객체를 pipe()로 연결
});
   server.listen(3000,'127.0.0.1');
```

<br/>

# 웹서버
### http모듈을 활용한 웹 서버
http모듈은 노드에 기본으로 내장되어 있으며, http 모듈을 로딩했을때 반환되는 객체에는 createserver()메서드가 정의 되어 있다.
```swift
   var http = require('http');
   var server = http.createServer();
   
   server.listen(3000,function(){
     console.log("server port is 3000 ");
   });
   
   /*
   // 특정 ip를 지정하여 서버를 실행할 때
    server.listen(3000,'192.168.0.5','50000',function(){
        console.log("포트 : 3000, host : 192.168.0.5, backlog : 50000");
    });
   */
```

### 클라이언트가 웹서버에 요청할때 발생하는 이벤트 처리
1. connection : 클라이언트가 접속하여 연결이 만들어질 때 발생하는 이벤트
2. request : 클라이언트가 요청할 때 발생하는 이벤트
3. close : 서버를 종료할 때 발생하는 이벤트 

```swift
  var http = request('http');
  var server = http.createServer();
  
  server.listen(3000,function(){ 
     console.log("웹 서버 켜짐, 포트는 3000");
  });
  
  
  // 웹 브라우저와 같은 클라이언트가 웹 서버에 연결되면 connection 이벤트 발생
  server.on('connection',function(socket){ // socket에는 클라이언트 연결정보가 담겨있음
   var addr = socket.address(); // 클라이언트의 ip와 port를 알 수 있음
     console.log('클라이언트가 접속 했음 : %s, %d',addr.address, addr.port);
  });
  
  
  // 클라이언트가 특정 패스로 요청하면 request 이벤트 발생
  server.on('request',function(req,res){
     console.log("클라이언트 요청이 들어옴");
     console.dir(req);
     
     res.writeHead(200, {"Content-Type": "text/html; charset=utf-8"}); // 응답으로 보낼 헤더를 만듬
     res.write("<!DOCTYPE html>");
     res.write("<html>");
     res.write("  <head>");
     res.write("    <title>응답페이지</title>");
     res.write("  </head>");
     res.write("  <body>");
     res.write("    <h1>노드제이에스로부터의 응답 페이지</h1>");
     res.write("  </body>");
     res.write("</html>");
     res.end(); // 응답을 모두보냈다. end()메서드가 호출될 때 클라이언트로 응답을 전송
  });
  
  server.on('close',function(){
    console.log("서버가 종료");
  });
```

request 관련 메서드
1. writeHead : 응답으로 보낼 헤더를 만듬
2. write : 응답 본문(body)데이터를 만듬, 여러번 호출 가능
3. end : 클라이언트로 응답을 전송, 파라미터에 데이터가 들어 있으면 데이터를 포함해서 응답 전송

<br/>

# 웹클라이언트요청
http 모듈을 이용하여 클라이언트의 기능을 활용하고, http 클라이언트가 get과 post방식으로 다른 웹 서버에 데이터를 요청
### get 방식

```swift
  var http  = require('http');
  var options = {
        host : 'www.google.com',
        port : 80,
        path : '/'
      };
  var req = http.get(options,function(res){
  //응답처리
  var resData = '';
  res.on('data',function(chunk){
    resData += chunk;
  });
  
  res.on('end',function(){
    console.log(resData);
  });
  });
  
  req.on('error',function(err){
    console.log("오류발생 : "+ err.message);
  });
```
http 객체의 get() 메서드를 사용하면 다른 사이트에 요청을 보내고 응답을 받아 처리할 수 있다.
다른 서버로 부터 응답을 받을 때는 data이벤트가 발생한다.

### post 방식
post 방식으로 요청할 경우에는 request()메소드를 사용한다.
post 방식으로 요청을 보낼 때에는 요청 헤더와 본문을 모두 직접 설정해야 한다.
```swift
  var http = require('http');
  var options = {
      host : 'www.google.com',
      port : 80,
      method : 'POST',
      path : '/',
      headers : {}
  }; // 물론 구글사이트는 post요청을 받지못한다, post메서드로 요청할 수있는 다른사이트에서는 정상작동한다.
  
  var resData = '';
  var req = http.request(options,function(){
   // 응답 처리 
   res.on('data',function(chunk){
     resData += chunk;
   });
   
   res.on('end',function(){
     console.log(resData);
   });
  });
  
  options.headers['Content-Type'] = 'application/x-www-form-urlencoded';
  req.data = "q=actor";
  options.headers['Content-Length'] = req.data.length;
  
  req.on('error',function(err){
    console.log("오류발생"+err.message);
  });
  
  //요청전송
  req.write(req.data);
  req.end();
  };
```
