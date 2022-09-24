# section 5 (1)

## What is Express.js?

- 왜 express를 사용하는가 ? 라우팅을 깔끔하게 관리하기 위해서이다.
    - 강의에서 이야기 하는 가장 큰 강점은 특별한 기능들을 과도하게 추가하지 않아도 된다는 것과 유연하다는 것, 높은 확장성이다.
    - express 없이 라우터를 만들면 요청과 주소별로 따로 처리해야하기 때문에 복잡해진다.
    - passing을 대신 진행 해주기 위해 프로젝트 내부에 쉽게 연결이 가능한 다른 패키지를 쉽게 설치하도록 도와준다.
- 무거운 작업들은 프레임워크를 통해 진행된다. 따라서 우리는 애플리케이션과 코드를 어떤 구조로 구축해야할지, 어떻게 이 프레임워크를 활용해서 깔끔한 코드를 작성할지 고민해야하는데, express는 우리가 하는 이 고민에 대해서도 도움을 준다.
    - 제 3자 패키지로 설치할 수 있는 프레임워크. 일부 필수적인 작업이라던가 신경 쓰고 싶지 않은 세부 내용을 외부에 맡길 수 있도록 도와준다.
    - 깔끔한 코드를 작성하고 핵심적인 일에 집중하게 도와주는 유틸리티 함수를 다수 제공한다.
- 설치
    - npm install —save express : 프로덕션 의존성
    
    ```jsx
    const http = require('http');   // node전용 코어 모듈 
    
    const express = require('express'); //패키지
    
    const app = express();  // express 앱 생성. 요청 핸들러 역할을 한다. 
    
    const server = http.createServer(app);
    
    server.listen(3000);
    ```
    
    - express 어플리케이션 생성을 하게 되면, 새로운 객체를 초기 설정하게 된다. 초기 설정을 할 때 express 또는 프레임 워크가 많은 내용을 저장 및 관리하게 된다.
- Express.js 백그라운드 확인  (다시)
    
    ```jsx
    const http = require('http');
    const server = http.createServer(app);
    server.listen(3000);
    //아래 코드는 위의 내용을 한번에 해결가능한 코드이다
    app.listen(3000); //서버생성 + 포트연결
    ```
    

## using Middleware

- 위의 코드를 보면 요청 핸들러 역할은 존재하지만, 요청을 처리하는 역할을 보이지 않는다. 요청을 처리하는 특정방식을 설정하는데 이 때 middleware가 사용된다.
- 위의 코드처럼 요청핸들러는 하나밖에 없지만, 응답을 전송하기 전까지 응답은 요청이 통과하게 될 다양한 함수들을 연결할 가능성을 확보한다.  하나의 함수만을 사용하는 것이 아닌 코드를 다수의 블록 or 조각들로 분할할 수 있다는 것이 express의 특징이다.
- use 사용 : 새로운 미들웨어 함수를 추가할 수 있다. 요청 핸들러 배열을 이 공간에 받아들인다.
    - req, res, next 총 3개의 인자를 받는다. (사용하지 않는 인자는 삭제해도 무방하다.)
        - next는 express.js를 통해 use로 전달되는 함수이다. next는 인수에 존재하는 또 다른 함수를 수신한다. 수신 내용은 다음 미들웨어로 요청이 이동할 수 있도록 실행되어야 한다.
        
        ```jsx
        ...
        const app = express(); // 요청핸들러
        app.use((req,res,next) => {});  // 응답. 이 경우 모든 요청을 받아 실행된다. 
        const server = http.creatServer(app);
        ...
        //아직 어디로 응답을 보낼지 설정하지 않은 상태이다. 
        ```
        
        - 새로운 미들웨어 함수를 추가할 때(다음 라인에 존재하는 미들웨어로 요청을 이동시키려 할 때는 ) next 함수를 호출해야한다. (+ 응답을 보내지 않을 때 호출. 그렇지 않으면 요청이 실패하고 다음 미들웨어로 이어지지 않는다. )
        
        ```jsx
        const app = express();
        app.use((req,res,next) => {
        	console.log('In the middleware');
        	next(); //next 호출. 아래에 존재하는 middleware로 이동.
        });  
        app.use((req,res,next) => {
        	console.log('In the middleware');
        });  
        const server = http.creatServer(app);
        ```
        
