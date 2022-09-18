# NodeJS_Section 3: 기본 개념 이해

2022-09-12 ~ 2022-09-18

# Node 서버 생성

## Node.js 호출

```jsx
const http = require('http');
function rqListener(req, res){
};
http.createServer(rqListener);
```

const 란?

| var, let | 변수 선언 키워드 | 리터럴 값 재할당 가능 |
| --- | --- | --- |
| const | 상수 선언 키워드 | 리터럴 값 재할당 불가능
즉, 절대 변경하지 않을 값일때 사용
→ 선언과 동시에 리터럴 값 할당
Ex) const countNumber = 23;
(23이 리터럴을 의미) |

require 란?

다른 파일(Ex. JavaScript, 코어 모듈인 http)의 경로를 불러올 수 있는 함수

http.

- http 모듈의 기능(JS 객체의 매서드/속성) 접근 가능

createServer 

- 서버 생성 시 꼭 필요한 매서드
- 모든 요청을 실행하는 기능인 requestListener 인자 필요

requestListener

- 첫 번째 인수에는, 요청 데이터인 Request 인자 필요
- 두 번째 인수에는, 응답에 사용되는 Response 인자 필요

→ requestListener를 사용하여 명확하게 생성하지 않고,

간단히 익명 함수(이름이 없는 함수)를 사용하여도 된다.

이러한 방식을 event driven architecture(EDA)라 한다.

function도 생략 가능하다.

이를 createServer 콜백 함수라고 부른다.

```jsx
const http = require('http');

http.createServer(function(req, res){
});
```

이제, 서버에 요청이 들어올 때마다 Node.js가 호출된다.

---

## 요청하기

요청 객체를 console.log 함수에 넣어 무엇이 있는지 알아보자.

```jsx
const http = require('http');
const server = http.createServer((req, res) => {
    console.log(req); //동기함수
 })

server.listen(3000);
```

1. createServer 메서드가 생성한 서버를 저장(선언)
2. 요청 객체를 console.log 함수에 넣어 안에 뭐가 있는지 확인해보자.
3. 해당 서버를 호출
    - Node.js가 스크립트를 바로 종료하지 않고 계속 실행되며 듣도록 한다.
    - 여러개 인수 중, 첫 번째 인수는 듣고자 하는 포트이다.
    - 호스트 이름도 저장해야 한다. 기본적으로 로컬 머신의 호스트 이름은 losthost.
    - 포트 번호는 대개 1000대 번호면 괜찮다.
4. 터미널에 “node app.js” 작성
5. 커서가 넘어가지 않는다. 왜냐하면, 프로세스가 아직 작동 중인 것으로 파일의 실행이 아직 끝나지 않았기 때문이다. 루핑 프로세스를 통해 계속 듣도록 설정했기 때문이다.
6. 브라우저에 [localhost:3000](http://localhost:3000) 입력.
7. 브라우저에서는 아무일도 일어나지 않는다. 특정 html 페이지를 반환하도록 구성하지 않았기 때문이다.
8. 다시 터미널로 돌아와보면 출력 결과가 나타난다. console.log로 인해 콘솔에 요청 로그가 들어왔기 때문이다.
9. 이제, 요청 내용과 출력 결과를 작성해보자.

+) console.log 다음

process.exit();를 작성해주면,

[localhost:3000](http://localhost:3000) 이동 후 터미널로 돌아오면 다음 줄로 넘어가 있다.

즉, 이제 중지되었다는 뜻이다.

대개는 서버를 중지하지 않기 때문에 process.exit();을 호출할 일은 없다.

---

## 요총 처리 및 응답 전송

```jsx
const http = require('http');
const server = http.createServer((req, res) => {
    console.log(req.url, req.method, req.geaders); //요청
    
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
    res.write('</html>');
    res.end();
})

server.listen(3000);
```

1. res를 호출하고, 새로운 헤더를 설정해보자.
    - res.setHeader()
    - Contet-Type은 브라우저가 알고 이해하며 받아들이는 다폴트(default)헤더이다.
    - 두 번째 인수는 헤더 키에 대응하는 값을 설정한다.
    text/html에 전송할 수 있다.
        
        → 응답(res)에 헤더를 붙이게 되고, 응답의 일부 콘텐츠는 HTML이라는 메타 정보를 전달하게 된다. 
        
2. res에 write 를 사용하여 데이터를 기록해보자.
    - res.write(’<html>’);
    - …
3. 응답의 생성이 끝난 뒤에는 node에게 알려주자.
    - res.end();
    - 더 이상 입력해서는 안 된다.
    - write를 호출 할 수는 있지만 오류가 발생한다.
4. 개발자 도구를 열어 확인해보자.
    - Network 탭의  Headers(머리글)에서, 설정한 콘텐츠 유형을 확인할 수 있다.
    - Response(응답)에서는, HTML 문서 코드 등을 확인할 수 있다.

---

## 요청과 응답 연결하기

 일부 요청 데이터를 콘솔에 출력할 경우 별다른 효과가 없으므로 다양한 기능을 수행하는 웹 서버를 입력해보자.

 입력하는 루트에 따라 기능이 달라지므로, / 뒤에 무엇을 입력하느냐가 기능을 좌우한다.

 “/”만 입력하면, 유저가 데이터를 입력하는 페이지를 로드한다. 데이터가 전송되면, 서버에 있는 파일에 저장하게 된다.

```jsx
const http = require('http');
const server = http.createServer((req, res) => {
		const url = req.url;
    if (url === '/') {
        res.write('<html>');
        res.write('<head><title>Enter Message</title></head>');
        res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
        return res.write('</html>');
    }
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
    res.write('</html>');
    res.end(); //더 이상 입력해서는 안 된다.
})

server.listen(3000);
```

1. URL 파싱하기.
    - const url = req.rul;
    - if문으로 url이 / 와 동일하며 이 조합만이 일치하는 것을 확인하자.
2. 응답 반환하기. 사용자에게 요청을 받는 입력 양식과 요청 발송 버튼
    - title 바꾸기
    - form 태그에, 요청이 전달될 URL을 추가하는 action, http 메서드는 POST로 정의
    +) get은 기본값으로 링크를 클릭하거나 url 입력 시 자동 전송, post는 해당 양식을 생성함으로써 개발자가 설정해줘야 한다.
    +) form의 좋은 점은 단순히 요청을 전달할 뿐만 아니라 폼을 살펴보고 다른 요소를 탐지한다.
    - input 태그로 text 받기
    - button 태그에 제출 유형 작성
    - return은 응답 반환 시 꼭 필요한 부분은 아니지만 익명 함수에 반환하여 함수 실행 코드를 중단한다.
