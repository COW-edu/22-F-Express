### Express.js

서버 로직은 매우 복잡한데, 우리는 비즈니스 로직과 같은 애플리케이션을 정의하는 코드에 집중하고 싶다!

Express.js는 비즈니스 처리에 집중하는데 도움을 주는 프레임워크이다.

```jsx
const express = require("express");

const app = express();

const server = http.createServer(app);
```

- 상수 `app` 에 express 프레임워크의 로직이 담김.
- `app` 은 유요한 요청 핸들러이기 때문에 `app` 을 사용해 서버 생성 가능

### Middleware

**Middleware**

서버에 들어오는 요청을 express.js의 다양한 함수를 통해 자동으로 이동하게 한다.

하나의 함수에서 모든 걸 처리하게 하지 않고 분리한다.

```jsx
app.use((req, res, next) => {
  console.log("In the middleware1!");
  next();
});

app.use((req, res, next) => {
  console.log("In the middleware2!");
});
```

- `(req, res, next) ⇒ {};` 의 형태를 가진 함수는 모두 middleware로 사용 가능하다.
- 파라미터로 받은 `next`는 다음 middleware에 대한 엑세스 권한을 갖는 함수이며, 다음 middleware로 이동하는 역할을 한다.

**Middleware 동작 원리**

![image](https://user-images.githubusercontent.com/106325839/191742334-295f3971-dfce-4b95-ae7c-b93b081f6584.png)

Middleware는 요청이 들어온 순간부터 순차적으로 실행되어 응답이 마무리 될 때까지 동작 사이클이 실행된다.

**미들웨어를 활용한 제어**

미들웨어를 활용해 무엇이 표시되는지 제어할 때는 `next()` 의 호출 여부 뿐만 아니라, 미들웨어의 순서도 중요하다. → Node.js 는 스크립트를 위에서 아래로 읽기 때문

```jsx
app.use("/", (req, res, next) => {
  console.log("a");
  next();
});

app.use("/add-product", (req, res, next) => {
  console.log("In another Middleware1");
  res.send('<h1>The "Add-Product" Page</h1>');
});

app.use("/", (req, res, next) => {
  console.log("In another Middleware2");
  res.send("<h1>Hello from Express!</h1>");
});

// Q1. localhost:3000 -> localhost:3000/add-product 어떤 화면이 나오고, 콘솔창은 어떻게 출력이 될까?
```

**요청 분석**

기본적으로 request는 들어오는 request body에 대한 내용을 분석하려 하지 않는다.

→ request body 분석을 위한 middleware 추가 구현

```jsx
//urlencoded()가 미들웨어 함수 포함, next()실행
app.use(express.urlencoded({ extended: false }));

app.use("/", (req, res, next) => {
  console.log("a");
  next();
});
```

- 일반적으로 경로 처리 미들웨어 전에 둔다. → 요청이 어느 경로로 향하든 request body 분석이 이뤄지도록 함.
- `{extended : false}`: 비표준 request body 분석 여부
- body-parser 가 수행. (express 4.x.x 부터 내장으로 body-parser 장착)

### 미들웨어 실행 제한

현재 `app.use()` 는 POST 요청 뿐만 아니라 GET요청에 대해서도 실행된다.

- `app.get()`
  - `app.use()` 와 기본적으로 동일하지만, GET 요청에만 작동
  - GET 요청만 확인하는 것이 아니라 정확한 경로인지도 확인
- `app.post()`
  - `app.use()` 와 기본적으로 동일하지만, POST 요청에만 작동
  - POST 요청만 확인하는 것이 아니라 정확한 경로인지도 확인

### Express 라우팅

- 모든 코드를 하나의 app.js 파일에 작성하는 것은 좋지않다
  - 파일 크기 증가, 하나의 파일에 여러 로직이 들어가면 유지 보수에 좋지 않음
- routes 폴더에 라우팅 관련 코드를 작성한 후 저장
- 라우팅 관련 코드에서 export
  ```jsx
  const router = express.Router();

  app.use("/add-product", (req, res, next) => {
    console.log("b");
    res.send("<form></form>");
  });

  module.exports = router;
  ```
- app.js에서 import
  ```jsx
  import adminRoutes from ("./routes/admin");

  app.use(adminRoutes);
  ```
- 기존 app.js파일 하나에서 미들웨어를 제어할 때와 마찬가지로 순서가 중요하다.

### 오류 페이지

요청처리(미들웨어 실행)는 작성 순서대로 처리하기 때문에 요청을 처리할 미들웨어가 없다면 요청이 처리되지 않는다.

→ 오류 페이지 전송은 맨 아래 모든 요청을 처리하는 미들웨어를 추가

```jsx
app.use((req, res, next) => {
  res.status(404).send("<h1>Page not found</h1>");
});
```

- `status()`:상태코드 설정

### 경로 필터링

일반적으로 라우터파일 속 라우트(미들웨어)들은 시작 경로를 공유하는 경우가 많다.

```jsx
router.get("/admin/add-product", (req, res, next) => {
  console.log("a");
  next();
});

app.use("/admin/add-product", (req, res, next) => {
  console.log("b");
  res.send("<form></form>");
});
```

- 공통되는 라우트 경로를 빼서 app.js에서 필터를 할 수 있다.
  `app.use("/admin", adminRoutes);`

### HTML 서비스

- `res.sendFile()` : 파일을 회신함, 콘텐츠 타입을 자동으로 응답 헤더로 설정

**경로 설정**

- 상대경로 설정을 위해 Node.js에서 제공하는 path모듈을 사용
  - `__dirname` : 절대경로를 프로젝트 폴더로 고정
  - 쉼표로 조합하면서 최종 파일 경로 생성
  ```jsx
  res.sendFile(path.join(__dirname, "../", "views", "shop.html"));
  ```
  - `path.join()` : 다른 파일 시스템 경로를 운영체제 맞게 변환

**헬퍼함수**

매번 귀찮게 \_\_dirname으로 절대 경로 고정하고 파일 경로를 따라 가는 것 보다 미리 경로를 어느정도 지정해서 편하게 사용하자

```jsx
const path = require("path");
module.exports = path.dirname(require.main.filename);
```

- import 후 \_\_dirname 대신 사용
