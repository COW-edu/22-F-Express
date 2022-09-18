# TEAM 민수 - 1주차 RBF

## 멘토 : 민수
## 팅원 : 태강, 영환, 해준, 은택



# Node.js란?

- Node.js는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.

- Node.js는 C++로 쓰인 V8 코드베이스를 사용하며 로컬 파일 시스템 활용, 파일 열기, 파일 읽기, 파일 삭제 등의 기능을 추가해 줍니다.

## REPL

- **REPL**은 Read-Eval-Print-Loop의 약자로, Read는 사용자의 입력을 받아들이는 것, Eval은 입력받은 코드를 평가하는 것, Print는 평가된 결과를 출력하는 것, Loop는 평가된 결과를 출력한 후 다시 사용자의 입력을 받아들이는 것을 반복하는 것을 의미합니다.


# 웹 작동 방식

웹의 작동방식으로는 유저와 클라이언트가 존재한다.

- 우리가 흔히 알고있는 주소=>url은 서버의 실제주소에 해당하지 않는다.
- url을 입력하면 브라우저가 도메인 이름 서버로 접근하여 해당 도메인을 찾는다.
- url => 사람이 주소를 읽을 수 있도록 인코딩한 버전이다.
- 브라우저는 주어진 IP 주소를 사용해 서버에 요청을 보낸다.

- 해당 IP 주소가 있는 컴퓨터의 인터넷에서 구동되는 서버 프로그램이 요청을 받아들이고 처리한다.
- 서버에서 처리된 결과를 브라우저에 응답한다.

# 서버 생성

서버를 생성하기 위해선 기능을 불러와야한다.
브라우저나 node.js의 js파일에는 아무것도 불러오지 않고도 전역으로
사용 가능한 기능, 객체들이 많다.
그러나 대부분의 기능은 기본으로 사용 가능하지 않기도 하고
저장된 키워드, 이름등을 어지럽히지 않고 기능을 확실히 나타내기위해
기능을 불러온다.

## 코어모듈

-http: 서버를 출시 혹은 요청을 보내는것과 같은 작업에 도움을 준다.
node.js앱이 다른 서버로 요청을 보내 여러 서번간에 소통을 할 수 있도록 한다.
예로) google maps api에서 좌표를 지급 요청후 주소를 받아낼 수 있음.

-https: 모든 전송 데이터가 앞호화되는 SSL암호화 서버를 출시할때 도움이 된다.

위 두 모듈 http, https는 서버를 생성 혹은 http요청 및 응답 작업에 
매우 유용하다.

-fs: 파일 생성 모듈
```jsx
const fs = require('fs');

fs.writeFileSync('hello.txt', 'hihihihihihihihihi');
```
이런 방법으로 node를 통해 파일을 실행하면 'hihihihihihihihihi'
문자열이 출력되어있는 'hello.txt' 파일이 생성됨.

-os: 운영체제 관련 도움을 준다.

