# Week_2

## Express 정의

rest 서버를 편리하게 구현하게 해주는 프레임워크

express는 서버가 아니라 단지 서버를 편히라게 가져오게 해주는 도구(모듈)일 뿐이다.

## 왜  Express를 사용해야 하지?

프레임워크는 개발하기 편하게 개발자들에게 개발 규칙을 강제하여 코드 및 구조의 통일성을 향상시킬 수 있다

그 중에서도 koa, hapi 등 다양한 프레임워크에 비해 Express의 장점은 

1. 단순함

작은 서비스를 만드는데 더 적합하다.

1. 커뮤니티

사용자가 많은 만큼 문제 해결이 비교적 원활하다.

## http 내장 모듈로 웹 서버 띄우기 vs Express로 웹 서버 띄우기

우리가 전에 배웠던 웹 서버를 띄운 과정은 http 내장 모듈로 이루어졌다. express , http 모두 가능하다. 하지만

 http 모듈만 사용해서 웹 서버를 구성할 때 많은 것들을 직접 만들어야한다. 이런 문제를 해결하기 위해 만들어진 것이 Express이다. 

- Express()

⇒ express 함수를 호출하여 반환된 객체

## 미들웨어란 ?

middle + software의 합성어이다. 말 그대로 가운데 위치하는 소프트웨어라는 말이다. client와 dat사이 가운데를 말하는 것이다. 서버에 들어온 요청에 대해 중간에 위치하여서 요청을 처리하기 전에 실행하는 소프트웨어를 말한다

클라이언트에게 요청이 오고 그 요청을 보내기 위해 응답하려는 중간에 목적에 맞게 처리를 하는 말하자면 거쳐가는 함수들이라고 보면 되겠다.

다음 미들웨어 함수에 대한 엑세스는 next 함수를 이용해서 다음 미들웨어로 현재 요청을 넘길 수 있다.

next라는 말에서 알 수 있듯이 next를 통해 미들웨어는 순차적으로 처리된다. → next는 세번째 인자로 들어온 함수

## 미들웨어 작성하기

use()를 호출하여 미들웨어를 등록해야한다. 

next는 지금 있는 미들웨어를 다른 미들웨어로 옮기는 것이다.

next를 호출 안하면 그냥 멈추고 next를 호출하지 않은 경우 응답을 다시 전송해야 한다. 왜냐하면 요청이 계속 이동할 수 없고 우리가 응답을 보낼 수 있는 장소에 결코 도달하지 못한다. 

→ a는 b한테 보내고 b는 c한테 보내서 각자 일을 처리해서 보내야하는데 next를 쓰지 않으면 호출이 안된다

## 미들 웨어 장점

- 표준화된 인터페이스 제공
- 다양한 환경 지원 체계가 다른 업무와 상호 연동 기능
- 분산된 업무를 동시에 처리 가능

## send 메서드

res.send()는 send에 전해진 argument에 따라서 content type이 자동적으로 만들어진다 이게 기본이다.

## 라우팅이란?

먼저 라우팅이란 네트워크 용어로서 어떠한 네트워크 안에서 통신되는 데이터를 보낼 경로를 선택하는 과정 . 간단히 말해서 갈림길에서 어디로 가야할지를 선택하는 과정을 말한다.

예를 들어 우리 웹 서버를 localhost:3000으로 지정해줬다면 /(루트)에 접속했을 때는 이러이런 것을 실행하고

/login에 접속했을 떄는 저러저러한 것을 실행한다

이렇게 해서 경로에 따라 데이터 보내는 게 달라지니까 라우팅을 구현한 것이다.

그런데 실제 웹 서비스를 구현할 때 규모가 점점 커진다면?

server.js안에 다양한 하위 디렉터리를 만들어야할 것이며 한 파일에 너무 많은 라우팅이 한번에 몰려있으면 유지 보수도 어렵고 코드의 가독성도 떨어질 수 있다.

