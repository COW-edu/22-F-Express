# Express.js

- 들어오는 요청을 분석하기 위해 아래의 코드를 짬.
    - body를 추출하기 위해 직접 데이터 이벤트와 end 이벤트를 사용하고 난 후 마지막에는 문자열로 변환했던 버퍼를 생성해야 했음

```jsx
req.on('data', (chunk)=>{
            console.log(chunk);
            body.push(chunk);
        });
        return req.on('end', ()=>{
            const parsedBody = Buffer.concat(body).toString();
            ...
            });
        });

//routes.js파일
```

- 데이터를 취급하거나 파싱하는 방법을 내부에 포함하고 있는 것은 아니지만 파싱을 대신해 주기 위한 프로젝트 내부에 쉽게 연결하는 다른 패키지를 쉽게 설치하도록 도와줌
    - 파싱: 문서나 HTML 등 어떤 큰 자료에서 내가 원하는 정보만 가공하고 추출해서 원할 때 불러올 수 있게 하는 것
        - 특정 패턴이나 순서로 추출해서 정보로 가공할 수 있음
- 특징
    - 매우 우연하며, 특별한 기능을 과도하게 추가하지 않음
    - 애플리케이션을 구축하거나 유입되는 요청들을 처리함에 있어서 특별한 방법을 제공해 높은 확장성을 가진다 → Express를 대상으로 구축된 제 3자 패키지의 수는 수천개 존재하며 쉽게 Node Express 애플리케이션에 추가할 수 있음
    - 깔끔한 코드를 작성하는 데 도움을 줌

- 미들웨어
    - Express.js는 미들웨어와 깊게 연관되어있음
        - Request → Middleware (req,res,next)⇒{}
        -next()→ Middleware (req,res,next)⇒{}
        -res.send()→Response
    - 미들웨어: 들어오는 요청을 express.js에 의한 다양한 함수를 통해 자동으로 이동하는 것. 
    → 단일 요청 핸들러를 보유하는 대신, 우리는 응답을 전송하기 전까지 요청이 통과하게 될 다양한 함수들을 연결한 가능성을 확보하게 된다.
    - 모든 것을 하나의 함수를 사용해 처리하기 보다 코드를 다수의 블록들로 분할할 수 있음.
    - 위→ 아래 순서로 실행됨
    
    - use 메서드 → 새로운 미들웨어 함수를 추가할 수 있음
        - 사용이 상당히 유연함
        - 요청 핸들러 배열을 받아들임
        
        ```jsx
        app.use((req,res,next)=>{});
        ```
        
        → app.use로 전달하는 함수는 모든 들어오는 요청마다 실행된다.
        
        - 인수
            - path: 경로 형태 → 우리가 찾고자 하는 대상
                - 특정 요청만을 걸러낼 수 있음
                - 미들웨어를 활용해 무엇이 표시되는 지 제어하는 방법
                    
                    ```jsx
                    app.use('/add-product',(req,res,next)=>{
                    	res.send(`<h1>add product</h1>`);
                    });
                        
                    app.use('/',(req,res,next)=>{});
                    	res.send(`<h1>hello express</h1>`);
                    ->'/'
                    ```
                    
                    → 도메인 뒤에 오는 전체 경로가 /로 시작하면 실행됨!
                    
                    → /add-product 에서만 화면에 add product가 출력되고, /로 시작되는 경로는 hello express가 출력됨
                    
                - 응답을 보내려고 한다면 next를 호출하지 않는 것이 좋음 → **하나 이상의 응답을 보내려고 하면 오류 발생**
            - callback: 실행해야 하는 함수
                - 하나 이상 있을 수 있으며
                - 원하는 개수가 가능
        - next()
            - Express.js를 통해 app.use로 전달되는 함수
            - 다음 미들웨어로 요청이 이동할 수 있도록 실행되어야 함
            - 미들웨어 사이를 이동할 수 있음
            
            → next를 호출하지 않는 경우 응답을 보낼 수 있는 장소에 아무것도 도달하지 않게 됨
            
        - send()
            - 응답을 보낼 수 있음
            - 기본적으로 html 콘텐츠 유형을 선택해줌
            - setHeader를 이용해 수동으로 선택할 수 있음
            
            ```jsx
            app.use((req,res,next)=>{
            	console.log("First!");
            	next(); -> 다음 미들웨어로 이동할 수 있게 해줌
            });
            
            app.use((req,res,next)=>{
            	console.log("Second!");
            	res.setHeader
            	res.send(`<h1>hello express!</h1>`);
            });
            ```
            
    
    <aside>
    💡 요청이 통과하는 funnel에 연결된 기능을 추가하고, next로 다음 미들웨어로 넘어가 응답을 보내게 됨.
    
    </aside>
    
    - funnel : 강의 내에서 미들웨어의 동작 방식을 설명하기 위해 사용, pipe and filter pattern과 같은 전처리 기능이라는 동작 방식을 전하고 싶었던 것으로 보임!
        - pipe and filter pattern: 데이터 스트림 (데이터 흐름) 절차의 각 단계를 필터 컴포넌트로 캡슐화해 파이프를 통해 데이터를 전송하는 패턴
            - 다양한 파이프라인을 구축하는 거싱 가능
            - 재사용성이 좋고, 추가가 쉬워 확장이 용이
        - 미들웨어같은 경우, 라우터 핸들러 전까지만 동작을 하고 pipe pattern처럼 **특정 목적에 맞는 것들을 구분지어서 구현하고 이를 순차적으로 적용하는 방식**으로 전처리 과정을 구현함
