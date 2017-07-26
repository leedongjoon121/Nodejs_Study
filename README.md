
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



