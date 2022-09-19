# Section 3

## NVM 이 무엇인가?

[https://codepathfinder.com/entry/NVM-Nodejs-버전-변경하기](https://codepathfinder.com/entry/NVM-Nodejs-%EB%B2%84%EC%A0%84-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0)

node version manager.

# 25. 웹 작동 방식

유저/클라이언트 → URL 입력

URL 에 맞는 도메인을 찾아서 연결해준다.

브라우저는 서버의 IP 주소로 요청을 보낸다.

서버는 HTML이나 데이터로 응답을 보냄.

### Node.js 가 필요한 이유?

서버를 구성하게 되는 코드.

요청과 응답이 프로토콜로 이루어짐.

HTTP: 어떤 전송이 유효한지를 결정

HTTPS: HTTP에 보안을 추가.

# 26. Node 서버 생성

코어 모듈 사용.

http → 서버 생성, 요청 보냄.

https → SSL 서버

전역으로 expose 하는 특성.

Require 함수 사용해서 import

### createServer()

requestListener를 인자로 가짐. 들어오는 모든 요청을 실행하는 기능

requestListener 는 IncomingMessage 와 ServerResponse 를 받음.

익명함수를 만들어서 실행 가능

function(req, res), (req, res) ⇒ ()

콜백함수라 부른다.

createServer를 호출하면 서버를 반환하기에 이를 새로운 변수에 저장해두어야함.

그렇지 않으면 생성된 서버의 위치를 특정할 수 없어서 무언가 실행이 불가능함.

### listen()

반환된 서버에 listen이라는 메서드 사용.

스크립트가 계속 실행되면서 요청을 듣도록 함.

인자로 포트를 받음. 포트는 1000번 대로 가능.

5 면 안될까? (문제 없이 돌아간다. 약속으로 정해둔거 아닐까 생각함)

listen으로 요청을 받도록 하고, local host 포트를 브라우저에서 열면 로딩중 표시가 계속 돌아감.

프로세스가 끝나지 않고 계속 돌아가게 해뒀기 때문

콘솔에는 요청 로그가 남아있음.

```jsx
const http = require("http");

const server = http.createServer((req, res) => {
  console.log(req);
});

const port = 3000;
server.listen(port);
```

# 27. Node의 라이프 사이클 및 이벤트 루프

위에서 만든 서버는 응답을 하지 못함.

node app.js → 스크립트 시작 → 전체 코드 읽고, 실행 → 이벤트 루프

작업이 남아있는 한 계속해서 작동함.

이벤트 리스너가 있는 한 멈추지 않음.

위에서는 요청 리스너를 제거하지 않았기 때문에 돌아감.

코어 노드 어플리케이션은 이벤트 루프에 의해 돌아감.

단일 스레드 JS 사용하기 때문.

수 많은 요청을 서버에서 받는 동시에 멈추면 안되기에 이벤트 루프가 작동

process.exit() 로 제거.

# 28. Node.js 프로세스 제어

node.js 서버 종료 ctrl + c

# 29. 요청의 이해

터미널에 찍히는 요청 객체는 우리가 로컬 호스트로 서버에 접속할 때 보낸 데이터를 통해 노드가 만들어낸 객체.

header 는 메타데이터.

요청 및 응답에 추가됨.

URL, 메서드, 해더 등 지정해서 가져올 수 있음.

강의에서는 쿠키가 등장하던데 해볼때는 안됨,,

# 30. 응답 전송

GET 메서드가 기본

응답 객체를 통해 데이터를 보냄.

우리가 반송하기 윈하는 데이터로 채울 수 있게 해주는 메서드들.

### res.setHeader()

새로운 헤더를 설정할 수 있게 해줌.

메타 데이터 전달함.

### res.write()

html 코드 전달.

end() 로 마무리. 이후에 더 입력하면 오류.

![스크린샷 2022-09-18 오후 3.35.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/101daa5e-7e6c-4ed7-a708-e43f1bd57622/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-09-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.35.57.png)

헤더에서 HTML 타입이라는 것을 알려주지 않는다면 인식하지 못함.

페이지 로딩이 되지 않음.

# 31. 요청과 응답 헤더

[https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

# 32. 라우터 요청

유저가 데이터를 입력하는 페이지를 로드하여

데이터가 전송된 뒤에는 서버에 있는 페이지에 저장.

GET: 자동으로 전송됨

POST: 해당 양식을 생성하고 설정해야됨.

# 33. 요청 리디렉션

POST 요청을 보낼때 무언가 바뀌게 해본다.

# 34. 요청 본문 분석

들어오는 요청을 분석해서 요청의 일부인 데이터를 알아본다.

데이터에 접근하는 방법?

들어오는 요청이 데이터 스트림으로 보내줌.

### Streams & Buffers

incoming request

Stream 에서 리퀘스트를 파싱.

버퍼: 버스정류장?

여러개의 청크를 가지고 파싱이 끝나기 전에 작업하게 해줌.

아직까지는 이해하기 어렵지만 Express 같은 툴 사용하면 간소화 됨.

# 35. 이벤트 기반 코드 실행의 이해

코드를 작성한 순서대로 진행되는 것이 아니라는 점.

응답을 보냈다고 해도 이벤트 리스너가 꺼지는 것이 아님.

함수 안에 넣은 함수가 나중에 실행되는 경우가 많음. 비동기식 실행이 일어남.

해당함수를 바로 실행하지 않고 내부적으로 이벤트 리스너 하나 추가.

콜백함수 이해하는 것이 중요.

# 36. 블로킹 및 논블로킹 코드

writeFileSync → Sync 는 동기화를 의미.

이 파일이 생성되기 전까지 코드 실행을 막는 특별한 메서드.

동기화: 파일이 완료될 때까지 코드의 다음 라인이 실행되지 않도록 하는 모드

그걸 막으면 다른 유저들이 들어오는 상황에도 진행이 되지 않음.

함수 안에 함수를 넣어서 이벤트 루프를 계속한다.

코드나 서버를 막을 일이 없이 운영체제에게 할 일을 던져주고, 다시 돌아와서 작업한다.

```jsx
const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === "/") {
    res.write("<html>");
    res.write("<head><title>Hello World</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
    );
    res.write("</html>");
    return res.end();
  }
  if (url === "/message" && method === "POST") {
    // 경로 재지정과 파일 생성.
    const body = [];
    req.on("data", (chunk) => {
      console.log(chunk);
      body.push(chunk);
    }); //요청 데이터 받음 on: 특정 이벤트 받게 해줌. 오는 데이터에 대응할 함수 만들어야됨.
    req.on("end", () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split("=")[1];
      fs.writeFile("message.txt", message, (err) => {
        res.statusCode = 302;
        res.setHeader("Location", "/");
        return res.end();
      });
    });
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>Hello World</title></head>");
  res.write("<body><h1>hello from my node js server</h1></body>");
  res.write("</html>");
  res.end();
});

const port = 3000;
server.listen(port);
```

# 37. Node.js 백그라운드 확인

### Single Thread, Event Loop & Blocking Code

자바 스크립트의 싱글 스레드 사용.

보안문제? 성능문제? 다양한 문제 제기됨.

파일에 대한 요청이 들어올때 뒤의 코드들이 딜레이 되면 다운될 수도 있음.

이벤트 루프가 모든 콜백을 파악하고 코드를 실행함.

파일연산 자체는 이벤트 루프가 해결해주지 못함.

이벤트 루프는 콜백과 빠르게 실행되는 함수만 가져갈 수 있다.

오래 걸리는 작업은 워커풀로 보내진다. 무거운 작업을 담당함.

워커풀은 코드에서 떨어져 다른 여러 스레드에서 작동 가능함.

워커가 작업을 마치면 콜백이 시작되는데, 이 때 작업은 이벤트 루프로 넘겨짐.

내장된 기능.

### Event Loop

콜백을 처리하는 순서가 있음.

실행해야하는 타이머 콜백이 있는지 확인함.

타이머가 끝나면 실행될 함수 실행

연산 이후의 콜백 실행.

너무 많은 콜백이 쌓이게 되면 루프 반복을 이어나가지 않고 다음 반복에서 처리함.

열린 콜백을 다 처리하면 Poll 단계로 들어감.

위의 단계를 반복

새로운 I/O 이벤트 찾음.

check 단계 : setImmediate()

close 이벤트 콜백 실행.

process.exit 하기 전에는 이벤트 리스너가 없는지 확인해야됨.

# 38. Node 모듈 시스템 사용

새로운 파일에 원하는 기능 넣고 export.

# 39. 마무리

### 웹의 작동 방식

클라이언트 → 요청 → 서버 → 응답 → 클라이언트

### 프로그램 라이프 사이클 & 이벤트 루프

Node.js : non-blocking 코드와 event-driven 코드로 싱글스레드임에도 다양한 작업 완료

더이상 할 일이 없으면 프로그램 종료

createServer 는 끝나지 않는다.

### 비동기 코드

JS는 논블로킹

콜백과 이벤트로 막힘 없이 작업 처리.

### 요청 & 응답

- 스트림과 버퍼로 청크와 데이터를 다룸
- 이중 응답을 주의해야됨.

### Node.js & 코어 모듈

- 다양한 코어 모듈로 서버에 다양한 작업 가능함.
- import via require(’module’)

### Node.js 모듈 시스템

Import via require(’./path-to-file’)

Export via module.exports
