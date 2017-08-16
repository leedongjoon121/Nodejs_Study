# 몽고디비
> 몽고디비는 기존에 자주사용하던 관계형 데이터베이스(Relational Database)와 달라 SQL을 사용하지 않는다. 또한 자바스크립트 객체를 그대로 저장할 수 있어서
  데이터 조회 방식도 SQL과 다르다. 
  몽고디비는 비관계형 데이터베이스라고도 불리며, NoSQL 또는 Not Only SQL 이라고도 한다. 관계형 데이터베이스는 시스템의 신뢰도를 높이는데 필요한 장치를 많은 장치를 가지고 있으며, SQL문을 읽어 들이고 실행하는데 많은 리소스를 필요로하며 이때문에 성능이 떨어지는 경우가 많다.
  
  <br/>
  
  이에 반해 NoSQL 데이터베이스는 성능을 최우선으로 생각하기 때문에 실시간으로 처리해야하는 경우나 대용량 트래픽을 감당할 수 있는 메시징 시스템등에 활용된다.
  특히 클라우드 서비스로 서버를 구성하는 경우가 많아지면서, 많은 사용자를 수용하거나 시스템 자원을 적게 소모하는 NoSQL 데이터베이스에 점점 더 관심을 갖게됨
  
  > 몽고디비는 NoSQL이기때문에 데이터베이스 테이블 개념이 없고, 여러 데이터가 모인 하나의 단위를 "컬렉션(Collection)"이라고 부른다. 몽고디비는 데이터 저장소를 가지고 있으며 그 안에 여러 컬렉션을 넣을 수 있다. 각 컬렉션은 여러개의 문서객체(document)를 가질 수 있다.

## 기본용법

### 데이터베이스 지정
use db명   (ex,  use person)
### 데이터베이스 목록 보기 
show dbs
### 데이터베이스 삭제  : 반드시 데이터베이스가 선택되어 있어져야 한다 (use 명령어)
db.dropDatabase()
### 데이터베이스는 db라는 이름으로 접근할 수 있고, 데이터베이스 안에 컬렉션을 만들고 그 안에서 문서를 저장할 수 있다.

<br/>

### 컬렉션 생성  : 하지만 이렇게 직접 컬렉션을 만들지 않고 => db.컬렉션명.명령어 형식으로 바로 사용가능
db.createCollection(name,[options])
### 컬렉션 제거 
db.컬렉션이름.drop()
### 컬렉션 목록 확인 : show collections

<br/>

## document 명령어
### 1.insert명령
db.users.insert({"name":"홍길동","age":25});        : 컬렉션을 별도로 만들지 않고 자동으로 만들어짐
> 배열의 형태로 넣고 싶을땐, db.users.insert([{"name":"김길동"},{"name":"박길동"}]);
### 2.find명령(찾기)
db.users.find().pretty()           : users컬렉션에 있는 모든 문서 객체들을 반환함, pretty() 예쁘게 정렬해서 보여줌
### 3.remove 명령
db.users.remove({"name":"홍길동"})     => 이름이 홍길동인거 삭제

## 몽고디비 간단한 예제
### <a href = "https://github.com/leedongjoon121/Nodejs_Study/tree/mongodb_ex">예제 브랜치</a>

### index.html 
- 몽고디비에 입력, 출력, 수정, 삭제 등을 할 수 있는 예제
## ![사진](https://github.com/leedongjoon121/Nodejs_Study/blob/document_img_file/project_img_file/mongoDB_main.png?raw=true)


### mongoDB_data
- 몽고디비 데이터
## ![사진](https://github.com/leedongjoon121/Nodejs_Study/blob/document_img_file/project_img_file/mongoDB_data.png?raw=true)
