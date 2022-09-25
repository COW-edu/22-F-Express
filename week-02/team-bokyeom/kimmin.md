# Section4

## NPM : Node Package Memory

npm을 사용하면 노드 프로젝트라고 부르는 작업의 초기 내용을 설정할 수 있다. 즉, 프로젝트에서 프로젝트로 이동하는 Terminal에서 NPM을 실행할 수 있다.

터미널에 **npm init**을 작성한다. Enter를 누르며 기본값으로 설정한다. package.json 파일이 생성되었다.

“scripts”의 속성

|  | 터미널 | 설명 |
| --- | --- | --- |
| “start” | npm start | 특수한 스크립트 이름 |
| “start-server” | npm run start-server | 알려지지 않은 명령어 |

## 제 3자 패키지 설치하기

코드 변경 시 Ctrl+C를 눌러 개발 서버를 종료하고 재시작해야한다. 저장만 눌러도 자동으로 재시작하겠금 해보자. 

해당 기능이 있는 제3자 패키지를 설치해야 한다.
터미널에 npm Nodemon —save-dev라 입력하자.

Nodemon은 개발 과정에서만 사용하는 개발의존성이다. 이에, 실제 서버에 앱 설치 시 필요가 없다. 인터넷 어딘가에서 실행중인 실제 서버는 재시작해서는 안 된다.

—save까지만 입력하면 패키지가 프로덕션 의존성으로 설치되며 실제로 코드 안에서 사용하고 작업하는 패키지이다.

-dev를 추가하면, 단순히 개발 도중 사용하는 것임을 나타낸다.

참고로, 세번째 선택자 -g는 머신 전체에 설치함으로써 어디에서든지 사용할 수 있다.

node_modules 폴더, package-lock.json 파일이 추가되었다.

package.json 파일도 업데이트 되었다.

이제 재실행하려면 npm install만 입력해주면 된다. package.json에 언급된 모든 패키지를 검색해서 설치해준다.

node_modules 폴더에 설치되어있다!

폴더 속 nodemon을 보면 여러 의존성들이 있는 것을 확인할 수 있다. 다만 방대한 양이기에 용량 부족 시 삭제하였다가, 언제든 npm install만 입력하면 다시 패키지를 사용할 수 있게 된다.

참고로, node_modules 폴더가 생성되지 않는다면

npm install -g nodemon

npm install —save-dev nodemon

을 입력해 주자.

## 전역 기능 vs 코어 모듈 vs 제 3자 모듈

| 전역 기능 | 코어 Node.js 모듈 | 제 3자 모듈 |
| --- | --- | --- |
| const나 function 같은 키워드 및 process 등의 전역 객체 | 파일 시스템 모듈 ("fs"), 경로 모듈 ("path"), Http 모듈 ("http") 등 | npm install을 통해 어떤 종류의 기능도 설치 가능 |
| 항상 사용 가능, 임포트도 필요 없다. | 설치하지는 않아도 되지만, 관련 기능 사용시 임포트 |  |

## 자동 재시작을 위한 Nodemon 사용

pakage.json의 start 부분을 변경해보자. 

node app.js에서 nodemon app.js로 바꾸자.

참고로, 터미널에서 nodemon app.js를 검색하면 오류가 뜬다. 왜냐하면 머신 전체가 아닌 이 프로젝트에만 설치했기 때문이다.

이제, npm start를 실행해보자.

routes.js파일에서 뭔가 편집해 보고 저장하면 재시작한다.

## 오류 수정

오류의 종류 3가지

- 구문 오류 ex)오타
- 런타임 오류; 코드를 실행하려고 할 때 멈춰버리는 상황
- 논리적 오류; 오류 메시지가 뜨지 않으며, 원하는 대로 작동하지 않는다. 원인 찾기가 쉽지 않다.

# 구문 오류

오타나 괄호를 빼먹은 경우 둥을 말한다.

고치기 어렵지는 않다. IDE 자체에서 문제가 어디에 있는지 알려준다.

## 런타임 오류

구문 오류가 없기에 코드는 문제없이 실행된다. 하지만 웹 페이지에서는 오류가 나타나는 것을 확인할 수 있다.

오류 메시지를 잘 읽어보면 해답을 찾을 수 있다.

## 논리 오류

오류 메시지가 뜨지 않아 고치기 까다롭다. 또한 원하는 대로 작동하지 않는다.

예를 들어 잘못된 인덱스를 사용한 경우이다.

## 디버거 사용

Node.js의 디버거(Debugger)를 사용하여 오류를 발견해보자.

1. VS code 메뉴 중 Debug > Start Debugging 클릭
2. 환경은 Node.js로 설정
    - 상단에 막대기 바 추가됨
    - 하단의 빨간 바는 디버깅 모드임을 나타냄
