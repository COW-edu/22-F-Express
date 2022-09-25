# TEAM 수환 - 1주차 RBF

## Node.js

Node.js는 JavaScript를 컴퓨터 환경에서 실행할 수 있도록 하고, V8엔진을 사용해 브라우저에서 사용하는 JavaScript를 보다 다양한 작업을 가능하게 한다.

- V8엔진
    - 웹 브라우저를 만드는 데 기반을 제공하는 오픈 소스 자바스크립트 엔진
    - 구글 크롬 브라우저와 안드로이드 브라우저에 탑재되어 있음
- Node.js 특징
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
    - HTTP 특징
        - **대부분의 파일 형식의 전송이 가능**하다.
            - JSON, Text, 이미지, 음성파일 등
        - 클라이언트-서버 구조이다.
            - 클라이언트-서버 구조: 클라이언트의 요청이 있을 때 서버가 응답하는 **단방향 통신**
            - 서버는 클라이언트에 요청을 하지 않으며, 클라이언트의 요청에 대한 응답만을 한다.
            
            ![image](https://user-images.githubusercontent.com/102955516/191176528-fc49d548-27b1-4cd9-b744-29e2b209735f.png)

            
            - 클라이언트의 요청에 대한 서버의 응답에는 요청 처리 결과에 따라 응답 코드가 다르게 온다. 따라서 응답 코드 별로 처리 로직을 만들어 서버의 상황에 대한 대응이 가능해진다.
        - Stateless
            - HTTP 통신에서 서버는 클라이언트의 상태를 저장하지 않는다. = 클라이언트가 이전에 했던 요청이 무엇인지에 따라 반응이 달라지지 않는다.
            - 보통 대규모의 트래픽이 발생하는 서비스에는 서버가 여러 대가 있는데, 서버가 많아지면 많아질 수록 서로 간에 정보를 공유하기 위한 비용이 비싸진다. → **Stateless하게 사용할 경우에 정보 공유가 최소화되어 정보를 공유하기 위한 비용을 최소화할 수 있다.**
        - Connectionless
            - **HTTP통신은 연결을 유지하지 않는 것을 기본 동작으로 가진다.** (별도의 옵션을 설정하면 일정기간동안 유지하게 할 수도 있긴 함)
            - 연결을 유지하게 되면 지속적으로 리소스가 사용되기 때문에 연결 유지는 최소화하는 것이 좋다.
            - HTTP 통신의 초기에는 서버가 응답한 후 사용자의 연결을 바로 끊어버렸으나, 최근에는 연결을 맺고 끊는 비용이 비싸다는 이유로 일정 기간 동안 연결을 유지하는 방식으로 통신이 가능해졌다.
    - HTTP 메서드
        - 요청 메서드
            - GET : 요청받은 URI의 정보를 검색해 응답한다.
                - URI(Uniform Resource Identifier) : 인터넷에 있는 자원을 나타내는 유일한 주소
            - POST: 요청된 자원을 생성한다.
            - PUT: 요청된 자원을 수정한다.
            - PATCH : PUT과 유사하게 수정할 때 사용하는데 해당 자원의 일부를 교체할 때 사용한다.
            - HEAD: 메세지 헤더를 취득한다.
            - OPTIONS: 제공하고 있는 메소드를 문의한다.
            - DELETE: 요청된 자원을 삭제한다.
    - HTTP 상태 코드
        - 200 (OK) → 요청이 성공적으로 수행됨
        - 404 (Not Found) → 요청한 페이지 없음
        - 400 (Bad Request) → 사용자의 잘못된 요청
        - 500 (Internal Server Error) → 서버 내부 에러
        
- HTTP 보안 문제
    - HTTP는 통신 상대를 확인하지 않음
        - 상대방이 누구인지 확인하려는 처리가 없기 때문에 누구든지 요청을 보낼 수 있고, 요청 대상이 접근 권한 처리를 하지 않은 경우 응답을 반환한다.
        - 의미 없는 요청도 수신하기 때문에 디도스 공격에도 취약하다.
    - 암호화하지 않은 통신이기 때문에 도청이 가능
    - 완전성(=정보의 정확성)을 보장할 수 없기 때문에 변조가 가능
        - 공격자가 도중에 요청이나 응답을 빼앗아 변조하는 중간자 공격에 취약
        

<aside>
💡 이러한 문제들을 방지하기 위해 아래의 HTTPS 기술이 나오게 됨!

</aside>

- HTTPS(Hyper Text Transfer ProtocolSecure): 컴퓨터 네트워크를 통한 보안 통신에 사용되는 프로토콜
    - 신용 카드 세부 정보와 같은 민감한 정보를 전송할 때 보안 연결이 선호된다.
    - 클라이언트와 서버 간에 데이터가 암호화되도록 함으로써 데이터 전송이 안전해진다.
    - HTTPS 암호화
        - HTTP를 SSL을 사용해 데이터를 둘러싼다.
            - CA(Certificate Authority): 암호화에 사용되는 키를 저장해주는 신뢰성이 검증된 민간 기업
            - CA의 공개키들을 브라우저가 알고 있음
            - 실제 HTTP로 데이터가 전송되기 전에 SSL 인증서를 서로 확인하는 과정을 가짐
                - 사용자가 HTTPS로 된 웹사이트에 접속 → 사용자의 브라우저(EX.크롬)는 이 사이트를 제공해주는 웹 서버와 서로 SSL 인증서를 교환한다.
                - 웹 서버와 브라우저는 서로 키를 교환하게 되며, 이때 브라우저가 키를 이용해 이들만의 예비 마스터키를 생성한다. → **데이터를 암복호화** 함.
                - 웹 서버도 마스터키를 획득하게 된다면 이를 통해 데이터를 암복호화 할 수 있게 되어 안전하게 데이터가 전송될 수 있다.
                    - 암호화: 의미를 알 수 없는 형식(암호문)으로 정보를 변환하는 것. 암호문의 형태로 정보를 기억 장치에 저장하거나 통신 회선을 통해 전송함으로써 정보를 보호할 수 있다.
                    - 복호화: 복호 키를 사용하여 원래의 정보를 복원하는 것을 말한다. 복호 키를 갖고 있는 사람 외에는 올바른 정보로 복원할 수 없으므로, 복호 키가 제3자에게 알려지지 않으면 정보는 보호된다.

## REPL

인터프리터 언어에서 주로 사용되는 **컴퓨터 프로그래밍 언어 사용자 환경**

- Read → 사용자 입력 받음
- Eval(evaluate) → 사용자 입력값 평가
- Print → 결과값을 출력
- Loop → 돌아가서 새로운 입력값을 기다리는 과정

- 터미널에 node 사용하는 것 ~> REPL 실행
- Ctrl+C → 현재 종료
- Ctrl+C (2번) → Node REPL 종료

- 참고링크

[[Node.JS] 강좌 04편: REPL 터미널](https://velopert.com/235)

# sec.3

- 코어 모듈
    - 서버를 생성하거나 http 요청 및 응답 작업
        - http: 서버를 출시하거나 요청을 보내는 작업에 도움을 줌
        - https: 모든 전송 데이터가 암호화되는 SSL 암호화 서비스를 출시할 때 도움을 줌
        - fs: 파일 입출력 처리를 할 때 사용
            - Node.js에 내장되어 있기 때문에 별도의 라이브러리 설치 없이 바로 불러와서 사용할 수 있음
        - path: 경로를 구축하는 데 사용
        - os: 운영체제 관련 도움을 줌

- createServer: 서버를 생성할 때 꼭 필요한 메서드
    
    ```
    const http = require('http');
    
    const server = http.createServer((req, res) => {
       console.log(req);
    });
    
    server.listen(3000);
    
    ```
    
    → require: 다른 파일로의 경로나 js 파일을 불러올 수 있는데, 위의 코드에서 require 키워드는 http라는 글로벌 모듈을 찾게 됨
    
    → createServer는 서버를 생성합니다. 위의 코드에서는 http 모듈에서 불러온 http 객체가 서버를 생성하게 됩니다. 그리고 createServer 메소드는 requestListener를 인수로 가짐
    
    → requestListener: 들어오는 요청에 대하여 실행되는 콜백 함수로, 들어오는 메세지에 대한 정보를 담은 request 객체와 응답에 대한 정보를 담은 response 객체를 인자로 받음
    
    → listen() : Node.js가 스크립트를 바로 종료하지 않고 계속 실행되면서 듣도록 하는 기능
    

- how the web works
    - client → request → server → response →client

- node.js Program Lifecycle

![image](https://user-images.githubusercontent.com/102955516/191176640-39bd015d-e729-435d-9fd3-e8816938c3df.png)


1. 이벤트 발생 시 이벤트 루프에 등록된 콜백함수들이 실행됨
2. 무거운 작업들(ex. 파일 시스템 작업)은 Node.js가 자동으로 시작하고  관리하는 워커풀이 처리함
    1. 워커풀은 자바스크립트 스레드와는 분리되어 다른 여러 스레드에서 작동하며 운영체제와 연관이 있음
3. 워커풀의 워커가 작업이 끝나면 해당 작업의 트리거 콜백이 실행 → 모든 콜백은 이벤트 루프가 관리하기 때문에 다시 이벤트 루프로 돌아감

- 블로킹/논블로킹
    - 제어권: 함수의 코드를 실행할 권리
        - 제어권을 가진 함수는 자신의 코드를 끝까지 실행한 후, 자신을 호출한 함수에게 돌려줍니다.
    - 블로킹: 코드의 실행이 다른 코드의 실행을 막는 것
        - A 함수가 B 함수를 호출하면 B 함수에게 제어권을 넘겨줌 → 제어권이 없는 A 함수는 잠시 실행을 멈추고 B가 함수를 실행하게 됨 → B 함수의 실행이 끝나면 자신을 호출한 A에게 제어권을 돌려줌
    - 논블로킹: 코드의 실행이 다른 코드의 실행을 막지 않는 것
        - A 함수가 B 함수를 호출해도 제어권은 그대로 A가 가지고 있음 → B 함수가 호출된 뒤에도 여전히 A 함수는 자신의 코드를 그대로 실행
        - Node.js에서 코드는 논블로킹 방식으로 실행되므로 수많은 콜백 함수와 이벤트를 등록해두면 특정 작업이 끝난 후 Node.js가 코드를 실행시킨다. → js 스레드가 항상 새로운 이벤트나 요청을 다룰 수 있는 이유
    - 동기: 호출하는 함수 A가 함수 B의 리턴값을 기다리거나 스스로 계속 확인하는 것
    - 비동기: 함수 A가 함수 B를 호출할 때 콜백 함수를 함께 전달 → 함수 B의 작업이 완료되면 함께 보낸 콜백 함수를 실행하는 것입니다. 함수 A는 B의 작업 완료 여부를 신경쓰지 않음

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
/*const http = require('http'); //Node.js에 탑재된 글로벌 모듈 http
const fs = require('fs');
*/ -> ES5 방식

import http from 'http';
import fs from 'fs';  
->ES6 방식

const server = http.createServer((req,res)=>{ //요청을 실행하는 server객체
    const url = req.url;
    const method = req.method;

    if(url === '/'){
	    res.write(`<html>
	    <head><title>Enter Message</title></head>
	    <body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>
	    </html>`);
    return res.end();
    }
    if(url === '/message' && method === 'POST'){
        fs.writeFileSync('message.txt','DUMMY');
        res.statusCode=302;
        res.setHeader('Location','/');
        return res.end();
    }
    res.setHeader('Content-Type','text/html');
    res.write(`<html>
    <head><title>My First Page</title></head>
    <body><h1>Hello from my Node.js Server!</h1></body>
    </html>`);
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
import http from 'http';
import fs from 'fs'; 

const server = http.createServer((req,res)=>{
    const url = req.url;
    const method = req.method;

    if(url === '/'){
	    res.write(`<html>
	    <head><title>Enter Message</title></head>
	    <body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>
	    </html>`);
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
    res.write(`<html>
    <head><title>My First Page</title></head>
    <body><h1>Hello from my Node.js Server!</h1></body>
    </html>`);
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

→ 동기는 시간이 얼마나 걸리든 요청 결과를 받아야하기때문에, 큰 data를 취급하는 파일에서는 파일이 생성될 때 까지 다음 코드를 실행하지 못하고 대기해야한다는 단점이 존재함

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
            - 지역으로 설치되는 경우에는 프로젝트 루트 디렉터리에 `node_module`
             디렉터리가 자동 생성되고 그 안에 패키지가 설치되어 해당 프로젝트 내에서만 사용할 수 있음
            - 전역으로 패키지가 설치되는 경우에는 전역에서 참조가 가능하기 때문에 모든 프로젝트에서 공통으로 사용하는 패키지는 전역에 설치하는 것이 유리
        
    - 오류 유형들
        - Syntax Error(구문 오류) - 코드에 오타, 중괄호 빼먹음 등
        프로젝트를 실행하려고 할 때 자동으로 오류가 등장
        - Runtime Error - 코드를 실행하려고 할 때 멈춰버림
        - Logical Error - 오류메세지가 뜨지 않고 앱이 원하는 대로 작동하지도 않으며 찾아내기도 힘듬
    
    # 과제
    

**NVM**(Node Version Manager) : Node.js의 버전을 관리하는 도구

- 사용 목적 : 협업 및 프로젝트 동시 진행 등의 상황에서 라이브러리 / 프레임 워크/ 개발 툴 등의 버전 호환 문제를 방지할 수 있음
- 특징
    - 다양한 버전의 Node.js를 설치 및 버전 간의 스위칭이 간단하게 가능해짐 → 버전 관리가 쉬워짐
    - 루비의 rvm, rbenv, 파이썬의 pyenv가 같은 역할
