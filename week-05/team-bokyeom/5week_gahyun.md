# Week5

# 동적 웹과 정적 웹

### 정적 웹이란?

움직이지 않는, 언제 접속을 해도 같은 리소스를 주는 웹 사이트를 칭하는 말로 이미 파일들을 클라이언트 브라우저에 전달되어 서버에서 접속시 매번 가공해서 제공하는 것이 아닌 html css javascript 코드들이 그대로 전달되는 것

ex) 소개 페이지, 댓글 없는 블로그글

→ 서비스가 한정적이지만 요청에 대한 파일만 전송하면 되기에 서버간 통신이 거의 없고 속도가 빠르다는 장점이 있다

### 동적 웹이란?

섭에서 접속시 매번 가공해서 제공하는 웹사이트로 데이터베이스에서 값을 읽어 접속할때마다 최신 정보를 주는것

ex) sns

→ 사용자에게 웹 페이지를 전달하기 전에 처리하는 작업이 필요하기에 상대적으로 느리짐나 다양한 서비스를 제공하고 관리가 쉬움

[https://velog.io/@pluviabc1/정적-웹과-동적-웹은-무엇인가요-얄팍한-코딩-사전](https://velog.io/@pluviabc1/%EC%A0%95%EC%A0%81-%EC%9B%B9%EA%B3%BC-%EB%8F%99%EC%A0%81-%EC%9B%B9%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-%EC%96%84%ED%8C%8D%ED%95%9C-%EC%BD%94%EB%94%A9-%EC%82%AC%EC%A0%84)

# 쿼리스트링

쿼리스트링은 어떤 애플리케이션에게 정보를 전달할 때 사용되는 URL에 약속되어 있는 국제적인 표준

쿼리스트링은 URLL에서 물음표 다음에 오는 것들이라고 생각할 수 있다 또한 쿼리스트링은 key = value 형태라고 보면 된다. 

ex)

```java
https://www.inflearn.com/courses?order=seq&skill=python
```

이런 url이면 order가 key고 seq는 value라고 볼 수 있다 또한 쿼리스트링을 여러 개 보내고 싶으면 앤드 연산자를 붙여서 추가할 수 있다.

[https://velog.io/@gth1123/쿼리-스트링Query-string-URL-파라미터](https://velog.io/@gth1123/%EC%BF%BC%EB%A6%AC-%EC%8A%A4%ED%8A%B8%EB%A7%81Query-string-URL-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0)

# 동적 세그먼트

우리가 상품조회 같은 걸 한다면 어떻게 파라미터 값에 따라 웹페이지를 출력해줄 수 있을까?

data를 저장할 때 해당 id값을 설정해준 후 클라이언트가 특정 상품을 클릭하면 해당 상품의 id값이 url 파라미터로 전달된다. 백엔드에서는 전달받은 파라미터에 해당하는 상품을 데이터 영역에서 찾고 그 상품과 관련된 정보를 전송해준다.

### url 파라미터

```java
"/products/:productId"
```

우리는 url의 /:productId 이 부분을 파라미터라고 한다.

콜론은 express에게 productId와 같은 라우트를 탐색하지 말라는 신호를 보냄. 이 경로에 대해 이 라우트를 불러오고 이름을 통해 정보 추출을 하게 되어있음

이걸 이용해서 url안에 변수를 포함시키는 것이다.

### req.params

라우터의 매개변수

```java
router.get('/:id/:name',(req,res,next)=>{
console.log(req.params);
});
```

만약 이런 코드의 예시가 있다면 req.params를 출력했을 때 실제 요청온 url이 요청온 url : [www.example.com/public/100/jun](http://www.example.com/public/100/jun)라면  {id : ‘100’, name’jun’} 이 출력될것이다. 

req객체에 parameter라는 프로퍼티가 있고 그 프로퍼티의 ‘id’라는 프로퍼티로 접근해 요청을 보낼 수 있는 것이다. 

### req.query

경로의 각 쿼리 문자열 매개변수에 대한 속성이 포함된 개체다 (주로 get 요청에 대한 처리)

예를 들어 [www.example.com/post/1/jun?title=hello](http://www.example.com/post/1/jun?title=hello)! 이면, title=hello! 부분을 객체로 매개변수의 값을 가져온다

```java
// 요청온 url : www.example.com/public/100/jun?title=hello!
app.use(express.urlencoded({ extended: false })); // uri 방식 폼 요청 들어오면 파싱

router.get('/:id/:name', (req, res, next) => {

  //../100/jun 부분이 담기게 된다.
  console.log(req.params) // { id: '100', name: 'jun' }
  
  // title=hello! 부분이 담기게 된다.
  console.log(req.query) // { title : 'hello!' }
});
```