- 서버를 구축할 때 코드를 더 짧게 해줌
    
    ```jsx
    const server=http.createServer(app);
    server.listen(3000);
    
    ->
    
    app.listen(3000);
    ```
    
    → listen이 http.createServer를 호출하고, 자신을 전달하는 두 가지의 기능을 함.
    

## 수신 요청 분석

```jsx
app.use('/add-product',(req,res,next)=>{
    res.send(`<form action="/product" method="POST"><input type="text" name="title"><button type="submit">Add Product</button></form>`);
});

app.use('/product',(req,res,next)=>{
    console.log(req.body);
    res.redirect('/');
});
```

→ button에 항목( ex)book )을 입력해도 콘솔에 undefined로 뜸 

- 리다이렉트(Re-direct): 서버가 클라이언트가 요청한 URL에 대해 다른 URL로 재접속하라고 명령을 보내는 것
- req. 는 이 코드에 편의 속성을 주지만, 기본적으로는 들어오는 요청의 본문을 분석하려 하지 않음 → 따라서 또 다른 미들웨어를 추가로 구현해 분석기를 등록함
    - 일반적으로 경로 처리 미들웨어 이전에 둠 ~> 요청이 어디로 향하는 본문 분석이 이뤄지도록 하기 위해
    - 제3자 패키지 (bodyParser)
        
        ```jsx
        app.use(bodyParser.urlencoded({extended: false}));
        //미들웨어를 등록해 줌
        //extended -> 비표준 대상 분석 가능 유무를 나타냄
        
        app.use('/add-product',(req,res,next)=>{
            res.send(`<form action="/product" method="POST"><input type="text" name="title"><button type="submit">Add Product</button></form>`);
        });
        
        app.use('/product',(req,res,next)=>{
            console.log(req.body);
            res.redirect('/');
        });
        ```
        
        → 파일, JSON 등은 분석 X(다른 분석기를 사용하면 됨), 폼을 통해 전송된 본문을 분석
        
    
    <aside>
    💡 모든 http 메서드에 반응하는 use 대신에 get이나 post를 이용해 해당 요청만 걸러낼 수 있음
    
    </aside>
    
    - app.get()
        - use와 같은 기능
        - 경로를 사용하거나 사용하지 않을 수 있고, 들어오는 GET 요청에만 작동함
    - app.post()
        
        ```jsx
        app.post('/product',(req,res,next)=>{
            console.log(req.body);
            res.redirect('/');
        });
        ```
        
        - 이 경로에 해당되는 들어오는 POST 요청만 실행되고 GET 요청에는 반응하지 않음.

