# 2주차

# Section 5

## Express.js란?

- Express.js는 강력한 Node.js 프레임워크이며 많은 유틸리티 함수들과 도구들, 그리고 앱을 어떻게 구성해야 하는지에 대한 규칙들을 더한 패키지이다.
- 다른 패키지가 parsing을 하도록 프로젝트 내부에 데이터를 쉽게 연결하는 것을 도와준다.
    - parsing : 일련의 문자열을 컴퓨터가 처리할 수 있게 의미있는 token으로 분해하는 것
- 애플리케이션을 구축하고 들어오는 요청들을 처리할 때 다른 패키지들이 쉽게 추가될 수 있게 높은 확장성을 가지고 있다.(유연하다.)

```r
npm –save express #설치

const app = express(); #app.js에 추가
```

이렇게 app.js에 상수 app을 추가하는 순간 app에 많은 로직이 들어있게 된다. 

app은 유효한 요청 핸들러이기도 하다.

## Middleware.next(), middleware.res()

- Express.js는 미들웨어 함수의 의존도가 높기 때문에 쉽게 use()함수를 이용하여 쉽게 미들웨어 함수를 더할수있다.
- Middleware 함수는 요청 객체나 응답 객체를 받아서 다양한 함수를 이용해 자동으로 처리하는 함수이다. (객체를 하나의 함수를 이용해 처리하기보다는 처리하는 함수를 여러 블록들로 분할해 처리할 수 있는 것이 특징이다.)
- app.js에서 app객체를 생성한 시점과 서버를 생성하기 위해 app을 전달한 시점 사이에서 사용할 수 있다.

```r
#ex)
const app = express();

app.use()

const server = http.createServer(app);
```

```r
app.use((req, res, next) => {})
```

- 요청을 다음 순서의 미들웨어로 향하게(강의에서는 forwarding이라고함)할 때 next 인수를 사용해야한다.
- 순서.. 루트파일의 위에서부터 아래로 향하는 방향을 의미한다.
- 응답을 전송하는 것이 아닌 이상 항상 next를 호출해야 하고 응답을 전송하려면 절대 next를 호출해서는 안된다.(이때 요청을 변환하고, 정보를 읽어내고, 액세스하는 라우트에 따라 다른 응답을 전송하기 위해 미들웨어를 잘 설계한다.)

```r
app.use((req, res, next) => {
console.log(‘In the middleware!’);
next(); # 다음 미들웨어를 호출하게 도와준다
});

app.use((req, res, next) => { 
console.log(‘In another middleware!’);
}); 
```

이때 3번째 줄의 next()를 호출하지 않으면 다음 순서의 미들웨어로 넘어가지 않는다.

```r
app.use((req, res, next) => {
console.log(‘In last middleware!’);
res.send(’<h1>text<h1>');
});
```

res.send()메소드는 자동으로 content-type을 html로 바꿔준다.

(setHeader을 통해 수동으로 content-type을 설정할수도있다)

```r
const server = http.createServer(app);

server.listen(3000);
```

 위 코드를

```r
app.listen(3000);
```

으로 바꿀수있다. 

listen() method는 http.createServer를 호출하고 자신을 전달하는 역할을 한다. 

이후 우리가 전달한 앱 객체를 createServer로 전달해주고, 최종적으로는 해당 서버 객체에서 listen이 호출되도록 해준다. 

## Routing

- app.use를 이용해 path(경로)나 method로 요청을 필터링할수있다.(app.get(GET요청에만 작동), app.post(POST요청에만 작동)를 사용해도 무관)
- callback 함수 : 나중에 실행해야 하는 함수

```r
app.use(‘/’,(req, res, next) => {console.log(“1”)})
#localhost:3000/으로 시작하는 모든 주소의 콘솔에 1을 출력
```

- 들어오는 요청을 처리하고 데이터를 추출하는 방법
    
    ```r
    app.use(‘/add-product’, (req, res, next) => { res.send(’Add Product ‘);}); 
    #submit버튼을 누르면 form의 내용을 /product의 경로로 POST method를 이용해 요청을 보낼 것이다.
    ```
    
    - product 요청을 처리할 라우트 또는 미들웨어가 필요하다.
    
    ```r
    app.post(‘/product’, (req, res, next) => {
    	console.log(req.body); res.redirect(‘/’); #자동으로 / 경로로 리다이렉트 된다.
    });
    #위의 것보다 전에 위치해야한다. 
    ```
    
    - Body parser
        - 위 코드들이 실행 될때, console.log(req.body);는 콘솔에 undefined가 뜨는 것으로 실행이 된다. 그 이유는 req가 들어오는 요청의 본문을 분석하려 하지 않기 때문이다. body parser는 이것을 분석해준다.
            
            ```r
            # npm install –save body-parser
            
            app.user(bodyParser.urlencoded(extended: false));
            ```
            
- express.Router 패키지를 이용할수도 있는데 이는 루트를 여러 파일에 걸쳐 나눌수 있고, 내보낸 route는 루트 파일의 app.usedp 미들웨어 함수로 추가할 수 있다. -처리되지 않는(경로를 찾지 못했다거나) 몇몇 요청은 route를 처리하는 코드들 맨 아래에 `app.use(‘/’, (req, res, next) =>{ res.status().send(’<h1>Page not found<h1>‘); });` 를 작성해서 해결할 수 있다.
- 아웃소싱 라우트
    - 시작 경로를 공유한다 ex)admin/a 와 admin/b는 같은 시작 경로를 공유한다.
    - 앞에서 get,post,use method를 사용할때, 경로에 시작경로를 추가함으로써 필터링이 가능하다.

## Serve Files

- sendFile()을 통해 HTML같은 언어의 파일들을 유저에게 보낼수도있고, CSS나 JavaScript 파일에 대한 직접적인 요청도 가능하다.

```r
const path = require(‘path’);
res.sendFile(path.join(__dirname, ‘views’, ‘shop.html’));
#path.join()은 자동으로 리눅스와 윈도우 시스템 모두에서 작동하는 방식으로 경로를 생성해준다.
#__dirname은 현재 파일이 들어있는 폴더를 가리킨다.
```

- Express.static()을 사용해 이러한 파일들(CSS, JS파일)에 대한 정적 서비스도 가능하다.
    - 정적…express.Router나 다른 미들웨어 소프트에서 처리되지 않고 파일 시스템으로 직접 포워딩된다는 뜻 -
    
    ```r
    app.use(express.static(‘path.join을 이용해 읽기 액세스를 허용하고자 하는 폴더의 위치를 입력’));
    #정적 파일을 서비스 해준다
    ```