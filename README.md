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


<br/>

# 노드

### 1.노드의 비동기 입출력 방식
노드는 하나의 요청처리가 끝날때까지 기다리지않고 다른 요청을 동시에 처리할 수 있는 비동기 입출력(논블로킹 입출력 방식)이다
- 파일 시스템에 읽기 요청을 한 후에 프로그램이 대기하지 않고 다른 작업을 진행 한다.
- 프로그램에서 해당 파일의 내용을 처리할 수 있는 시점이 되면 콜백함수가 호출된다.
- 파일시스템은 파일처리가 끝나면 자동으로 콜백 함수를 호출한다. 

### 콜백함수
자바스크립트에서는 변수에 함수를 할당할 수 있다. (1급 객체의 특성) 따라서 변수에 할당된 함수를 다른 함수의 파라미터로 전달할 수 있다.
이렇게 파라미터로 전달된 함수를 다른 함수의 내부에서 호출하는 것이 콜백 함수를 뜻한다.

### 이벤트 기반 입출력 모델
- 파일 읽기가 완료 되었을 때, 파일 시스템에서 콜백 함수를 호출하는데, 파일 시스템이 이벤트와 함께 호출하는 방식이면 이벤트 기반 입출력 모델이라 부름
- 파일 시스템에서 파일 읽기가 완료 되었다는 이벤트만 전달하면 프로그램에서는 그 이벤트를 받아 콜백함수를 실행할 수 있으며, 콜백 함수는 이벤트가 발생하기
전에 미리 등록 한다.

<br/>

### http 프로토콜을 이용한 데이터 송수신 개념 => 추후 업로드를 통해 더 깊이 있게 들어갈 예정

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

### 노드의  모듈 사용 방식
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

### 모듈 분리하기의 예 (exports 전역객체)
노드는 CommonJS의 표준스펙을 따라 모듈을 사용할 수 있으며, 이 과정에서 exports 전역 객체를 사용 한다.
#### 모듈을 생성 할때 : exports 객체의 속성으로 변수나 함수를 지정하면 다른 자바스크립트 파일에서 불러와 사용 가능
#### 모듈을 불러올 때 : require()메소드를 사용하며, 모듈로 만들어 둔 파일의 이름을 메소드의 파라미터로 전달
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

### npm
Node Package Manager의 약자로 노드의 패키지를 사용할 수 있도록 설치 및 삭제 등을 지원하는 프로그램 

# 노드의기본기능

### 주소 문자열과 요청 파라미터
웹사이트에 접속하기 위한 주소 정보는 노드에서 URL 객체로 만들수 있다.
- url 모듈을 이용해 주소 문자열을 객체로 만들면 문자열 안에 있던 각각의 정보를 나누어 그 객체의 속성으로 보관한다.

#### 주요메서드
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
### 이벤트 주고 받는 과정
1. EventEitter 상속 -> 2.리스너 등록(on()) -> 3.이벤트전달(emit()) -> 4. 리스너함수호출(이벤트리스너)

### EventEmitter
1. on(event,listener) : 지정한 이벤트의 리스너를 추가
2. once(event,listener) : 지정한 리스너를 추가하지만 한번 실행한 후 자동 제거
3. removeListener(event,listener) : 지정한 이벤트에 대한 리스너를 제거

### 간단한 계산기 예제로 확인
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

#### 결과
Calc에 stop event 전달됨.
calculator에 stop 이벤트 전달함.

<br/>

# 파일다루기
#### 노드의 파일 시스템은 파일을 다루는 기능과 디렉터리를 다루는 기능으로 구성되어 있다.
#### 동기식 IO 방식과 비동기식 IO 기능을 함께 제공한다.
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

#### fs모듈 메서드정리
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

#### 스트림 단위로 파일 읽고 쓰기
파일을 읽거나 쓸때 데이터 단위가 아니라 스트림 단위로 처리 할 수도 있다.
1. createReadStream(path[,options])    : 파일을 읽기 위한 스트림 객체를 만듬
2. createWriteStream(path[,options])     : 파일을 쓰기 위한 스트림 객체를 만듬
옵션으로는 flags,encoding,autoClose 속성이 들어 있는 자바스크립트 객체를 전달 할 수 있다.
