# week-01

# week - 01(~Section 04)

1. 웹 작동 방식
    1. 브라우저에 웹 주소를 입력한다.
    2. 브라우저는 DNS 서버로 가서 웹 사이트가 있는 서버의 진짜 주소를 찾는다.
    3. 그 다음 브라우저는 서버에게 웹 사이트 사본을 클라이언트에게 보내 달라는 HTTP 요청 메세지를 서버에 전송한다. (여기서 메시지와 클라이언트와 서버의 모든 데이터는 TCP/IP 연결을 통해 전송)
    4. 이 메세지를 받은 서버는 클라이언트의 요청을 승인하고, “200 OK” 메세지를 클라이언트에 전송한다. 
    5. 그 다음 서버는 웹 사이트의 파일들을 데이터 패킷이라 불리는 작은 일련의 덩어리들로 브라우저에 전송하기 시작한다.
    6. 브라우저는 이 작은 덩어리들을 완전한 웹사이트로 조립하고, 사용자에게 보여준다. 
    
2. Core Module
    - 의미 : Node라는 실행파일 안에 이미 포함되어 있는 모듈, node 개발자들이 일반적으로 필요한 기능들을 모아 node 안에 넣어둔 모듈들을 칭한다.
    - 더 많은 모듈들을 찾아보고 싶다면, [https://nodejs.org/dist/lastest-v12.x/docs/api/](https://nodejs.org/dist/lastest-v12.x/docs/api/) 여기에 접속하자.

1. 서버 만들기
    
    ```jsx
    const http = require('http'); //서버를 만드는 모듈 불러옴.
    const server = http.createServer((res, req) => { //서버 만드는 메소드
        console.log('server start!');
    });
    server.listen(3000); //localhost: 3000 서버 생성
    ```
    
2. Node.js Program Lifecycle
    1. 스크립트가 시작되어 Node.js가 파일 전체를 살펴보고 
    2. 코드를 살펴보고 변수와 함수를 등록한다.
    3. 이벤트 리스너가 존재하는 한 계속 돌아가는 이벤트 루프가 돌아간다.(사실상 이벤트 루프가 모든 코드를 관리한다.)
    
3. Stream & Buffer
    - Stream
        1. 입출력 데이터를 입출력 순서에 의해서 순차적으로 처리되는 데이터
        2. 데이터를 이동 시킬 수 있는 다리
        3. 전송되어야 할 크기만큼 바이트들이 모여 만들어진 통로
        4. 통신을 목적으로 한 바이트 단위의 집합.
        
    - Buffer
        
        의미: 입력 속도에 비해 출력 속도가 느린 경우 데이터를 임시 저장하는 공간을 말한다.
        
        데이터를 입력하면 버퍼라는 임시 공간에 담은 뒤  버퍼에 꽉 차거나 개행 문자가 입력되면 버퍼에 저장된 데이터를 한 번에 전송한다. 
        
4. Single Thread, Event Loop & Blocking Code
    - js는 Single Thread를 사용한다.
    - 하지만 js는 마치 여러 개의 작업을 동시에 작업하는 것처럼 보인다. 그 이유는 Evenr Loop 때문이다.
    
5. NPM
    - Node Package Manager의 줄임말로써 패키지를 관리할 수 있는 도구이다.
    - 단축기를 설정 할 수 있다.
    
6. 3rd Party Packages
    - 때에 따라 3rd Party Pakages를 pull할 필요가 있기 때문이다.
    - NPM을 통해 다운로드를 할 수 있다.
    - —save와 —save-dev는 프로덕션과 개발 의존성을 구분하게 하며 -g에 해당하는 전역 의존성은 발견되지 않은 명령어 오류가 발생하는 일 없이 Terminal 내부에서 실행할 수 있다.
    
7. Types of Errors
    - Syntax and runtime error
        - 최소한 도움이 되는 오류 메시지를 출력해준다.
    - Logical error
        - 다른 오류에 비해 고치기 더욱 어려운 경우가 많다.