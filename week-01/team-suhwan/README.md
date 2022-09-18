# TEAM 수환 - 1주차 RBF

## Node.js

Node.js는 JavaScript를 컴퓨터 환경에서 실행할 수 있도록 하고, V8엔진을 사용해 브라우저에서 사용하는 JavaScript를 보다 다양한 작업을 가능하게 한다.

- 특징
    - variable을 전역으로 노출시킴
    - 서버 사이드 기술
    - 하나의 JS 스레드만 사용
        - 스레드: 운영체제의 프로세스
- Web이 작동할 때, 클라이언트는 요청을 보내고 서버는 응답을 보낸다. 이때 응답은 HTML 형식을 가질 수 있으며, file이나 json 등 다른 종류의 데이터도 가능하다.

## HTTP, HTTPS

- HTTP(Hyper Text Transfer Protocol)
    - 프로토콜: 컴퓨터 내부에서, 또는 컴퓨터 사이에서 데이터의 교환 방식을 정의하는 규칙 체계
        - 기기 간 통신은 교환되는 데이터의 형식에 대해 상호 합의를 요구함
    - HTTP: HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜
        - 웹에서 이루어지는 모든 데이터 교환의 기초이며, 클라이언트-서버 프로토콜(웹브라우저인 수신자 측에 의해 요청이 초기화되는 프로토콜)이기도 하다.
        - 각각의 개별적인 요청들은 서버로 보내지며, 서버는 요청을 처리하고 response라고 불리는 응답을 제공한다. 이 요청과 응답 사이에는 여러 개체들이 있다.
- https(Hyper Text Transfer ProtocolSecure): 컴퓨터 네트워크를 통한 보안 통신에 사용되는 프로토콜
    - 신용 카드 세부 정보와 같은 민감한 정보를 전송할 때 보안 연결이 선호된다.
    - 클라이언트와 서버 간에 데이터가 암호화되도록 함으로써 데이터 전송이 안전해진다.

## REPL

- Read → 사용자 입력 받음
- Eval(evaluate) → 사용자 입력값 평가
- Print → 결과값을 출력
- Loop → 돌아가서 새로운 입력값을 기다리는 과정

- 터미널에 node 사용 ~> REPL 실행
- Ctrl+C → 종료

# sec.3

- 코어 모듈
    - 서버를 생성하거나 http 요청 및 응답 작업
        - http: 서버를 출시하거나 요청을 보내는 작업에 도움을 줌
        - https: 모든 전송 데이터가 암호화되는 SSL 암호화 서비스를 출시할 때 도움을 줌
        - fs
        - path: 경로를 구축하는 데 사용
        - os: 운영체제 관련 도움을 줌
    - createServer: 서버를 생성할 때 꼭 필요한 메서드
        - requestListener: 들어오는 모든 요청을 실행하는 기능
            - 첫 번째 인수 → 요청에 대한 데이터
            두 번째 인수 → 응답에 사용됨
        - 익명함수 사용 가능
        - createServer 메서드가 생성한 서버를 새로운 변수나 상수로 지정해야 함
        - 콜백함수임. → 서버에 요청이 들어올 때마다 Node.js가 호출하게 됨
    - listen() : Node.js가 스크립트를 바로 종료하지 않고 계속 실행되면서 듣도록 하는 기능

- how the web works
    - client → request → server → response →client

- node.js Program Lifecycle
    1. 이벤트 발생 시 이벤트 루프에 등록된 콜백함수들이 실행됨
    2. 무거운 작업들(ex. 파일 시스템 작업)은 Node.js가 자동으로 시작하고  관리하는 워커풀이 처리함
        1. 워커풀은 자바스크립트 스레드와는 분리되어 다른 여러 스레드에서 작동하며 운영체제와 연관이 있다.
    3. 워커풀의 워커가 작업이 끝나면 해당 작업의 트리거 콜백이 실행 → 모든 콜백은 이벤트 루프가 관리하기 때문에 다시 이벤트 루프로 돌아감
    
    - 블로킹/논블로킹
        - 블로킹: 코드의 실행이 다른 코드의 실행을 막는 것
        - 논블로킹: 코드의 실행이 다른 코드의 실행을 막지 않는 것
        - Node.js에서 코드는 논블로킹 방식으로 실행되므로 수많은 콜백 함수와 이벤트를 등록해두면 특정 작업이 끝난 후 Node.js가 코드를 실행시킨다. → js 스레드가 항상 새로운 이벤트나 요청을 다룰 수 있는 이유
    
