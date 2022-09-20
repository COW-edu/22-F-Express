## 웹의 작동방식

유저와 클라이언트가 존재

브라우저에 URL을 입력하면

브라우저가 도메인 이름 서버로 접근함. 이때 도메인은 실제 주소를 인코딩한 버전

서버는 각각 IP주소를 가지고 있어 IP주소(도메인URL)을 입력하면 서버에 연결됨

IP주소가 도메인에 속하게 되는 것

## DNS
- 네트워크에서 도메인이나 호스트 이름을 숫자로 된 IP 주소로 해석해주는 TCP/IP 네트워크 서비스
- 이름과 주소 정보를 저장하기 위한 데이터베이스
- Resource Record를 가짐
   - 자원 레코드라는 개념을 사용해 DNS데이터를 저장
- Forward Zone
   - 도메인 이름 -> IP
   - 도메인을 구성하는 호스트에 대한 정보를 record
- Reverse Zone
   - IP -> 도메인 이름
   - DNS 서버 자신에 대한 정보를 record

## 도메인

IP주소를 영어로 변환해놓은 것

## 서버사이드

네트워크의 한 방식인 클라이언트 - 서버 구조의 서버 쪽에서 행해지는 처리

예) HTTP 통신에 있어서 브라우저의 주요 기능 중 하나는 서버에서 HTML 문서를 수신하는 것인데, 브라우저에서 요청한 HTML문서가 서버 사이드 스크립트 언어를 포함하고 있으면 서버 쪽에서 이 부분을 처리하여 결과를 브라우저에 송신하게 된다

## 스크립트 언어

응용 소프트웨어를 제어하는 컴퓨터 프로그래밍 언어

## HTTP

표준화된 서버 소통방식으로 요청과 응답을 처리

## Node로 서버 작동해보기

### require

- require 키워드는 다른 파일로의 경로나 JS파일을 불러올 수 있다
- ./ 을사용하지 않고 사용하면 글로벌 모듈의 기능을 불러오게 된다

```jsx
const http = require('http');

module.exports = http;
```

### import

```jsx
import library from "library";

export default library;
```

- require 과 import모두 다른파일의 코드를 불러옴
- ES6 코드를 변환해주는 도구 없이는 require 키워드를 사용해야 한다
- require / exports는 NodeJs에서 사용되고 있는 CommonJS키워드
- import / export는 ES6(ES2015)에서 새롭게 도입된 키워드로 JAVA와 언어 방식이 유사

### createServer

- 서버를 생성할 때 필요한 메서드
- 첫번 째 인수는 요청에 대한 데이터 두번째 인수는 응답에 사용
- 중괄호 안쓰면 들어오는 모든 요청에 따라 실행하라는 것

```jsx
const http = require('http');

function rqListener(req, res) {

};

http.createServer(rqListener);익명함수
```

### Event Driven Architecture (EDA) : 서버에 요청이 들어올 때마다 함수 실행

```jsx
const http = require('http');

http.createServer(function(req,res) {

});
```

### server 생성

- 상수로 서버를 선언(만들기)해줘야 출력됨
- 포트와 호스트 이름
- 브라우저에서 localhost:3000치면 아무일도 일어나지 않지만 터미널에선 아님
- 화살표함수를 씀으로써 createServer 콜백함수를 만듦

```jsx
const http = require('http');

const server = http.createServer((req,res) => {
	console.log(req);
});

server.listen(3000);
```

### 포트
- 논리적인 접속장소를 뜻함
   - 컴퓨터는 동시에 하나 이상의 프로그램을 실행하기에 IP주소만으로는 특정 서비스에 접근할 수 없다.
   - 포트를 통해 원하는 서비스에 접근할 수 있다
- 특정 서버 프로그램을 지정하는 방법으로 사용 (in TCP/IP)
- 포트번호는 0부터 65535까지 존재한다

- 잘 알려진 포트(0번 ~ 1023번)
   - 특정 용도로 사용하기 위해 IANA에서 정한 포트
   - 강제로 지정되어 있는 것은 아니므로 다른 용도로 사용이 가능하다
   - 루트 권한으로만 포트를 열 수 있다
