# 9-18 pr

## Node Js란?

Node.js는 chrome v8 자바스크립트 엔진으로 빌드 된 자바스크립트 런타임이다.

→ 자바스크립트를 서버에서도 사용할 수 있도록 만든 프로그램

→ v8이라는 자바스크립트 엔진 위에서 동작하는 자바스크립트 런타임(환경)이다. ( 스크립트 언어가 아닌 환경!)

## Node JS 사용 이유

자바스크립트를 서버에서도 사용하게 만들기 때문에

→ 한가지 언어로 전체 웹 페이지 만들기 가능!

또한 자바스크립트는 독립적인 언어가 아닌 스크립트 언어 ( 다른 응용 프로그램에 삽입되어서 동작하는 프로그래밍 언어), 웹 브라우저 프로그램 안에서만 동작을 한다 즉 웹 브라우저가 없으면 사용할 수 없는 프로그램이다

왜냐하면 브라우저안에 자바스크립트 해석기가 들어가있음. 그래서 브라우저가 없으면 자바스크립트 해석을 못한다. 그래서 자바스크립트를 브라우저 밖에서도 해석기를 통해 실행 가능하도록 하자 해서 나온게 node.js

( 참고로 애초에 자바스크립트가 브라우저에서 사용하려고 만든건데 그걸 브라우저 밖으로 꺼내려고 하니까 그 해석기를 가지고 있는 친구가 노드라고 생각하면 됨)

[https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)