3. route.js 파일로 가서 코드 라인 번호 왼편에 마우스를 가져다 대어 나타나는 빨간 점을 클릭하여 중단점을 설정한다.
4. 브라우저를 실행하고 돌아오면 해당 코드 라인에 노란색 하이라이트 표시가 나타난다. 코드 실행이 중단되었다는 뜻이다. 이제 들여다보자.
5. View 메뉴의 Debug를 누르면, 특별한 디버그 모드가 실행되어 변수를 보여준다. 잘못된 변수가 있는지 오류를 찾아보자.
6. 상단 막대기 바의 버튼을 통해 코드를 나누며 분석해보자.

+) 콘솔에서 코드를 실행해보자. 실제 코드에 영향 없이 연산 등을 실행할 수 있기에, 실제 코드에 추가하기 전 변경 사항을 시험해 볼 수 있다.

## 편집 후 자동으로 디버거 재시작하기

 nodemon의 패키지를 사용하여, 코드를 추가할 때마다 디버그를 재시작 해보자.

1. Explorer 모드로 돌아간 후 Debug 메뉴에 Add Configuration을 클릭한다. 
.vscode 폴더와 launch.json 파일이 추가되어 프로젝트에 디버깅을 구성할 수 있도록 한다.
2. “restart”: ture, 
    
    “runtimeExecutable”=”nodemon” (주의, 기본값인 node가 아니다.)
    
3. “console”:“intergratedTerminal” 작성하며 로그가 남는 콘솔도 일반 터미널로 바꾼다.
4. 디버깅을 재시작하면 오류가 난다. 지역 nodemon이 아닌 전역 nodemon을 이용하려 하기 때문이다. 터미널에 npm install nodemon -g를 입력하여 전역 nodemon으로 바꾸자.

# Section5

## Express.js란?

Node.js만 단독으로 사용할 경우 많은 코드를 입력해야합니다.

Express.js를 사용하면, Node 프로젝트에 제3자 패키지로 설치할 수 있는 프레임워크로 일부 필수 작업이나 세부 내용을 외부에 맡길 수 있도록 도와줍니다. 기준이 되는 코드를 깔끔하고 핵심적인 일에 집중할 수 있도록 도와주는 유틸리티 함수를 다수 제공합니다.

## Express.js 설치

터미널에 “npm install —save”를 실행하여 Express.js를 설치하자.

참고로, “—save dev”가 아니라 “—save”를 사용하는 이유는, 프로덕션 의존성이기 때문이다. 개발 중에만 사용하는 것이 아닌, 애플리케이션 배포한 뒤에도 실행되겠금 하기 위해서이다.

## Middleware

미들웨어란 들어오는 요청을 자동으로 이동하는 것이다. 요청이 통과해야하는 다양한 함수들을 연결할 가능성을 확보한다. 이에, 하나의 함수를 사용하기 보다는, 코드를 다수 블록/조각들로 분할할 수 있는 것이 Express.js의 특징이다.

```jsx
app.use((req, res, next) => {
    console.log('In the middleware!');
    next(); // Allows the request to continue to the next middleware in line
});
```

use 메서드를 사용하면 middleware 함수를 추가할 수 있다.

3가지 인수를 받게 된다. req, res는 요청과 응답이다. next는 Express.js를 통해 전달되는 함수이다. 함수이기에, next 안의 인수도 수신한다. next 안의 인수가 다음 middleware로 요청이 이동할 수 있도록 실행되어야 한다.

예를 들어, console.log('In the middleware!');라 작성하면 nodemon이 서버를 자동 재시작하고, localhost:3000브라우저 콘솔창에 'In the middleware!'가 나타난다.

다음 라인의 미들웨어로 요청을 이동 시키려면 next();를 작성해야 한다.

## Middleware 작동

Express.js는 기본 응답이 없기에, use 함수에, 새로운 Utility 함수인 send를 사용해 응답을 보내거나 and 유형의 본문을 첨부해보자.

```jsx
app.use((req, res, next) => {
    console.log('In another middleware!');
    res.send('<h1>Hello from Express!</h1>');
});
```

html 코드를 작성하고 페이지를 열어보면 코드가 나타나는 것을 확인할 수 있다! 개발자도구의 Network > Header > Content-Type 이 text/html 이므로 이는 Express에서 제공하는 자동 기능인 것을 알 수 있다. 참고로, setHeadear를 사용해 수동으로 설정할 수도 있기도 하다.

## 다른 라우트 사용

API reference에 들어가면 app.use(); 항목에 대한 설명이 나와있다. 5가지의 사용법 중, callback 함수에 대해 알아보자.

callback은 하나 이상 있을 수 있기에 원하는 만큼 가능하다.

“/”와 같은 방식은 기본값이다. “/”로 시작하는 어떤 값이든 다 적용된다. 예를 들어 “localhost:300/app-prouduct”로 이동하여도 “localhost:3000/”과 동일한 결과를 확인할 수 있다. 도메인 뒤에 오는 경로가 “/”로 시작한 경우 모두 해당된다. 

