## Section 5 - Express.js

### Expres.js란?

Node.js를 위한 웹 애플리케이션 프레임워크이다. 전 세계 node.js 프레임워크로 1위를 지키고 있으며 코드 및 구조의 통일성을 향상시킬 수 있었다. 익스프레스는 개발자들이 일부 필수적인 작업과 신경 쓰고 싶지 않은 세부 내용을 외부에 맡길 수 있도록 도와주고 핵심적인 일에 집중하게 도와주는 유틸리티 함수를 다수 제공한다.

### Express의 특징

- 가볍게 웹 서버를 구현할 수 있다.

```jsx
import express from "express";

const app = express();

app.get('/', (req, res) => {
	res.send('hello, world!');
});

app.listen(3000);
```

기존의 다른 웹 프레임워크와는 다르게 Express는 서버를 아주 간단하게 구현할 수 있다는 특징이 있다.

- 미들웨어

### 미들웨어란?

클라이언트에게 요청이 오고 그 요청을 보내기 위해 응답하려는 중간(미들)에 목적에 맞게 처리를 하는 (거쳐가는) 메소드를 의미한다.

next()메소드를 통해 다음 미들웨어를 호출할 수 있다.

use 메소드를 사용하면 새로운 미들웨어 함수를 추가할 수 있다.

send 메소드를 사용하면 응답을 보낼 수 있고 any 유형의 본문을 첨부할 수 있다.

## 미들웨어의 유형

### Application 레벨 미들웨어

```jsx
...
const app = express();

app.use((req, res, next) => {
  console.log('In the middleware!');
  next(); //다음 라인에 있는 미들웨어로 요청이 이동할 수 있게 함
});
```

app.use() 및 app.METHOD() (method: get, post 등)을 이용해 미들웨어를 Application 영역에서 지정한 path대로 처리할 수 있게 해준다.

```jsx
// 모든 요청에 대해 작동

app.use(function (req, res, next) {
  console.log("모든 요청에서 다 실행됨");
  next();
});

//  GET / 요청에 대해서만 응답
app.get('/',function (req, res, next) {
  res.send('USER');
});
```

### 라우터 레벨 미들웨어

라우터는 클라이언트의 요청 경로(path)를 보고 이 요청을 처리할 수 있는 곳으로 기능을 전달해주는 역할을 한다.

```jsx
// /admin/add-product => GET
router.get('/add-product',(req, res, next) => {
  res.sendFile(path.join(rootDir, 'views', 'add-product.html'));
});

// /admin/add-product => POST
// POST요청의 경우만 실행되고 GET 요청에는 반응하지 않음.
router.post('/add-product', (req, res, next) => { 
  console.log(req.body);
  res.redirect('/');
});
```

메소드가 다르면 같은 경로를 사용해도 문제가 없다는 것을 알 수 있다.

### 에러 핸들링 미들웨어

에러를 처리하는 미들웨어는 다음 4개의 인자를 사용해서 정의한다.

```jsx
//에러 처리 미들웨어
app.use((err, req, res, next) =>
	console.error(err);
res.status(500).send(err.message);
});
```

### 페이지를 찾을 수 없을 때

```jsx
app.use((req, res, next) => {
  res.status(404).send("<h1>Page not found!</h1>");
});
```

처리할 수 없는 요청이 들어오면 오류가 발생하는데 이런 경우에는 404 error 페이지를 나타내기 위해 미들웨어를 사용해 요청을 처리하는 방법을 활용한다.

기본 응답 헤더는 ‘text/html’이며, setHeader를 호출하여 응답 헤더의 타입을 지정할 수 있다.