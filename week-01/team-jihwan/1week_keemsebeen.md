# section 3, section 4

## How Does The Web Work ?

- Client → Request → Server → Response → Client
    - 클라이언트인 브라우저가 서버에 요청을 보냄
    - 서버가 파일을 다룬 후 응답을 클라이언트에게 보냄.
    - 결과적으로 브라우저가 이 응답을 보여주게 된다.
    
    ![Untitled](section%203,%20section%204%200f0b1db9fd2048dc83a6e8e2737d6f6b/Untitled.png)
    

## Creating a Node.js Server

- 서버 생성 방법
    
    ```jsx
    const http = require('http');
    // 함수생성
    function rqListener(req,res) {
    }
    http.createServer(rqListener);
    // 익명함수
    http.createServer(function(req,res) {
    });
    ```
    
- require 키워드를 사용해서 다른 파일의 경로나 자바스크립트을 불러 올 수 있다.
- createServer() : 서버를 생성할 때 꼭 필요한 메소드이다.
    - requestListener를 인수로 갖는다.
    - requestListener는  request와 response 두개의 인수를 받는다.  즉, 요청과 응답을 받음.
- listen : node.js가 스크립트를 바로 종료하지 않고 계속 실행되면서 듣도록 한다.  port, hostname, backlog, listeningListener 총 4개의 인수를 받는다 .
    - port  :
    - hostname : 기본적으로 localhost를 사용한다.
        - [localhost](http://localhost) : 내 컴퓨터에 할당된 ip주소.
    - backlog  :
    - listeningListener :

## Using Node Core Modules

- http : 서버 생성, 요청을 보내는 작업에 도움을 준다.
    - require : 다른 파일의 경로나 자바스크립트를 불러 올 수 있다.
- https : 모든 전송 데이터가 암호화되는 ssl 암호화 서버를 생성할 때 도움이 된다.
- fs : 파일을 불러오거나 읽을 수 있다.
- path :
- os : 운영체제 확인, 서버의 아키텍쳐를 구분, 서버의 지역 ip를 확인한다.

## Working with Requests & Responses

- 요청은 해당 포트에 있는 request에 따라 데이터를 받는다.
    - 요청은 header를 보면 알수 있는데, 헤더는 메타데이터 + 브라우저가 요청한 데이터 등을 볼 수 있다.
    - console.log(req.url, req.method, req.headers)
        - req.url을 통해 호스트 다음에 붙는 모든 주소를 알 수 있다.
        - req.headers : header에 대한 값은 그대로지만, req.method의 출력값이 get으로 나온다.
- 응답
    - res.setHeader(’Content-Type’, ‘text/html’) ( + setHerder을 통해 새로운 header를 생성할 수 있다. )
        - Contetnt-Type은 브라우저가 알고 이해하며 받아들이는 디폴트 헤더이다.
        - setHeader 안에 헤더 키에 대응하는 값을 설정하고 text/html에 전송하거나 설정할 수 있다.
        - 이렇게 응답을 하게 되면 , 응답에 헤더(메타데이터)를 붙이게 되고 응답의 콘텐츠 유형은 html이라는 것을 메타정보로 전달하게 된다.
    - res.end() : 응답의 생성이 끝난 뒤에 노드에 알리는 역할을 한다. 이후에는 아무것도 입력해서는 안된다. 오류 발생.
        
        ```jsx
        const server = http.createServer((req, res) => {
            console.log(req.url, req.method,req.headers); //요청
            //process.exit();
            res.setHeader('Content-Type', 'text/html');   //응답
            res.write('<html>');
            res.write('<head><title>My first page</title></head>');
            res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
            res.write('</head>');
            res.end();
        });
        
        server.listen(3000);
        //요청과 응답이 연결되어 있지 않은 형태이다. 
        ```
        
- 응답 리디렉션
    
    ```jsx
    const method =req.method;
    if (url ==='/message' && method ==='POST') {
        fs.writeFileSync('message.txt', 'DUMMY');  // 새로운 파일 생성 → 메세지 저장
        //res.writeHead(302,{}) // 한번에 몇가지 메타정보를 설정할 수 있게 함
    		res.statusCode=302; // 경로 재지정
        res.setHeader('Location', '/'); 
        return res.end();
      }
    ```
    
- 요청 본문 분석
    - req.on(’data’, (chunk) ⇒ ) 을 활용해서 특정 이벤트를 들을 수 있도록 함.
    - 첫번째 인자로는 사용할 이벤트를 입력. 이때 이벤트가 발생하는 데에 버퍼가 도움을 줌
    - 두번째 인자로는 모든 데이터 이벤트에 실행될 함수를 넣어주면 됨.
    - chunk 사용해서 상호작용함
    - Buffer.concat(body).toString(); 을 통해서 들어온 버퍼를 문자열로 반환해줌.

## Asynchronous Code & The Event Loop

- node.js는 거의 비동기적으로 작동한다.
- Event loop
    - node.js가 시작되면 프로그램에 의해 event  loop는 자동으로 실행이 된다 .
    - 모든 콜백을 처리하는데, 콜백을 처리하는데도 일정한 순서가 존재한다.
        - ① Timers : 새로운 반복이 시작될 때 마다 실행해야하는 timer callback이 있는지 확인한다. (setTimeout, setInterval)
        - ② Pending Callbacks : 다른 콜백 함수를 처리한다. 입출력 + 네트워크와 관련된 콜백함수를 처리한다. ( 남은 콜백이 존재할 수 있는데 이 경우는 다음 콜백에서 실행하도록 미룬다고 한다. )
        - ③ Poll : node.js가 새로운 I/O이벤트를 찾아 해당이벤트의 콜백을 빨리 실행하도록 한다 . ( 이 경우에서는 실행을 미루기보다는 대기 콜백으로 등록한다고 한다. )
        - ④ Check : setImmediate이 실행된다. setTimeout, setInterval처럼 바로 실행되기는 하지만 반드시! 열린 콜백이 모두 실행된 다음 실행된다.
        - ⑤ Close Callbacks : 닫혀있는 콜백이 등록되어 있다면 주기의 마지막 단계에서 실행이 진행된다.
        - ⑥ process.exit : 등록한 이벤트 핸들러가 남아 있지 않은지 확인한후 , 프로그램을 종료한다.
    - event loop는 콜백을 다루기 때문에, 빨리 끝낼 수 있는 코드를 포함한 콜백만 다룬다.   (시간이 오래걸리는 파일 연산에는 도움이 되지 않기 때문이다. )
    - 오래 걸리는 연산은 worker pool이 다루게된다. (마찬가지로 node.js가 자동으로 시작하고 관리함.)
        - worker pool은 운영체제와 연관이 있으며, 무거운 작업을 하기 때문에 javascript 코드로부터 완전히 분리된다고 한다.
        - worker가 작업을 마치면 (ex. 파일 읽기) 해당 작업에 대한 콜백이 실행되는데 event loop가 이벤트와 콜백을 책임지기 때문에 결국 이벤트 루프로 들어가게 된다.

## nodemon package

- 파일 수정 시 자동으로 서버를 재시작해주는 패키지이다. ( + 코드를 수정할 때마다 서버를 종료하고 재시작하는 과정이 필요하지 않게 되는 것이다.)
- 서버에서 실행중인 앱에 도움이 되는 패키지
- 개발에 도움이 되는 패키지( Nodemon의 경우 개발 과정에서만 사용한다. )
- —save : 프로덕션 의존성. 코드안에서 사용하고 작업
- -dev :단순히 개발 도중 사용
- -g :전역에 설치, 현재 프로젝트 외에 모든 프로젝트에 적용