```jsx
app.use('/add-product', (req, res, next) => {
	console.log('In another middleware!');
    res.send('<h1>The "Add Product" Page</h1>');
});
app.use('/', (req, res, next) => {
	console.log('In another middleware!');
    res.send('<h1>Hello from Express!</h1>');
});
```

“/app-product”를 작성해보자. 윗줄에 작성해야 한다! 요청은 파일의 위에서부터 아래로 향하기에, next();를 호출하지 않으면 다음 미들웨어로 넘어가지 않기 때문이다. 이제, “localhost:300/app-prouduct”로 이동하면 The "Add Product" Page가 나타난다. 이를 제외한 모든 다른 경로에서는 Hello from Express!가 나타난다.

즉, 작성 순서와 next등을 잘 활용하며 작성하면 된다.

## 수신 요청 분석

```jsx
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

app.use(bodyParser.urlencoded({extended: false}));

app.use('/add-product', (req, res, next) => {
  res.send('<form action="/product" method="POST"><input type="text" name="title"><button type="submit">Add Product</button></form>');
});

app.use('/product', (req, res, next) => {
	console.log(req.body);
	res.redirect('/');
});

app.use('/', (req, res, next) => {
  res.send('<h1>Hello from Express!</h1>');
});

app.listen(3000);
```

1. add-product에 form이 있는 html 페이지를 작성해보자.
action에 요청을 보낼 경로 url을 작성하자. product로 해보자.
    
    input 과 button도 추가하자.
    
2. product 요청을 처리할 라우트/미들웨어인 app.use(’/product’)를 작성하자.
    
    이는 어디에 두어도 충돌하지 않는다! product 요청 이후에만 위치하면 된다.
    
    참고로, 사용하지 않는 인수는 생략가능하다. 단, res를 받기 위해서는 req는 생략해서는 안 된다. 순서는 중요하기 때문이다.
    
3. 리다이렉트하기 위해 res.redirect(’/’);를 입력하면 자동으로 ‘/’ 경로로 리다이렉트 된다.
4. 사용자가 보낸 내용 추출을 위해, console.log(req.body)를 작성하자.
5. 미들웨어 전에 경로 처리를 먼저 해주어야 한다. 여기서 들어오는 요청을 분석도 한다. 이를 위해 제3자 패키지를 설치하자.
터미널에 npm install —save body-parser를 실행하여 설치하자. 이름이 body-parser로 설정했으니, 임포트할 때 const bodyParser = require('body-parser');라 작성하면 된다.
6. bodyParser를 호출해서 사용하자.
.urlencoded는 실행해야 하는 함수로 미들웨어를 등록해준다. 수동으로 해왔던 요청 본문 분석을 담당한다. 폼을 통해 전송된 본문을 분석해준다.
7. 비표준 대상의 분석이 가능한지를 타나태는 extended를 false로 설정하여 본문 분석기가 활성화 되었다.
8. 재시작하고, 서버를 실행하여 book를 input 하고 버튼을 눌러보자. 그럼 리다이렉트 되고, 콘솔 창에는 { tilte: ‘book’ } 이 잘 나타난다.

---

## Express 라우터 사용

파일의 양이 클 경우, 여러 파일로 나누는 것이 좋기에 이때 활용 되는 것이 라우터이다. 여러 파일로 내보내고 임포트하자. Express.js는 라우팅을 다른 파일에 위탁하는 편리한 방법을 제공한다.

1. routes라는 폴더를 만들고, 폴더 속에는 역할에 따라 다른 js 파일들을 생성하기
2. 각 js 파일에는, express 임포트를 위한 ‘const express = require(’express’);’를 작성
3. 라우터 임포트를 위한 ‘const router = express.Router();’도 작성. ‘module.exports = router;’도 작성.
4. app.js에서 사용했던 라우터들을 가지고 와서, app 대신 router로 바꿔주자.
5. use 대신하여, 회신하는 데만 사용하는 Get으로 변경해도 된다.
6. 새로 만든 라우터 js 파일을 기존의 app.js 파일에 임포트 해야 한다. 
const adminRoutes = require(./routes/[새로 만든 라우터 js 파일명].js);’
7. router의 정점은 유효한 미들웨어 함수이다. 그렇기에, 함수처럼 호출하지 않고, 괄호 없이 객체 자제만 입력해도 된다.
app.use(adminRoutes);
코듸의 위치 순서가 중요하므로 주의하자.
8. 새로 만든 여러 라우터 js 파일들에게도 동일하게 적용하여 라우터를 분리하자.

+) get은 정확한  경로를 확인하므로 주의하자. ‘/’일때, ‘/something’으로 안 들어가진다.
+) 코드 위치를 주의하여 경로 순서를 조심하자!

---