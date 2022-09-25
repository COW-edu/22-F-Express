# section 5

# 57. 모듈 소개

### Express.js

node.js 만 사용하면 힘들게 해야할 작업들을 간소화 해주는 프레임 워크

- middleware
- 요청과 응답 간소화
- 라우팅
- HTML 반환

# 58. Express.js란?

### What & Why?

복잡한 서버 로직을 해결해준다. 다른 패키지를 쉽게 설치하도록 도와준다.

사소한 디테일보다, 중요한 로직에 집중할 수 있게 해준다.

무거운 작업을 프레임워크가 처리해준다.

### 대체제들

바닐라 node.js

Adonis.js

Koa

Sails.js 등등

Express?

특별한 기능을 과도하게 추가하지 않음

유연함, 높은 확장성.

다양한 패키지 활용 가능.

# 60. 미들웨어 추가

요청 → 미들웨어( next() ) → 미들웨어( res.send() ) → 응답

들어오는 요청을 다양한 함수를 통해 자동으로 이동하는것

응답을 보내기 전까지 다여러 함수를 거쳐 감.

코드를 여러개의 함수로 분할하여 관리할 수 있다.

```jsx
app.use((req, res, next) => {
  //모든 요청마다 실행
  console.log("In the middleware");
  next(); //요청이 다음 미들웨어로 이동하게 해줌.
});
```

# 61. 미들웨어 작동 방식

next() 를 통해서 다음 미들웨어로 이동할 수 있음.

send()를 통해 기본값이 지정된 응답 받을 수 있음.

# 62. 백그라운드 확인

작동원리? → github 확인

res.send()

버전확인

어떤 데이터를 주는가? 확인

타입이 이미 정해져있는가?

# 63. 다른 라우트 사용법

미들웨어의 use() 에 경로 지정.

next() 활용하지 않으면 아래로 내려가지 않는다는 점을 활용해서 코딩.

```jsx
app.use("/", (req, res, next) => {
  //모든 경로는 /로 시작하므로 항상 실행됨.
  console.log("This always runs");
  next(); //다음 미들웨어로 넘어감.
});

app.use("/another-page", (req, res, next) => {
  //이 경로로 들어왔다면 아래의 미들웨어 실행x
  console.log("In another middleware");
  res.send("<h1>Another page showing</h1>");
});

app.use("/", (req, res, next) => {
  console.log("In another middleware");
  res.send("<h1>Hello form Express!</h1>");
});
```

# 64. 수신 요청 분석

들어오는 요청을 분석하고 데이터를 추출

POST 요청 처리.

아무런 설정 없이 요청을 분석하면 undefined 로 돌아온다.

body-parser 패키지로 body를 분석 할 수 있다.

[https://www.npmjs.com/package/body-parser](https://www.npmjs.com/package/body-parser)

폼을 통해 보낸 본문 분석.

# 65. POST 요청으로 미들웨어 실행 제한

app.get, app.post로 특정 요청만 받을 수 있음.

delete, patch 또한 존재.

# 66. Express 라우터 사용

로직을 여러 파일로 내보내서 import

코드 길어지는 것을 방지.

유지보수 용이하게 만들기 위함.

# 67. 404 오류 페이지 추가

app.js 파일에서의 미들웨어에 next 가 없다면 내려가지 않지만

그 어떤 미들웨어도 거쳐가지 않는다면 그 요청은 처리되지 않음.

```jsx
res.status(404).send("<h1>Page not found</h1>");
```

send 이전에 status 코드 추가해서 구현 가능.

다른 메서드들도 호출 가능.

# 68. 경로 필터링

아웃소싱 라우트들은 시작경로를 공유하는 경우가 있음.

상위 파일에서 라우팅 해줄때 시작점을 꺼내줄 수 있다.

예를 들어 app.use 안에 있는 라우터에 같은 경로로 여러개의 요청이 나뉘어 있다면

/path/main-page 의 경로를 요청마다 다 써야되겠지만,

```jsx
app.use("path", file);
```

위와같이 한다면 main-page만 적어도 이동 가능.

# 69. HTML 페이지 생성

MVC - model veiw controler 구조

사용자에게 제공하는 것이 veiw 폴더에 들어감.

# 70. HTML 페이지 서비스하기

```jsx
res.sendFile(path.join(__dirname, "../", "views", "shop.html"));
```

sendFile 로 원하는 파일 응답 가능.

내 경로 안에 있는 파일을 보내기 위해서 path import 하여 join 메서드 사용

join 뒤에 \_\_dirname 전역변수(자신이 사용된 파일의 경로 알려줌) 사용 이후, 폴더와 파일 띄어서 입력

, 사용해서 띄는 이유는 어떤 운영체제에서도 그에 맞게 작동하는 방식으로 경로를 생성해주기 때문.

# 73. 내비게이션을 위한 헬퍼 함수 사용

helper 함수로 상위 디렉터리를 얻을 수 있음.

```jsx
const path = require("path");

module.exports = path.dirname(require.main.filename);
```

dirname 메서드 안에 require.main.filename 으로 어플리케이션이 시작한 모듈 가져오고 경로를 알 수 있게됨.

# 75. 정적으로 파일 서비스하기

외부 css 파일 임포트 하는 방법

대중에게 항상공개되는 폴더 public

엑세스에 권한이 필요하지 않음

일반적인 방식으로는 연결되지 않음

```jsx
app.use(express.static(path.join(__dirname, "public")));
```

public 폴더에 있는 파일들을 쉽게 쓸 수 있도록 해줌.

# 76. 마무리

### Express.js?

- node.js 프레임워크. 유틸리티 함수와 도구들을 사용해 앱을 어떻게 구성해야하는지 쉽게 해줌.
- 확장성때문에 널리 사용됨. 쉽게 패키지를 추가할 수 있음. 미들웨어 함수를 통함.

### 미들웨어

- 요청객체나 응답객체를 받아서 응답 전송을 도와주는 함수.
- next 다음 미들웨어로 보낼때 필요한 함수. 위에서 아래로 흠름이 이어짐.
- 이로 인해 흐름을 제어할 수 있음

### 라우팅

- 쉽게 경로나 요청 메서드로 요청을 필터링할 수 있음.
- 라우터 패키지 활용가능.

### 파일 서비스

- 이미지나 html 등의 다양한 파일에 대한 요청도 가능함.