## 라우팅

: 주소(IP)를 정하고, 경로를 선택하고, 패킷을 전달하는 것

→ 목적에 따라 효율적인 프로토콜을 선택하는 것

- 네트워크에서 패킷을 보낼 때 목적지까지 갈 수 있는 여러 경로 중 하나의 경로를 설정해주는 과정
- 파일 크기가 커짐에 따라서 일반적으로 라우팅 코드를 여러 파일로 나누는 것이 좋음 → **유지보수를 위해**서 나눈다, 파일 크기가 커지면 실행에 시간이 오래걸림
    - Express.js는 라우팅을 다른 파일에 위탁하는 방법을 제공
    - 라우팅에 의한 결과 = **라우트**
- 아웃소싱 라우트들은 시작 경로를 공유하는 경우가 있음
    - 아웃소싱 → routes/admin.js 에서 /admin/add-product 라고 써야하는데 /admin을 계속 써야할 수 없으니까 get, post를 각각 아웃소싱됐다고 말함
    - 주어진 파일의 모든 루트가 app.js 파일로 위탁하는 데 사용할 공통 경로를 앞에 추가할 수 있음 → 모든 루트에 대해 반복해서 입력할 필요가 없음
    
    ```jsx
    app.use('/admin',adminRoutes);
    ```
    
    → admin으로 시작하는 라우트만 adminRoutes 파일로 들어감
    
    → /add-product는 /admin/add-product 라우트에 매칭됨
    
    ~> /admin/add-product여야만 폼이 나타남
    

## 에러처리

- status(): 상태 코드 설정
    - 모든 메서드 호출을 연계할 수 있음

```jsx
app.use(bodyParser.urlencoded({extended: false}));

app.use(adminRoutes);
app.use(shopRoutes);

//미들웨어 맨 마지막에 작성
app.use((req,res,next)=>{
    res.status(404).send('<h1>Page not found</h1>')
})
```

→ 더미 경로를 불러오면 화면에 Page not found 출력

- 400 (Bad Request) → 사용자의 잘못된 요청
    - 401 (Unauthorized) → 인증되지 않음
    - 403 (Forbidden) → 클라이언트가 컨텐츠에 접근할 권리를 가지고 있지 않음
    - 404 (Not Found) → 요청한 페이지 없음

## html 파일과 연결

- vs code 파일 목록
    - routes
        - admin.js
        - shop.js
    - views
        - add-product.html
        - shop.html

```jsx
//shop.js 파일

const path=require('path');

router.get('/',(req,res,next)=>{
    res.sendFile(path.join(__dirname,'../','views','shop.html'));
});

// ../ 대신 .. 사용 가능
```

→path.join() : 생성한 경로로 파일을 보냄

→ __dirname : 우리 os의 절대 경로를 이 프로젝트 폴더로 고정해 주는 전역 변수

 - 절대경로: 최상위 위치에서 시작

 - 상대경로: app.js에서 시작한다고하면 app.js를 중심으로 찾아가는 것 (나 기준)

## 정적으로 파일 서비스하기

- 정적: express.Router나 다른 미들웨어 소프트웨어에서 처리되지 않고 파일 시스템으로 직접 포워딩된다는 뜻
- html 파일에 작성했던 css코드를 따로 파일로 만들어서 사용
    
    ```jsx
    app.use(express.static(path.join(__dirname,'public')));
    ```
    
    → public 폴더 내에 존재하는 css 접근을 위해 app.js에 작성
    
    → 사용자가 public 경로에 접근할 수 있음
    
- css, js 이미지 등을 제공 할 수 있음