- 요청을 처리하고 응답을 전송하는 법

```jsx
const server = http.createServer((req, res) =>{
	console.log(req.url, req.method, req.headers);
	
	res.setHeader('Content-Type', 'text/html');
	res.write('<html>');
	res.write('<head><title>My First Page</title></head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});

server.listen(3000);
```

- write() → 데이터를 입력
- end() → 데이터를 모두 입력한 뒤 end()를 호출해서 끝내준다.

- 전송 버튼을 누르면 /message로 POST 요청을 보냄

```jsx
const http = require('http'); //Node.js에 탑재된 글로벌 모듈 http
const fs = require('fs');

const server = http.createServer((req,res)=>{ //요청을 실행하는 server객체
    const url = req.url;
    const method = req.method;

    if(url === '/'){
        res.write('<html>');
    res.write('<head><title>Enter Message</title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end();
    }
    if(url === '/message' && method === 'POST'){
        fs.writeFileSync('message.txt','DUMMY');
        res.statusCode=302;
        res.setHeader('Location','/');
        return res.end();
    }
    res.setHeader('Content-Type','text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //server가 실행 직후 꺼지지 않고 계속 실행되도록 함
```

→ vs code에 message.txt라는 파일이 생기고 그 안에 ‘DUMMY’ 라고 쓰여져 있음.
→ 버튼 눌러서 입력해도 Hello from my Node.js Server! 가 뜨지 않고 그냥 그대로의 상태로 있음.

- 스트림: 지속적인 프로세스
    - 노드가 많은 양의 요청을 한 청크씩 읽고, 다 읽은 시점부터 요청 전체를 읽기까지 기다리지 않고 각각의 청크를 다룰 수 있게 됨
    - 버퍼(Buffer) → 버스 정류장과 같음! 여러 개의 청크를 포함하고 파싱이 끝나기 전에 작업하는 것을 가능하게 한다.
    
