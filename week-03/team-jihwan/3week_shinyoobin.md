# 섹션6

**모듈**

node express 백엔드에서 어떻게 데이터베이스를 관리하는지.

동적 콘텐츠를 렌더링 하는 법. (각각의 콘텐츠 마다 다른 html 페이지를 제공)

엔진을 템플레이팅 하는 방법의 이해.

### **제품명 저장 - 객체 저장**

admin.js  

```jsx
const produncts = [];

exports.routes = router;
exports.products = products;

router.post('/admin-product',(req, res, next) => {
	products.push({title: req.body.title});
	res.redirect('/');
});
```

app.js

```jsx
app.use('/admin',adminData.routes); 
//adminRoutes를 불러오는 부분을 바꿔주는 것.. 
//adminData는 모든 내보내기를 지칭하기 때문에 routes인지 products 인지 지정 해 주어야함
```

shop.js

```jsx
const adminData = require('./admin');
```

객체나 배열같은 참조유형을 내보내기 하면 다른 파일에서 변경하면 저절로 업데이트 됨 → 데이터가 공유된다는 뜻 !

**문제점** : 서버가 가지고 있는 정보가 사용자 간에 공유됨 → 보안 문제 발생할 수도.

### **템플릿 엔진**

템플릿을 생성하고 동적 콘텐츠를 투입하여 html 파일을 얻을 수 있음 

- EJS
    - `<p><%= name %></p>`
    - 일반 html을 사용하고 단순한 js를 사용할 수 있게 하는 플레이스 홀더가 있어 for루프를 위해 if문을 사용할 수 있음
- Pug (Jade)
    - `p # { name }`
    - 최소 html 구문과 확장 가능하지만 일반적으로 일련의 요소나 작업 종류만을 제공하는 맞춤형 템플릿 언어를 사용하지만 if문들과 목록도 포함되고, 반복문도 포함될 수 있음
- Handlebars
    - `<p>{{ name }}</p>`
    - 일반 html을 사용하지만 동시에 제한된 기능의 맞춤형 템플릿 언어도 사용하고, if문이나 목록 등의 공통요소들을 포함함

### Pug

```jsx
npm install --save ejs pug express-handlebars
//여러개의 패키지를 한 번에 설치 가능.
```

app.js

```jsx
app.set('view engine', 'pug');
//전체 구성 값을 설정한 것.
//app.set()으로 express 애플리케이션 전체에 어떤 값이든 설정할 수 있음
//expresss가 이해할 수 없는 키나 구성항목도 포함함.

//view 엔진은 우리가 렌더링 하려고 하는 동적 템플릿이 있으며 이를 실행하기 위한 함수가 있으니 이 엔진을 사용해 달라고 알림

app.set('views', 'views');
```

views 폴더에 shop.pug 파일 생성 

특이한 문법을 가짐..

```jsx
header.main-header
	nav.main-header_nav
		a.activate(hreef="/") Shop
```

shop.js

```jsx
router.get('/', (req, res, next) => {
	res.render('shop')
});
```

적용 - admin.js

```jsx
router.get('/add-product', (req, res, next) => {
	res.render('add-product', { pageTitle: "Add Product', path: '/admin/add-product'});
});
//path를 view로 넘기면 어느것에 대한 경로가 로딩된 것인지 알아냄

//main 페이지의 path도 모두 '/admin/add-product'
```

### 핸들바

일반 html + 템플릿

app.js

```jsx
const expressHbs = require('express-handlebars');

app.engine('handlebars', expressHbsf()); 
//pug는 내장된 엔진이 있지만 핸들바는 아니라서.
app.set('view engine', 'handlebars');

```

404.handlebars 파일 생성 가능.. 

- Pug 참고자료: [https://pugjs.org/api/getting-started.html](https://pugjs.org/api/getting-started.html)
- Handlebars 참고자료: [https://handlebarsjs.com/](https://handlebarsjs.com/)
- EJS 참고자료: [http://ejs.co/#docs](http://ejs.co/#docs)