- app.use([우리가 찾고자 하는 대상 (path)], 실행해야하는 함수 (callback) )
    - path을 통해 특정 요청만을 받을 수 있다. (ex. localhost**:/** , localhost:**/message**)
    - callback은 하나 이상 가능하며, 다중 경로 필터를 설정할 수도 있다.
    
    ```jsx
    app.use('/add-product',(req, res, next) => {
        console.log('In another middleware!');
        res.send('<h1>hello from express</h1>');
    });
    app.use('/',(req, res, next) => {
        console.log('In another middleware!');
        res.send('<h1>hello from express</h1>');
    }); // 첫번째 미들웨어에서 next를 호출하지 않았으므로 이 미들웨어는 요청을 처리할 기회가 없다.
    ```
    

> 미들웨어 순서가 / → /add-product가 아닌, /add-product → / 인 이유는 요청이 코드의 첫번째 줄에서 차례대로 진행되고 , next를 호출하지 않으면 다음 미들웨어로 넘어가지 않기 때문이다.
> 
- send : 응답을 보낼 수 있고 어느 유형의 본문이든 첨부할 수 있다.
    
    ```jsx
    app.use((req, res, next) => {
    	console.log('In another middleware!');
    	res.send('<h1> Hello from express</h1>'); //응답을 보내는 기능을 한다. 
    });
    ```
    
    - 브라우저를 열어서 개발자 도구-netwotk에 들어가면 Header 파일의 Content-Type을 보면 html로 들어가 있는 것을 볼 수 있는데., 이것이 강의 초반에 설명한 express.js가 자동적으로 제공하는 기능이다. ( + send 메소드가 기본적으로 html 콘텐츠 유형을 선택해준다. )
    - write 보다 사용이 쉽고 , 실제 파일을 전송하는데 효율적이다.

## Working with Requests & Responses

- res.redirect : ( + 수동으로 상태코드를 설정하고 위치 헤더들 설정하는것보다 간편하다.)
    
    ```jsx
    app.use('/product', (req,res,next)=> {
    	console.log(req.body);//사용자가 보낸 내용을 추출하는 역할을 한다. 
      res.redirect('/');  //자동으로 /경로로 리다이렉트가 된다. 
    });
    ```
    
    - 위의 코드를 실행하면 터미널에는 undefined을 나타내게 된다. ‘req.’ 는 들어오는 요청의 본문을 분석하려 하지 않기 때문이다.  이를 위해 또 다른 미들웨어를 추가해서 분석한다.
- body -parser :본문 분석 미들웨어 패키지
    - 결과적으로 키-값 쌍의 자바스크립트 객체를 얻게된다. ( + 첫째주의 split 함수를 사용해서 수동으로 배열을 만들던 것을 패키지 하나로 자동적으로 생성. )
    - 경로 처리를 미들웨어 이전에 둬야한다. 요청이 어디로 향하든 본문 분석이 이루어지도록 하기 위해서이다.
        
        ```jsx
        app.use(bodyParser.urlencoded); // 미들웨어를 등록해주는 코드이다. 
        app.use('/product', **(req,res,next)=> {
        	console.log(req.body);
          res.redirect('/');** 
        });  //마지막에는 req,res,next를 인자로 하는 미들웨어 함수를 내놓는다.
        ```
        
        ```jsx
        app.use(bodyParser.urlencoded({extended : false})); 
        // extended는 비표준 대상의 분석이 가능한지를 나타낸다. 
        ```
        
- app.use는 POST 요청 뿐만 아니라 GET요청에 대해서도 실행이 된다. POST 요청만을 받고 싶다면 ? **get, post  사용!**
    - app.get을 사용할 경우 GET 요청을 걸러낸다.
    - app.post를 사용할 경우 들어오는 post 요청의 경우만 실행되고 get 요청에는 반응하지 않는다.

## Routing