- 입력받은 메세지를 파일에 저장

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req,res)=>{
    const url = req.url;
    const method = req.method;

    if(url === '/'){
        res.write('<html>');
    res.write('<head><title>Enter Message</title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end();
    }
    if(url === '/message' && method === 'POST'){
        const body = [];
        req.on('data', (chunk)=>{
            console.log(chunk);
            body.push(chunk);
        });
        req.on('end', ()=>{
            const parsedBody = Buffer.concat(body).toString();
            const message = parsedBody.split('=')[1];
            fs.writeFileSync('message.txt',message);

        })
        res.statusCode=302;
        res.setHeader('Location','/');
        return res.end();
    }
    res.setHeader('Content-Type','text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000);
```

→ message.txt에 입력받은 텍스트가 저장되어 있음

- 코드 진행 순서
    - 코드가 먼저 작성되었다고 해서 먼저 실행되는 것이 아님.
    
    ```jsx
    req.on('end', ()=>{
                const parsedBody = Buffer.concat(body).toString();
                const message = parsedBody.split('=')[1];
                fs.writeFileSync('message.txt',message);
    
            }) -> 콜백함수
    ```
    
    - 코드나 함수를 등록해 지금 당장이 아니어도 나중에 실행될 수 있도록 하는 것임
    - 이런 설정이 중요한 이유: node.js가 파일 작성 완료 전까지 멈추는데 그러면 서버가 느려지며 완료되기 전까지 들어오는 요청을 포함해 아무것도 처리할 수 없게 됨
    

```
fs.writeFileSync('message.txt',message);
```

→ Sync: 동기화를 의미
- 이 파일이 생성되기 전까지 다음 코드 실행을 막는 메서드

- 이벤트루프는 이벤트콜백을 다룬다.
    - 특정 이벤트가 일어났을 때 이벤트루프가 해당 코드를 실행하는 것
    - 시간이 오래걸리는 파일 연산에는 도움되지 않음. →Worker Pool로 보내짐. (무거운 작업 담당)
    js 코드로부터 완전히 분리되어 다른 여러 스레드에서 작동할 수 있으며, 앱을 실행하는 운영체제와 깊은 관련이 있음.
    - this 연산을 다루지 않음. → 오직 완성된 쓰기 파일에 정의한 콜백에 대한 코드들만 정의
    - 즉, 이벤트 루프는 빨리 끝낼 수 있는 코드를 포함한 콜백만을 다룸.
    
- 이벤트 루프
    - Node.js를 계속 실행하도록 하는 루프, 모든 콜백을 처리한다. (콜백을 처리하는 데는 일정한 순서가 있음.)
    - Timers
    타이머를 설정하면 타이머가 끝나면 실행할 함수를 Node.js가 알고있어서 항상 새로운 루프가 반복이 일어날 때 마다 시간이 다 된 콜백을 실행한다.  즉, 타이머가 끝난 콜백을 실행한다. (setTimeOut과 setInterval)
    - Pending Callbacks 
    다른 콜백을 체크한다. → 읽기 및 쓰기 파일의 연산이 끝나 콜백이 있을 수 있는데 이때 이 콜백을 실행한다.
    - Poll 단계
     Node.js가 새로운 I/O(input/output) 이벤트를 찾아 최대한 해당 이벤트의 콜백을 빨리 실행하도록 한다. 가능하지 않다면 실행을 미루고 대기 콜백으로 등록하게 됨.
    - 타이머가 다 되어 실행해야 하는 콜백도 확인하는데, 만약 있다면 timer로 넘어감.
    없다면 loop가 계속됨
    - check
    setImmediate 콜백이 실행됨. setImmediate는 반드시 열린 콜백이 모두 실행된 다음에 실행됨
    - Close Callbacks
    - 프로그램 종료 (process.exit) → 남아있는 이벤트핸들러가 없는 것을 확인해야 함
    
    - 이벤트 루프 참고
    
    [로우 레벨로 살펴보는 Node.js 이벤트 루프](https://evan-moon.github.io/2019/08/01/nodejs-event-loop-workflow/)
    
    # sec.4
    
    - npm: node package manager
        - 노드 프로젝트라고 부르는 작업의 초기 내용을 설정할 수 있음
        - 터미널에 npm init → package.json 파일이 생김
        : 현재 파일 프로젝트의 구성 파일임
    - package.json에서 scripts부분에 “start”: “node app.js” 를 치고 npm start로 실행을 하면 app.js가 실행된다.
        - 본인만의 스크립트를 실행하는 법 → “start-server” : “node app.js”를 치고 npm run start-server로 실행하면 됨
    - nodemon (제 3자 패키지 중 하나) : 자동 재시작 메커니즘
        - npm install nodemon 코드를 치면 다운받을 수 있음
        - Terminal이나 명령줄에는 local dependency가 아니라 글로벌 패키지를 사용하기 때문에, `nodemon app.js`가 작동하지 않음
        - 지역 레벨에서 실행해도 되니 굳이 필요하지는 않지만, 전역 레벨에 `nodemon`을 설치할 수 있다. `npm install -g nodemon`을 이용해서→ 그중 `-g` 플래그는 패키지가 전역 패키지로 추가되게끔 해서, Terminal이나 명령 프롬프트를 포함해 머신 어디에서나 사용할 수 있게 됨
        
    - 오류 유형들
        - Syntax Error(구문 오류) - 코드에 오타, 중괄호 빼먹음 등
        프로젝트를 실행하려고 할 때 자동으로 오류가 등장
        - Runtime Error - 코드를 실행하려고 할 때 멈춰버림
        - Logical Error - 오류메세지가 뜨지 않고 앱이 원하는 대로 작동하지도 않으며 찾아내기도 힘듬