# 섹션 5

**express.js** : 제 3자 프레임 워크. 자질구레한 일들을 해결해 줘 더 깔끔한 코드를 짤 수 있게 하는 함수를 제공 

→ 데이터를 취급하거나 파싱하는 방법을 내부에 포함하고 있는 것은 아니지만 파싱을 대신해 주기 위해 프로젝트 내부에 쉽게 연결하는 다른 패키지를 쉽게 설치하도록 도와줌

// 대안… 바닐라 노드JS, Adonis.js, Koa, Sails.js 등등.. 

**프레임워크** : 도움 함수 이자 어플리케이션을 만들기 위한 도구와 규칙의 모음 

**미들웨어** : 코드를 다수의 블록으로 쪼개서.. 사용하는 개념. 

다음 미들웨어로 넘어가고 싶다면 next(); 사용. 위에서 부터 아래로 내려오는 순서. 

ex)

`app.use((req, res, next) ⇒ {`

`console.log(’In the middleware!’);`

`next();`

`});`

`app.use((req, res, next) ⇒ {`

`console.log(’In another middleware!’);`

`});`

ex2)

`app.use('/add-product', (req, res, next) ⇒ {`

`console.log(’In the middleware!’)`

`});`

`app.use('/', (req, res, next) ⇒ {` 

`console.log(’In another middleware!’);`

`});`

이 경우.. 위의 미들웨어에 next()가 없기 때문에 아래 미들웨어로 넘어가지 않음. 

아래 미들웨어는 ‘/’로 시작하는 모든 링크에 해당함.. 

get() 이나 post()를 통해서 필터링도 가능.. 

라우터 : 다른 express 앱에 연결되거나 꽂아 넣을 수 있는  express 앱과 같음

라우팅 : 네트워크에서 경로를 선택하는 프로세스 

사용할 때는 함수 말고 객체 자체로 언급하면 됨

`app.use(adminRoutes);`

필터 추가 가능. app.js 라우터 앞에 `‘/admin’` 식으로 경로를 넣어주면 위 경로로 시작하는 정보만 넘어감 

ex)

`// /admin/add-product ⇒ GET`

`router.get(’/add-product’, (req, res, next) ⇒ {`

`res.send(`

`‘<form action=”/admin/add-product” method=”POST”><input type “text” name “title”><button type>`

`);`

`});`

`// /admin/add-product ⇒ POST`

`router.post(’/add-product’, (req, res, next) ⇒ {`

`console.log(req.body);`

`res.redirect(’/’);`

`});`

view 폴더 : 애플리케이션에서 사용자들에게 제공하는 것들이 이 폴더에 들어가 있음 

404 페이지도 따로 만드는구나.. → 오류 처리의 개념과 비슷한 듯

`app.use((rerq, res, next) ⇒ {`

`res.status(404).sendFile(pate.joiin(__dirname, ‘views’, ‘404.html’));`

`});`

helper 함수.. 

상위 디렉토리로 쉽게 가주게 하는 helper 함수를 만들려면.. 

path.js 파일 

`const path = require(’path’);`

`module.export = path.dirname(process.mainMudule.filename);`

admin.js 파일

`const rootDir = require(’../util/path’);`

이렇게 해두면 admin.js 파일에서 ../ 이런식으로 경로를 입력하는 것이 아니라 rootDir로 대신할 수 있음