[https://hanamon.kr/nodejs-개념-이해하기/](https://hanamon.kr/nodejs-%EA%B0%9C%EB%85%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/)

그런데 node.js는 자바스크립트를 웹 브라우저에서 독립시킨 것으로 node.js를 설치하게 되면 브라우저 없이 바로 실행 가능 !

## **이벤트 루프**

node 명령어를 이용해 js 파일을 실행시키면 해당 스크립트를 실행시키게 한다.

nodejs는 event loop라는 특별한 성질 때문에 eventListener가 있는 한 절대로 서버가 꺼지지 않고 해당 event 의 요청을 감지하게 된다

왜→? NodeJS는 싱글 스레드 JS를 실행한다. 수천 수만의 요청을 처리해야하는데 , 서버가 멈춰 있는 도중 들어올 때마다 실행하기를 반복한다면 처리 속도에 안좋다

자바는 멀티 스레드, 자바스크립트는 싱글 스레드다

자바는 멀티 스레드므로 일할 수 있는 애가 여러 명 있다고 볼 수 있다. 그래서 애가 이작업을 하고 쟤가 이 작업을 하고 이런 식으로 한다. 그래서 동기적으로 실행한다

하지만 자바스크립트 싱글 스레드 ! 일할 수 있는 애가 한명 그래서 애는 비동기적으로 실행한다. 이벤트 함수등 이런게 있을 때 이벤트 큐들에게 넘겨준다 ( 모던 딥다이브 자바 스크립트에 있는 내용)

햄버거 가계를 예시로 들면 자바스크립트는 주문 받는 애가 한명 그래서 햄버거 만드는 일을 다른 애한테 넘겨주는 거 하지만 자바는 사람이 여러명이여서 한명이 주문 받고 햄버거 만들고 다 하는거 ( 대신 이런 사람이 여러명 인것)

[https://intrepidgeeks.com/tutorial/js-asynchronous-key-event-loop](https://intrepidgeeks.com/tutorial/js-asynchronous-key-event-loop)

[https://medium.com/zigbang/nodejs-event-loop파헤치기-16e9290f2b30](https://medium.com/zigbang/nodejs-event-loop%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-16e9290f2b30)

## **Header**

**정의**

먼저 http 헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다. 메타 데이터이기도 한데 부가적 정보에는 요청자, 컨텐트 타입, 캐싱 등등이 있다

**종류**

Header에는 4가지 종류가 있다

**General header** : 요청과 응답 모두에 적용되지만 바디에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더.

**Request header** : 페치될 리소스나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더. == 내가 보내는 메세지의 헤더

**Response header** : 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더. == 내가 받은 메세지의 헤더

**Entity header**: 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더. ( 메세지 바디에 관련된 정보)

일반헤더와 엔티티 헤더는 응답 요청에서 공통적으로 사용될 수 있다.

엔티티 헤더에서는 content-type이라고 payload의 데이터 타입을 나타내는 것이 있다

이것은 accept 헤더와 대응한다

### chunk

→데이터 조각

### buffer

→ chunk를 받아주는 용기와 같다 즉 chunk들을 buffer에 채운 후 다 참녀 buffer을 통째로 옮기고 새 buffer에 아직 옮기지 못한 데이터 조각을 다시 채우는 과정은 반복한다

### **stream (스트림)**

→ 데이터 흐름으로 볼 수 있고 buffer가 다 차면 이를 전송하고 다시 buffer를 채우는 버퍼링 작업을 연속하는것

나온 이유 :100MB 용량의 파일을 읽을 때는 메모리 100MB의 버퍼를 만들어야하고 이 작업을 10개만 동시에 해도 메모리가 1GB에 도달하게 되고 서버처럼 다수가 사용할 수 있는 상황에서 메모리 문제가 발생할 수 있다.

이런 문제 떄문에 스트림이 나와서 한번에 100mb가 아니라 조금씩 지속적으로 전달하는 방식

**구문 오류 (Syntax error)**

코드에 오타가 나거나 중괄호를 빼 먹는 등 실행하려 할 때 자동으로 오류가 등장한다. 코드 자체에 빨간 줄이 뜨고 보통 visual code에서 해결책을 마련해주기도 한다

**런타임 오류**

코드를 실행하려 할 때 멈추면서 나는 오류

에러 메세지가 떠서 에러 메시지를 살피면 해결 가능

→ ex) 예시로 쓰던 코드에 return res.end()를 해주지 않아 밑에 if문에 없는 코드까지 쓰이게 된다면 클라이언트가 발송된 이후에 헤더를 설정 , 오류 발송된 헤더에 문제

**논리 오류**

오류 메시지를 보여주지 않아서 까다로움 원하는 대로 앱 작동이 안되는 오류가 난다.

ex) 파일에 제대로 입력이 되지 않았다던지

고치는 방법→ 디버거 사용

### 요청 응답 코드 이해하기

**요청과 응답 코드 이해하기**

const fs=require('fs');
const http =  require('http');

const server=http.createServer((req, res)=>{
    const url=req.url;
    console.log("지금"+req.url);
    const method = req.method;
    if(url ==='/'){
        res.write('<html>');
        res.write('<head><title>enter message</title><head>');
        res.write('<body><form action="/message" method ="POST"><input type="text" name="message"><button>send</button><body>');
        res.write('</html>');
        // /message에 post 요청을 전달할 것
        return res.end();
    }
    if(url === '/message' && method ==='POST'){
        const body=[];
        req.on('data',(chunk)=>{
            console.log(chunk);
            body.push(chunk);
        });
        req.on('end',()=>{
        const parsedBody = Buffer.concat(body).toString();
        const message = parsedBody.split('=')[1];
        fs.writeFileSync('message.txt',message);
///동기적으로 파일 쓰는 처리하는 작업
        });
        res.statusCode=302;
        res.setHeader('Location','/');
        return res.end();
    }
    res.setHeader('Content-Type','text/html');
    res.write('<html>');
    res.write('<head><title>hi</title><head>');
    res.write('<body><h1>hello from my Node.js Server!</h1><body>');
    res.write('</html>');
    res.end();
});

server.listen(3000);

Java

코드 이해하기

createserver에는 지금 콜백 함수가 들어가 있는 것이다. 즉 요청이 들어올 때마다 매번 콜백 함수가 실행되는 거다.

실제 맨 처음 [localhost:3000/](http://localhost:3000/) 로 들어갔을 때는

이와 같은 명령어가 나오는 걸 알 수 있다. (지금/)

또한 개발자 도구에서 네트워크를 보면

이런식으로 나와있다 url이 /인 곳에서 요청이 왔고 우린 요청에 따라 form태그를 보여준 것이다.

이때 아무 단어를 입력하고 send 버튼을 누르면

터미널에는 이런 식의 명령어가 뜬다. 즉 form태그에서 message url에 post 요청을 한 거고 콜백함수가 다시 실행된다. 이때 요청 url은 /message이고 두번째 if문의 실행을 다시 한다.

setHeader로 인해 다시 url이 /인곳에서 실행을 하여 form태그가 다시 한번 보이는 것이다.

setHeader가 없으면 다시 요청이 온 게 아니라 form 태그가 보이지 않음

실제 네트워크를 보면

localhost의 로드 시점? initiator 호출자?는 3000/message인 것을 알 수 있다.