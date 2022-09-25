# TEAM 보겸 - 2주차 RBF

# Week2 RBF

## Express.js란?

Express.js를 사용하면, 세부 내용을 외부에 맡길 수 있도록 도와줍니다. 이에, 기준이 되는 코드를 깔끔하게 작성할 수 있습니다.

힘들게 해야할 작업들을 간소화 해주는 프레임 워크.

express는 서버가 아니라 단지 서버를 편히라게 가져오게 해주는 도구(모듈)일 뿐이다.

- middleware
- 요청과 응답 간소화
- 라우팅
- HTML 반환

### Why?

복잡한 서버 로직을 해결해준다. 다른 패키지를 쉽게 설치하도록 도와준다.

사소한 디테일보다, 중요한 로직에 집중할 수 있게 해준다.

무거운 작업을 프레임워크가 처리해준다.

프레임워크는 개발하기 편하게 개발자들에게 개발 규칙을 강제하여 코드 및 구조의 통일성을 향상시킬 수 있다

그 중에서도 koa, hapi 등 다양한 프레임워크에 비해 Express의 장점은

1. 단순함

작은 서비스를 만드는데 더 적합하다.

2. 커뮤니티

사용자가 많은 만큼 문제 해결이 비교적 원활하다

## http 내장 모듈로 웹 서버 띄우기 vs Express로 웹 서버 띄우기

우리가 전에 배웠던 웹 서버를 띄운 과정은 http 내장 모듈로 이루어졌다. express , http 모두 가능하다.

하지만 http 모듈만 사용해서 웹 서버를 구성할 때 많은 것들을 직접 만들어야한다. 이런 문제를 해결하기 위해 만들어진 것이 Express이다.

- Express()

⇒ express 함수를 호출하여 반환된 객체

## Middleware

middle + software의 합성어이다. 말 그대로 가운데 위치하는 소프트웨어라는 말이다. client와 dat사이 가운데를 말하는 것이다. 서버에 들어온 요청에 대해 중간에 위치하여서 요청을 처리하기 전에 실행하는 소프트웨어를 말한다.

클라이언트에게 요청이 오고 그 요청을 보내기 위해 응답하려는 중간에 목적에 맞게 처리를 하는 말하자면 거쳐가는 함수들이라고 보면 되겠다. 요청을 응답까지 자동적으로 이동시키는 것.

하나의 함수를 사용하기 보다는, 여러 블록(조각)들로 분할하여 사용한다.

다음 미들웨어 함수에 대한 엑세스는 next 함수를 이용해서 다음 미들웨어로 현재 요청을 넘길 수 있다.

```jsx
app.use((req, res, next) => {
  //모든 요청마다 실행
  console.log("In the middleware");
  next(); //요청이 다음 미들웨어로 이동하게 해줌.
});
```

next라는 말에서 알 수 있듯이 next를 통해 미들웨어는 순차적으로 처리된다. → next는 세번째 인자로 들어온 함수

## 라우트 사용

먼저 라우팅이란 네트워크 용어로서 어떠한 네트워크 안에서 통신되는 데이터를 보낼 경로를 선택하는 과정 . 간단히 말해서 갈림길에서 어디로 가야할지를 선택하는 과정을 말한다.

use 함수의 인자를 통해 라우트를 사용할 수 있다.

예를들어, ‘/add-product’를 작성하면 “localhost:300/app-prouduct”로 이동하게 된다.

참고로, “/”를 작성하면 “/”로 시작하는 어떤 곳이든 다 이동할 수 있으므로 주의해서 사용하자.

이렇게 해서 경로에 따라 데이터 보내는 게 달라지니까 라우팅을 구현한 것이다.

그런데 실제 웹 서비스를 구현할 때 규모가 점점 커진다면?

server.js안에 다양한 하위 디렉터리를 만들어야할 것이며 한 파일에 너무 많은 라우팅이 한번에 몰려있으면 유지 보수도 어렵고 코드의 가독성도 떨어질 수 있다.