3. [localhost:3000](http://localhost:3000) 실행 시,  입력과 send 버튼이 나타난다. 뭔가 입력하고 send 버튼을 누르면 if 문이 종료되고 다음 코드가 나타난다. 이제 url이 /message로 변경되어있다.

---

## 요청 리디렉션

요청 처리하기

```jsx
const http = require('http');
const fs = require('fs');
const server = http.createServer((req, res) => {
		const url = req.url;
    const method = req.method;   
    if (url === '/') {
        res.write('<html>');
        res.write('<head><title>Enter Message</title></head>');
        res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
        return res.write('</html>');
    }
    if (url === '/message' && method === 'POST') {
        fs.writeFileSync('message.txt', 'DUMMY');
        res.statusCode = 302;
        res.setHeader('Location', '/');
        return res.end();
    }
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
    res.write('</html>');
    res.end(); //더 이상 입력해서는 안 된다.
})

server.listen(3000);
```

1. get 요청 말고 POST 요청 다루는 메서드 파싱
2. 조건에 맞는 if 문 작성하기
3. 경로 재지정 및 파일 생성하기.
    - 파일 시스템 코어 모듈 작성
    const fs = require('fs');
    fs를 통해 파일 시스템을 작업할 수 있다.

---

## 요청 데이터

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === '/') {
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end();
  }
  if (url === '/message' && method === 'POST') {
    const body = [];
    req.on('data', (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });
    req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split('=')[1];
      fs.writeFileSync('message.txt', message);
    });
    res.statusCode = 302;
    res.setHeader('Location', '/');
    return res.end();
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title><head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});

server.listen(3000);
```

1. 직접 서버를 생성할 수 있는 on 메서드
    - 새 청크가 읽힐 준비가 될 때마다 data 이벤트가 발생하는 데에 버퍼가 도움을 준다.
    - 두 번째 인자로, 모든 데이터 이벤트에 실행될 함수 작성하기
    - on(data)를 호출하면 리스너가 데이터 chunk를 받도록 해야한다.
2. 본문 읽을 const body 작성
    - 빈 배열이어야 한다.
3. 상수는 새로운 값을 설정할 수 없으므로 ‘body =’ 이 아니라, push를 사용해야 한다. 
4. 다른 이벤트 리스너 ‘end’ 등록하기
5. 모든 청크에 기반하는 함수 ()
6. 버퍼 생성
    - 본문 안의 모든 청크가 추가되었다.
    - toString을 호출해 문자열로 전환한다. 버퍼가 생긴 청크에 작헙하는 것이다.
7. 서버를 재시작해보면, 터미널에 두 개의 요소가 나타난다.
8. 새로운 상수 message를 만들고 parsedBody.split에 등호를 넣은 후 결과 배열의 두 번재 요소인 인덱스 1을 요소로 둔다.
등호의 오른쪽 부분에 결과 배열이 들어간다.
9. message.txt 파일에 message라고 입력
10. 파일을 재시작해서 텍스트를 input하고 send 하면, message.txt에 입력한 텍스트가 들어간 것을 확인할 수 있다.

---

## 라우터를 활용해 여러 파일로 만들기

```jsx
const http = require('http');

const routes = require('./routes');

console.log(routes.someText);

const server = http.createServer(routes.handler);

server.listen(3000);
```

1. routes.js라는 새 파일을 생성한다.
2. 기존 app.js의 if문과 기본 응답 코드를 잘라내  routes.js에 작성합ㄴ다.
3. app.js의 http는 아직 필요하므로 둔다.
4. url과 method는 필요하지 않으니 지운다.
5. routes.js의 가장 위에 fs를 추가한다.
6. app.js와 routes.js를 연결하기 위해 routes.js에 requestHandler함수를 생성한다. req와 res를 인수로 설정하면 http.createServer 함수를 대체하게 된다.
7. 코드를 함수로 옮기기 위해 req.url과 req.method의 데이터를 상수로 추가한다.
8. Handler 내보내기
9. app.js에서 require를 통해 라우터 불러오기

---