- 라우팅이란, 클라이언트의 요청 경로를 보고 이 요청을 처리할 수 있는 곳으로 기능을 전달해주는 역할을 하는 것을 의미한다.
- 파일의 크기가 커질 수록 라우팅 코드를 여러 파일로 나누는 것이 좋다.(??) (app.use, [app.post](http://app.post) 등)
- express는 라우팅을 다른 파일에 위탁하는 편리한 방법을 제공한다. 폴더를 하나 만들고 , 다양한 경로와 http method에 대해 실행할 라우팅 관련 코드를 파일에 담아서 이 폴더 안에 저장한다.
    
    ```jsx
    const express = require('express'); // express 임포트
    const router = express.Router(); // 라우터 생성, 실행하게 될 함수이다. 
    router.get( ... );  //라우터를 사용해 대상 등록. 
    module.exports = router; //라우터 전송
    ```
    
    - express.Router는 다른 express 앱에 연결되어 있거나 꽂아 넣을 수 있는 express 앱과 같으며 module.exports를 통해 내보낼 수도 있다.
    
    ```jsx
    const express = require('express');
    const bodyParser = require('body-parser');
    
    const app = express();
    
    const adminRouters = require('./routes/admin');
    const shopRouters = require('./routes/shop');
    //express와 분리된 import추가. 자기 자신의 파일임을 확실히 하기 위해서이다.
    
    app.use(bodyParser.urlencoded({extended : false}));
    
    app.use(adminRouters); 
    app.use(shopRouters);
    
    app.use('/',(req, res, next) => {
        res.send('<h1>hello from express</h1>');
    });
    
    //app.use(adminRouters); 코드가 현재 위치에 들어가면 안된다 .
    //next를 호출하지 않았으므로app.use(adminRouters); 는 미들웨어 요청을 처리할 기회가 없다.
    app.listen(3000);
    ```
    
    - app.use를 호출해서 adminRouters를 넘겨줄 수 있다. 위의 코드와 같이 함수처럼 호출하지 않고 사용이 가능한데 유효한 미들웨어 함수라는 점에서 강점을 지닌다. ??
    - 주의해야할 점은 , get과 post 등 http 메소드 등은 경로와 정확히 일치하는 요청만을 처리하기 때문에 app.use(adminRouters); 와 app.use(shopRouters);의 코드의 위치를 바꾸게 된다면 정상적으로 실행이 되지 않는다.
        - 그렇기 때문에 use를 사용할 때는 순서를 신경쓰는 것이 좋다.
        - use는 모든 들어오는 http 메소드를 처리하지만, get은 정확히 일치하는 요청만을 처리하기 때문에 무작위 경로를 입력하면 오류가 발생한다.
        - 왜? 무작위 경로를 처리할 단일 미들웨어가 존재하지 않기 때문이다.
- 404 error 오류 페이지를 전송하려면 코드 맨 아래 모든 요청을 처리하는 미들웨어를 추가하면 된다.
    
    ```jsx
    .. 
    app.use((req,res,next) => {
        res.status(404).send('<h1>Page not found</h1>');
    }); // status를 호출해서 404상태 코드를 설정한다. 
    ```
    
- 경로필터링 : 아웃소싱 라우터를 사용해서 진행한다.
    - 경로가 같은 구간에서 시작하는 경우는 해당 구간을 빼서 필터로 지정할 수 있다.
    
    ```jsx
    app.use('/admin',adminRouters); 
    // admin 경로에 해당하는 라우트만 adminRouters에 들어가게 지정이 가능하다.  
    ```
    

## Retunning HTML

- sendFile : sendFile을 사용함으로써 사용자에게 파일을 회신할 수 있으며 콘텐츠 유형을 자동으로 응답헤더로 설정한다. (section 3에서 content-type을 hrml로 돌려줬던 과정보다 자동화된것을 알 수  있다. )
    - 파일의 경로는 node.js가 제공하는 코어 모듈 ‘path’를 활용한다.
- path.join() : 마지막에 경로를 출력해준다. 여러 부분을 이어 붙여서 경로를 구축한다.
    - __dirname: 운영체제의 절대경로를 프로젝트 폴더로 고정시켜주는 역할을 한다. ( node에서 제공하는 전역변수이다. )
    - 사용 이유는 ? 자동으로 윈도우 시스템과 리눅스 모두에서 작동하는 방식으로 경로를 생성해주기 때문이다.  (윈도우와 리눅스가 가진 운영체제가 달라서 경로 설정하는 방식이 다른데, join은 실행중인 운영체제를 감지해서 자동으로 올바른 경로를 생성한다고 한다. )
- Helper 함수 : 상위 디렉토리를 얻어 올 수 있다.
    - 폴더를 생성한 후, 상위 디렉토리로 가는 경로를 구축한다.
    - join대신 dirname을 사용한다.
        - dirname은 경로의 디렉터리 이름을 회신한다.
    - ~~process.mainModule 전역 프로세스 변수를 사용해서 인자로 넘겨준다.~~
    
    ```jsx
    const path = require('path');
    
    module.exports = path.dirname(require.main.filename);
    //process.mainModule은 현재 deprecated 됐다. →require.main 을 사용
    ```