- 더 많은 모듈들을 찾아보고 싶다면, [https://nodejs.org/dist/lastest-v12.x/docs/api/](https://nodejs.org/dist/lastest-v12.x/docs/api/) 여기에 접속하자.

먼저 http모듈만 사용해보자

http 모듈을 사용하려면 => http모듈 호출후 node.js에 탑재된 http모듈의 기능을
사용할 수 있는지 확인 해야한다.
기본적으로 전역에서 사용 불가능하기에 변경되지 않는 const를 사용하여 불러온다.
= 으로 값을 저장해야 하는데 특별한 키워드 require이 있다.

## require
node.js는 전역으로 노출하는 특성이 있어서 node.js로 실행하는 모든 파일에서
기본으로 require이라는 키워드 사용가능하다.
파일을 불어오는 방법인 require이라는 키워드는 다른 파일의 경로나 
js파일을 불러올 수 있고 파일에 대한 경로를 모른다 하더라도 http같은 코어 모듈을
불어올수도 있다.

다음과 같이 사용

```jsx
const http = require('http');
```

## 파일경로

파일경로는 상대경로인 ./ 이나 절대경로인 / 로 시작한다.
(.js는 자동으로 붙으니 생략해도됨.)
만약 모든 경로를 생략하고 'http' 단독으로 사용한다면.
http라는 글로벌 모듈을 찾게된다. => 이는 node.js에 탑재된 모듈중 하나임.

위 코드처럼 작성 하였기에 이제 글로벌 모듈의 기능을 사용할 수 있다.

코드창에 http. 을 입력하면 우리가 불러온 모듈의 기능(js의 객체의 메서드, 속성)
에 접근할 수 있다.

그중 하나 createServer() 메서드에 대하여 알아보자

## createServer();
이 메서드는 서버를 생성할때 꼭 필요한 메서드이다.
이 메서드 위에 마우스 커서를 올려보면 requestListener를 인수로 가진다 라고 나온다.
(requestListener=> 들어오는 모든 요청을 실행하는 기능.)

이것을 함수로 만들어 기능을 사용해보자.
```jsx
const http = require('http');

function rqListener(req, res){

}rqListener
//첫번째 인수에는 요청에대한 데이터 두번째 인수는 응답에 사용

const server = http.createServer(rqListener);
```
이렇게 다시 마우스 커서를 올려보면 "두개의 인수가 존재해야한다. "라고
알리는데
requestListener는 들어오는 메세지 혹은 응답객체유형의 요청을 받는다.

정리하면 node.js가 자동으로 들어오는 요청을 대변하는 객체를 제공하고 
해당요청으로 부터 데이터를 읽을 수 있게끔 하며 요청을 보낸 사람에게 응답을 보낼수있는 응답객체를 주는 것이다.

이제 여기에 입력할 두가지 인수에 첫번째는 요청에 대한 데이터 
두번째는 응답에 대한 데이터에 사용한다.(req, res로 이름붙이자!) + (rqListener를 참조하자)

위와 같은 방법으로 사용해도 상관없고 익명함수로도 사용가능하다.

## 익명함수로 사용하기

결과는 동일하다.

이번에도 createServer에 이 함수를 입력하면 

서버에 요청이 들어올때마다 노드가 익명함수를 실행한다.


**이게 바로 node.js의 주된 이벤트 드리븐 아키텍쳐(EDA)이다.**

```jsx
const http = require('./http.js');

const server = http.createServer(function(req, res){
});
```

앞으로는 이 같은 설정이나 코드 스니펫을 자주 사용할것이다.

코드 스니펫은 노드에게 x가일어나면 y를 실행하라 즉 이경우는 요청이 들어오면
이 함수를 실행하라 라고 알려주는것이다.

## 콜백함수로 사용하기

추가로 새로운 방식의 js구문과 화살표 함수를 통하여 콜백함수로도 사용이 가능하다.
(콜백함수로도 가능)

```jsx
const http = require('./http.js');

const server = http.createServer((req, res) => {
    console.log(req);
});
```

위와 같이 서버를 저장하였으면 server.으로 접근하여 다양한 메서드를 호출할 수 있는데
그 중 하나인 listen에 대하여 알아보자

## listen

listen은 node.js가 계속 실행되며 듣도록한다.
listen 에는 인수로 포트를 입력할 수 있다.

```jsx
const http = require('http');

/*function rqListener(req, res){

}rqListener
//첫번째 인수에는 요청에대한 데이터 두번째 인수는 응답에 사용*/

const server = http.createServer((req, res) => {
    console.log(req);
});

server.listen(3000); //3000은 포트번호
```


## Node의 라이프사이클

- node app.js 파일 실행
- 스크립트 실행
- 코드를 분석한 후 변수와 함수를 등록
- .. 이벤트 루프

### **이벤트 루프로 인해 프로그램이 종료되지 않는다.**


# 이벤트 루프

node.js가 관리하는 이벤트 루프는 작업이 남아았는한 계속해서 작동하는 루프 프로세스로 이벤트 리스너가 있는 한 계속 작동한다.

등록 후 제거하지 않았던 이벤트 리스너로 createServer로 만든 들어오는 요청 리스너가 있다. 

(((req, res) => {
    console.log(req);
});)





# node.js 프로세스 제어

ctrl+C를 누르면 실행중인 node.js서버가 종료되니 알아두자




# 요청의 이해



## 필드 URL

```jsx
const http = require('http');

const server = http.createServer((req, res) => {
    console.log(req.url, req.method, req.headers);
});

server.listen(3000); //3000은 포트번호
```

위와 같이 url다음에 쉼표로 나누어 하나 이상의 필드의 결과를 보자

req.method, req.headers를 입력한다. 

세가지 값을 출력하고 node app.js로 서버를 재시작하면  터미널의 출력값이 조금 바뀌었는데
req.headers를 출력했기 때문에 헤더는 똑같이 나오지만 그 이전에 req.method 값이 CET으로 나오며
url으로는  / 으로만 나왔다 

왜냐하면 url은 호스트 다음에 붙는 모든 주소인데 이 경우는 localhost외에 아무것도 없기 때문에
localhost뒤에 /만 나온것이다. 만약 주소에 /test를 입력하면 또다른 출력값이 나온다 ⇒ /test가 로그에 남는다.

# 응답전송

## setHeader

setheader를 통하여 헤더를 새롭게 설정가능하다.
```jsx
const http = require('http');

const server = http.createServer((req, res) => {
    console.log(req.url, req.method, req.headers);
    
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title> h1h1h1h1h1h </title></head>');
    res.write('<body><h1> fsdaf </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //3000은 포트번호
```

위 작성한 코드처럼
res를 설정하고 여기에 write를 설정해주자  ⇒ res.write

write는 res에 데이터를 기록할 수 있으며 기본적으로 대단위 내지는 다수의 라인을 통해 작동한다.

res를 여러 라인에 걸쳐 작성할 수 있다.

res.end()를 호출하고 response body에 데이터를 모두 입력한 뒤에는 end를 호출하고
이 시점 부터는 더이상 아무것도 입력해서는 안됨 만약  
res.write를 계속 호출할 경우 호출은 되나 오류가 발생함 ⇒ 응답을 끝낸뒤에는 변경이 안됨

# 라우터 요청

## 요청과 응답을 연결
```jsx
const http = require('http');

const server = http.createServer((req, res) => {
    const url = req.url;
    if(url === '/'){
        res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>Enter Message </title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></from></body>');
    res.write('</html>');
    return res.end();
    }
    
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title> My first server </title></head>');
    res.write('<body><h1> fsdaf </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //3000은 포트번호
```
## url의 파싱

url을 새로운 상수에 저장 => url의 액세스 요청으로 진행하게 된다.
(const url = req.url;)
후에 if문을 두어 url이 / 와 동일한지 확인( if(url === '/') )

## 요청 리디렉션

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    const url = req.url;
		const method = req.method;
    if(url === '/'){
        res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>Enter Message </title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></from></body>');
    res.write('</html>');
    return res.end();
    }
    
		if(url === '/message' &&  method === 'POST'){
				fs.writeFileSync('message.txt', 'DUMMY')
				res.statusCode = 302;
				res.setHeader('Location', '/');
				return res.end();
		}
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title> My first server </title></head>');
    res.write('<body><h1> fsdaf </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //3000은 포트번호
```
if(url === '/message') 여기에 if문을 추가하여 다음을 확인하고
추가할 또 다른 조건은 여기에서 GET요청 말고 POST요청을 다루도록 하는 것이다.
request method에서 메서드를 파싱하고 method === POST가 되도록 입력하자(4번째 줄)

const fs = require('fs') 를 통해 fs를 통해서 파일시스템을 작업할수있으며 아까만든 if문에 새로운 파일을 작성하려 한다.

따라서 writeFile을 실행해 줄텐데 writeFile은 파일로 가는 경로를 취하게 되며, 우리는 app.js와 동일한 폴더에 생성하고
이름은 간단하게 message.txt로 지정하자 이 안에는 물론 유저가 보낸 내용을 저장하게 될것이다.

위 코드를 재실행해보면 입력 필드가 있는 페이지를 다시 로드하여 미지의 값을
전송하면 개발자탭 네트워크의 탭에서 경오 재지정을 보면 메세지 요청을 전송하였고
localhost로 리디렉트 됬음을 알 수 있다.

# 요청 분문 분석

## 데이터 스트림

데이터 스트림을 알기전 연관된 버퍼라는 개념을 알아보자면
스트림은 지속적인 프로세스이다. 노드가 많은양의 요청을 한 청크씩 읽고 어느시점에 다 읽게 되었다면
바로 이때부터 요청 전체를 읽기까지 기다리지 않고도 각각의 청크를 다룰 수 있게 된다. 
우리가 다루고 있는 간단한 요청 같은 경우에는 이 과정이 필요하지않다.
입력 필드 데이터가 단 하나이므로 분석하는데 그리 오래 걸리지 않는다.
하지만 업로드된 파일의 경우 상당히 오래걸린다.
이럴때에는 데이터를 스트리밍 하면 디스크에 쓸수있다.

데이터가 들어오는 와중에 
앱이 실행되는 하드 드라이브나 노드 앱이 실행되는 서버에 쓸수있기 때문에
파일 전체가 분석 완료되고 전부 업로드되기까지 아무것도 안하면서 기다릴 필요가 없다.
node.js는 요청이 얼마나 크고 복잡한지 미리 알지 못하기 때문에 전부 이런 방식으로 처리하는 것이다.
하지만 데이터를 미리 다룰수있다.
버퍼는 비유하자면 버스 정류장과 같다.

*요약*
    - Stream
        1. 입출력 데이터를 입출력 순서에 의해서 순차적으로 처리되는 데이터
        2. 데이터를 이동 시킬 수 있는 다리
        3. 전송되어야 할 크기만큼 바이트들이 모여 만들어진 통로
        4. 통신을 목적으로 한 바이트 단위의 집합.

    - Buffer

        의미: 입력 속도에 비해 출력 속도가 느린 경우 데이터를 임시 저장하는 공간을 말한다.

        데이터를 입력하면 버퍼라는 임시 공간에 담은 뒤  버퍼에 꽉 차거나 개행 문자가 입력되면 버퍼에 저장된 데이터를 한 번에 전송한다.

*실습*

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    const url = req.url;
		const method = req.method;
    if(url === '/'){
        res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>Enter Message </title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></from></body>');
    res.write('</html>');
    return res.end();
    }
    
		if(url === '/message' &&  method === 'POST'){
				const body = [];
				req.on('data', (chunk) => {
					console.log(chunk);
		      body.push(chunk);
        }
        );
				fs.writeFileSync('message.txt', 'DUMMY')
				res.statusCode = 302;
				res.setHeader('Location', '/');
				return res.end();
		}
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title> My first server </title></head>');
    res.write('<body><h1> fsdaf </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //3000은 포트번호
```

다음과 같은 코드를 보자

POST메세지를 받고 응답을 보낼때 팡일에 쓰기전에
요청 데이터를 받아함 => req.을 입력하여 이벤트 리스너를 등록하면 된다.

createServer가 대신 서버를 생성해주는 반면 on메서드를 사용하면 직접 할수있다.
on메서드는 특정 이벤트를 들을수있도록 한다. 이경우 듣고 싶은 이벤트가 data이벤트라고 하자 
여기에 data를 입력하면

```jsx
const body = [];
				req.on('data', (chunk) => {
					console.log(chunk);
		      body.push(chunk);
        }
```

새로운 청크가 읽 준비가 될 때마다 데이터 이벤트가 발생한다.
이때 버퍼가 도움을 주는데 두번째 인자로 오든 데이터 이벤트에 실행될 함수를
넣어야 한다.
새로운 const를 만들어 요청본문을 읽을것이다.
```jsx
const body = [];
```
(빈 배열이여야함.)

이벤트 리스너를 등록
END리스너는
들어오는 요청 데이터 혹은 들어오는 전반적인 요청을 분석한 후에 발생한다.
```jsx
req.on('end', (chunk) => {
    const parsedBody = Buffer.concat(body).toString();
		console.log(parsedBody);
});
```
두번째 인수로 정의하는 함수를 실행 => 읽어들인 모든 청크에 기반한다. => 모두
본문에 저장됨. 
parsedBody라는 새로운 상수를 생성하고 전역에서 사용가능한 buffer 객체를 사용해 concat(body)를 입력한다.
그러면 새로운 버퍼가 생성되고 본문안에 있던 모든 청크가 추가될것이다.
이제 parsedBody라는 버퍼에 toString을 호출해 문자열로 전환한다.
이렇게 parsedBody를 완성하면 처리할수 있는 상태가 된다.

그럼 parsedBody를 출력해보자

서버를 재시작하고 무작위 메세지를 보내면 

이렇게 두개의 콘솔이 나타난다.

![zsss.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b40cbff-5049-49f9-bdee-d60dc823d9d7/zsss.png)

하나는 콘솔로그의 출력값으로 우리가 처리할수없는 청크이다.

반면 parsedBody는 아까 입력한 메세지를 받아서 우리가 처리할수있는 이런 메세지를 산출한다.

이름을 message로 설정해서 message=으로 시작하는 것이다.

*지금까지 전체코드
```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    const url = req.url;
		const method = req.method;
    if(url === '/'){
        res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>Enter Message </title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></from></body>');
    res.write('</html>');
    return res.end();
    }
    
		if(url === '/message' &&  method === 'POST'){
		const body = [];
		req.on('data', (chunk) => {
			console.log(chunk);
		    body.push(chunk);
        });
        req.on('end', (chunk) => {
            const parsedBody = Buffer.concat(body).toString();
            console.log(parsedBody);
        });
		fs.writeFileSync('message.txt', 'DUMMY')
		res.statusCode = 302;
		res.setHeader('Location', '/');
		return res.end();
		}
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title> My first server </title></head>');
    res.write('<body><h1> fsdaf </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); //3000은 포트번호
```


# Single Thread, Event Loop & Blocking Code
- js는 Single Thread를 사용한다.
- 하지만 js는 마치 여러 개의 작업을 동시에 작업하는 것처럼 보인다. 그 이유는 Evenr Loop 때문이다.

# NPM
- Node Package Manager의 줄임말로써 패키지를 관리할 수 있는 도구이다.
- 단축기를 설정 할 수 있다.

# 3rd Party Packages
- 때에 따라 3rd Party Pakages를 pull할 필요가 있기 때문이다.
- NPM을 통해 다운로드를 할 수 있다.
- —save와 —save-dev는 프로덕션과 개발 의존성을 구분하게 하며 -g에 해당하는 전역 의존성은 발견되지 않은 명령어 오류가 발생하는 일 없이 Terminal 내부에서 실행할 수 있다.

# Types of Errors
- Syntax and runtime error
- 최소한 도움이 되는 오류 메시지를 출력해준다.
- Logical error
- 다른 오류에 비해 고치기 더욱 어려운 경우가 많다.