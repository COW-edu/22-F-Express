# Section 3

require 키워드 - 다른 파일로의 경로나 JS파일을 불러올 수 있음

이벤트 루프 - 작업이 남아 있는 한 계속 작동하는 루프 프로세스(이벤트 리스너가 있는 한 계속 작동함)

Domain name은 Server의 실제 주소와 다르다. Domain name은 사람이 읽기 쉽게 인코딩한 버전이라고 생각하면 된다.

Web이 작동할 때, 클라이언트는 request를 보내고 서버는 response를 보낸다. 이때 Respose에는 HTML 형식을 가질 수 있고 file이나 json 등 다른 종류의 데이터도 가능하다.

우리는 Node.js를 통해 서버에 사용될 코드를 입력할 수 있다.

HTTP에 사용되는 메서드 중 GET 메소드는 서버에게 리소스를 달라는 요청 데이터를 받기만 하는 메소드이고, POST 메소드는 서버에 입력 데이터를 전송하며 요청 엔티티 본문에 데이터를 넣어 서버에 전송하는 메서드이다.

  

```
const fs = require('fs');

const requestHandler = (req, res) => {
  const url = req.url;
  const method = req.method;

  if (url === '/') {
    res.write('<html>');
    res.write('<head><title>Enter Message</title><head>');
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
    );
    res.write('</html>');
    return res.end();
  }
  if (url === '/message' && method === 'POST') {
    const body = [];
    req.on('data', chunk => {
      console.log(chunk);
      body.push(chunk);
    });
    return req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split('=')[1];
      fs.writeFile('message.txt', message, err => {
        res.statusCode = 302;
        res.setHeader('Location', '/');
        return res.end();
      });
    });
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title><head>');
  res.write('<body><h1>Hello from my Node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
}

module.exports = requestHandler; 
// exports.handler = requestHandler; 로도 사용 가능
```

on 메소드는 특정 이벤트를 listen할 수 있게 한다.

body처럼 본문은 빈 배열이어야 한다. 

node.js에서 함수를 함수 안에 넣으면 안에 있는 함수를 나중에 실행한다.

코드가 실행되는 방식에는 블로킹 방식과 논블로킹 방식이 있다. 블로킹 방식이란 코드의 실행이 다른 코드의 실행을 막는 것이고, 논블로킹 방식은 코드의 실행이 다른 코드의 실행을 막지 않는 것이다. node.js에서 코드는 논블로킹 방식으로 실행되어서 수많은 콜백 함수와 이벤트를 등록해두면 특정 작업이 끝난 후 node.js가 코드를 실행시킨다. 그러므로 js 스레드는 항상 새로운 이벤트나 요청을 다룰 수 있다.