- 등록된 포트(1024번 ~ 49151번)
   - IANA에게 포트 등록을 요청하여 특정 어플리케이션에 사용하는 포트
   - 서버 소켓으로 사용함
   - 마찬가지로 강제로 지정되어 있지 않음
- 동적 포트(49152번 ~ 65535번)
   - 용도가 지정되어 있지않고 어느 프로그램에서나 사용가능하다

### Event Loop

- 작업이 남아 있는 한 계속 작동 (예 event listen)
- 단일 쓰레드를 사용함으로 eventLoop로 처리
- process.exit로 이벤트 루프를 즉 프로그램을 종료
- 로컬호스트를 3000으로 설정함
- Event Loop와 EDA로 인해 요청이 들어올때마다 멈추는 것이 아닌 언제나 이벤트 사용이 가능하게 함

```jsx
const http = require('http');

const server = http.createServer((req,res) => {
	console.log(req);
	process.exit();
});

server.listen(3000);
```

### 요청에 대한 정보 접근하기

```jsx
const http = require('http');

const server = http.createServer((req,res) => {
	console.log(req.url, req.method, req.headers);
});

server.listen(3000);
```

- 서버에 바뀐건 없지만 GET으로 받아온 것이 다르다 터미널에 보여지는 것이 달라짐

### url

호스트 다음 붙는 모든 주소

/뒤에 오는 값들인데 이 값들에 따라 역할이 달라진다

### 응답하기

```jsx
const http = require('http');

const server = http.createServer((req,res) => {
	console.log(req.url, req.method, req.headers);
	res.setHeader('Content-Type', 'text/html');
	res.write('<html>');
	res.write('<head><title>My First Page</title></head>');
	res.write('<body><h1>Hello from my node.js server</h1></body>');
	res.write('</html>');
	res.end();
});

server.listen(3000);
```

- setHeader로 새로운 헤더를 설정 가능
- Content-Type은 브라우저가 받아드리는 디폴트 헤더, 두 번째 값 또는 인수로
- Content-Type이라는 헤더 키에 대응하는 값을 설정
- text/html에 전송하거나 설정
- 응답에 헤더를 붙이고 응답의 일부가 html이 될거라는 메타데이터를 전송함
- write로 작성가능 (쉬운 html작성법은 추후에)
- end로 마무리

### Get , Post

- get요청은 링크를 클릭하거나 URL을 입력하면 자동으로 전송
- post요청은 해당 양식을 생성함

## Data Stream & Buffer

### Stream

- JS는 연속적인 이진 데이터에 대한 정보 읽거나 조작할 수 없다
- 한 지점에서 다른 한 지점으로 이동하는 데이터들을 의미
    - Node.js에서는 이런 데이터들을 청크단위로 묶어 전송
    - 즉 이동해야할 데이터를 청크단위로 전송하는 과정

### Buffer

- Buffer Class는 8bit - data로 이루어진 stream과 상호작용하기 위한 Node.js의 한 부분이다
    - 즉 Buffer은 Stream을 위해 존재하는 JS class이다

### 동기

- 호출하는 함수가 호출되는 함수의 작업이 완료 후 리턴을 기다린다
- 미완료 상태라면 리턴받더라도 완료 여부를 계속 확인한다

### 비동기

- 호출하는 함수A가 호출되는 함수B에게 콜백 함수를 함께 전달해서, 함수 B의 작업이 완료되면 함께 보낸 콜백 함수를 실행
- 호출 이후 작업완료 여부는 신경쓰지 않는다

## 블로킹 및 논블로킹

- 처리되야 하는 작업(전체 작업)의 흐름을 막냐 안막냐에 대한 관점
    - 제어권이 누구한테 있냐가 중요

### Blocking

- 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 끝날때까지 기다렸다가 자신의 작업을 함
- 호출하는 함수A가 호출되는 함수 B를 호출하면 제어권을 A가 B에게 넘겨준다/

### Non-Blocking

- 다른 주체의 작업에 관련없이 자신의 작업을 함