+routes는 서버의 라우터와 로직을 모아둔 것

    [https://futuregate.tistory.com/8](https://futuregate.tistory.com/8)

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

## 수신 요청 분석

1. app-product에서 form이 있는 html 페이지를 작성해보자.
2. product 요청을 처리할 라우터 작성법은, app.use(’/product’)이다.
   참고로, product 요청 이후에만 작성한다면 어디에 작성하든 상관없다.
3. ‘/’ 경로로 리다이렉트하기 위한 작성법은, res.redirect(’/’)이다.
4. 사용자가 보낸 내용 추출, console.log(req.body);
5. 제3자 패키지 설치를 위해 터미널에 npm install -save body-parser를 작성하자.
   이름을 body-parser로 설정하였으므로, 임포트할 때 const bodyParser = require(’body-parser’);가 된다.
6. bodyParser 호출은 app.use(bodyParser.urlencoded({extended: false}));
   .urlencoded는 실행해야 하는 함수로 미들웨어를 등록해준다. 폼을 통해 전송된 본문을 분석해준다.
7. 비표준 대상의 분석이 가능한지를 나타내는 extended를 false로 설정하여 본문 분석기가 활성화 되었다.

body-parser는 미들웨어이다. 즉 요청과 응답 사이에서 공통적인 기능을 수행하는 소프트웨어이다. 어떤 역할을 하냐면 요청의 본문을 지정한 형태로 파싱해주는 미들웨어이다.

express는 요청을 처리할 때 기본적으로 body를 undefined로 처리하고 있다. 그래서 req.body를 콘솔로그로 출력해보면 undefined가 출력된다

이럴때 body parser 미들웨어를 이용하면 request의 body부분을 자신이 원하는 형태로 파싱하여 활용할 수 있다.

(강의 예시로는 form으로 받은 데이터를 파싱)

[https://ninjaggobugi.tistory.com/11](https://ninjaggobugi.tistory.com/11)

[https://www.npmjs.com/package/body-parser](https://www.npmjs.com/package/body-parser)

### 경로 필터링

아웃소싱 라우트들은 시작경로를 공유하는 경우가 있음.

상위 파일에서 라우팅 해줄때 시작점을 꺼내줄 수 있다.

예를 들어 app.use 안에 있는 라우터에 같은 경로로 여러개의 요청이 나뉘어 있다면

/path/main-page 의 경로를 요청마다 다 써야되겠지만,

```jsx
app.use("path", file);
```

위와같이 한다면 main-page만 적어도 이동 가능.

## send File 메서드

이 경로(ex /add-product)로 어떤 요청이 들어오게 되면 해당 경로의 파일을 읽고 내용을 클라이언트에게 전송한다

  <aside>
  💡 그런데 이때 이 파일을 어떤 방식으로 적어야 하나?
  
  </aside>
  
  ex) /views/shop.html 이런 식으로 적으면 절대 경로여야 한다는 오류가 발생 지금 /이것은 프로젝트 폴더가 아닌 운영체제의 루트 폴더를 나타내기 때문에
  
  - 절대 경로 : 어떠한 웹 페이지나 파일이 가지고 있는 고유한 경로 ex) |Desktop|projects ..
  - 상대 경로: 현재 위치한 곳을 기준으로 상대적으로 표현한 경로
  
  **우리는 파일이나 디렉토리 경로를 다루기 위해 path 모듈을 쓴다**
  
  그런데 경로 다루는게 뭐가 어렵다고 굳이 별도의 모듈을 쓰나?
  
  일단 유닉스 계열의 운영체제와 윈도우 운영체제는 서로 다른 문자로 디렉토리 구조 표현 ( 유닉스는 / 문자 윈도우는 \문자)
  
  그래서 파일이나 디렉토리 경로를 단순히 문자열을 이용하여 접근하면 프로그램이 특정 운영체제에서만 돌아갈 위험이 생긴다. 그래서 개발자들이 위험없이 경로를 다룰 수 있도록 도와준것
  
  - path . join() : path 인자로 받은 주소들을 합쳐준다
  - __dirname : 운영체제의 절대 경로를 프로젝트 폴더로 고정 시켜주는 역할을 한다
  
    dirname 구현하면 수동으로 이어붙일 수 있다
  
  ```jsx
  app.use(express.static(path.join(__dirname, 'public')));
  ```
  
  public 폴더에 있는 파일들을 쉽게 쓸 수 있도록 해줌.