+routes는 서버의 라우터와 로직을 모아둔 것

 [https://futuregate.tistory.com/8](https://futuregate.tistory.com/8)

## 우리는 여러 미들웨어로 라우팅 해준다

우리는 다양한 라우트 즉 url을 처리하고자 한다.

어떻게? → 미들웨어로 !

경로를 추가한다.

코드 예시로 살펴보면

```java
const express=require('express');

const app = express();

app.use('/',(req,res,next)=>{
console.log("This always runs!");
next();
});

app.use('/',(req,res,next)=>{
console.log("In another middle ware");
res.send('<h1> The add product</h1>');
});

app.use('/',(req,res,next)=>{
console.log("In the middle ware");
res.send('<h1> hello from express</h1>');
});

app.listen(3000);
```

먼저 express의 listen 메서드를 통해 웹 서버를 3000으로 지정한다. 

use 미들웨어를 통해 다양한 라우트를 만들었던 것

맨 첫번째 미들웨어를 보면 / 루트로 사용해서 console 찍고 next로 다음 미들웨어로 넘어감

두번째는 /루트긴하지만 send로 화면에 보이고 싶은 html을 추출

그런데 이때 next를 사용하지 않았어서 다음 라우팅으로 넘어가지 않는다

왜? → **다른 응답 관련 코드들이 실행되면 안되니까**

**하나이상의 응답을 보내려면 오류가 발생하니까 !**

## 오류 페이지 추가

우리가 경로에 무작위로 문자열을 입력하면 오류가 발생한다 404 에러 페이지에 들어가고 싶을 때 그런 요청을 처리하는 방법을 활용한다.

적당한 미들웨어가 존재하지 않으면 맨 아래로 내려가고 요청은 처리되지 않는다. 따라서 오류 페이지 전송을 위한 미들웨어를 추가하면 된다. 

(/루트를 사용해도 괜찮다 어차피 중간에 미들웨어를 거치면 next가 없으므로 거기까지 안 내려간다)

( 마지막이 send이기 전에 status를 호출해서 상태 코드를 정의할 수 있다)

## redirect 리다이렉트 메서드

리다이렉트 메서드는 파라미터에 해당하는 url로 페이지를 이동시킨다

## body parser

body-parser는 미들웨어이다. 즉 요청과 응답 사이에서 공통적인 기능을 수행하는 소프트웨어이다. 어떤 역할을 하냐면 요청의 본문을 지정한 형태로 파싱해주는 미들웨어이다. 

express는 요청을 처리할 때 기본적으로 body를 undefined로 처리하고 있다. 그래서 req.body를 콘솔로그로 출력해보면 undefined가 출력된다

이럴때 body parser 미들웨어를 이용하면 request의 body부분을 자신이 원하는 형태로 파싱하여 활용할 수 있다.

(강의 예시로는 form으로 받은 데이터를 파싱)

[https://ninjaggobugi.tistory.com/11](https://ninjaggobugi.tistory.com/11)

```java
app.use(bodyParser.urlencoded({extended:false}));
```

## 코드 예시

```java
const express=require('express');
const bodyParser= require('body-parser');

const app = express();

app.use(bodyParser.urlencoded({extended:false}));

app.use('/add-product',(req,res,next)=>{
    res.send('<form action="/product" method="POST"><input type="text" name="title"><button type="submit">ADD</button></form>');
});

app.post('/product',(req,res,next)=>{
console.log(req.body);
res.redirect('/');
});
app.use('/',(req,res,next)=>{
    res.send('<h1> hello from express</h1>');
});

app.listen(3000);
```

일단 action을 통해 요청을 보낼 경로 url을 지정해준다 form으로 부터 입력 받은 데이터를 product 경로로 보내는거

사실 /add-product 경로나 /product 경로 둘다 단어가 겹치는것이지 경로는 겹치는게 없어서 어디에 놓던 상관이 없다

req를 분석하기 위해 body parser을 깔아야 한다. 분석기를 등록해야하는데 또 다른 미들웨어를 추가해서 구현

body parser.urlencoded 이 코드를 미들웨어로 등록해준다. 요청 본문 분석을 수행한다. 하지만 이것은 앞에다가 등록 해주는 게 좋음. 왜냐하면 요청이 어디로 향하든 본문 분석이 이루어지도록 하기 위해서다.

그런데 extended:false랑 extended true랑 무슨 차이냐? 인코딩 방식의 차이 key value 형태로 인코딩 하려면 저 방식을 사용해야함 json 데이터 형식 파일 분석은 따로 있다고 강의에서 언급했듯이 다른게 있음

우리는 이 요청 본문 분석을 통해 javascript 객체를 얻는 것이다.

또한 use인 미들웨어는 post 요청 뿐만 아니라 get 요청에 대해서도 실행이 되기에 get 요청만 다루고싶으면 get 또는 post로 적어도 된다

## express.Router

router 인스턴스는 완전한 미들웨어이자 라우팅 시스템이다. 우리는 app.js에서 라우팅을 할 수도 있지만 프로젝트가 커져 라우팅을 할 경로가 많아지면 유지 보수가 어려워질 수 있어서 route를 분리하기 위해 사용한다

파일을 여러 로직에 보내서 임포트!

```java
const router= express.Router();
```

이렇게 라우터 객체를 참조해서 exports하고

```java
app.use(adminRoutes);
```

라우터 객체를 앱 객체에 등록 시켜준다

## 코드 예시

```java
//server.js
const express=require('express');
const app = express();
const bodyParser= require('body-parser');

const adminRoutes = require('./routes/admin');
const shopRoutes=require('./routes/shop');

app.use(bodyParser.urlencoded({extended:false}));

app.use(adminRoutes);
app.use(shopRoutes);

app.listen(3000);

//admin.js
const express = require('express');

const router = express.Router();
//이 클래스를 사용하면 모듈식 마운팅 가능한 핸들러 작성 가능

router.get('/add-product',(req,res,next)=>{
    res.send('<form action="/product" method="POST"><input type="text" name="title"><button type="submit">ADD</button></form>');
});

router.post('/product',(req,res,next)=>{
console.log(req.body);
res.redirect('/');
});

module.exports=router;
//router 객체에 라우터를 등록시키고 내보낸다고 생각..한다
//shop.js
//이렇게 라우터가 등록된 것
const express= require('express');

const router= express.Router();

router.get('/',(req,res,next)=>{
    res.send('<h1> hello from express</h1>');
});

module.exports=router;
//파일로 내보낸 라우터 객체
```

<aside>
💡 get과 post 등은 경로와 정확히 일치하는 요청만을 처리

</aside>

만약 이게 use를 사용한 것이었으면 미들웨어 순서가 중요하다 하지만 지금 모든 라우트들은 get과 post를 사용 get과 post는 정확히 이 경로가 맞는지를 확인한다.

그래서 use를 쓸 때는 /만 들어가면 그에 맞는 요청을 다 처리해줬는데 get post는 /~~무작위로 입력하면 오류이고 /루트에만 맞는 요청을 처리한다.

하지만 그래도 순서를 신경 쓰는 것이 좋음 !!

## 경로 필터링

하나의 라우터 파일안에 있는 라우트들은 시작 경로를 공유하는 경우가 있다 .  우리는 경로에 대한 필터를 걸 수 있다

```java
app.use('/admin',adminRoutes);
```

이것은 /admin으로 시작하는 경로만 adminRoutes 파일로 들어가게 된다. 그러면 adminRoues 파일에 있는 경로들은 /add-product라고 하면 /admin/add-product와 사실상 마찬가지이다.

/add-product를 /admin/add-product와 매칭하는 거

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

[https://www.daleseo.com/js-node-path/](https://www.daleseo.com/js-node-path/)

[https://curryyou.tistory.com/361](https://curryyou.tistory.com/361)

[https://velog.io/@cloud_oort/node.js-path모듈과-경로설정](https://velog.io/@cloud_oort/node.js-path%EB%AA%A8%EB%93%88%EA%B3%BC-%EA%B2%BD%EB%A1%9C%EC%84%A4%EC%A0%95)

[https://velog.io/@yhe228/res.sendFile-path.join](https://velog.io/@yhe228/res.sendFile-path.join)

### helper 함수: 상위 디렉토리를 얻어올 수 있다

폴더를 생성한 후 상위 디렉토리로 가는 경로를 구축할 수 있다

path.join()이 아닌 path.dirname을 사용한다

dirname은 경로의 디렉터리 이름 회신