# week3

## 요청 및 사용자 간의 데이터 공유

→ admin에서 product를 사용자가 브라우저에서 입력을 하면 받고 그걸 shop.js에서 console로 받고 찍는다

```java
//admin js
const express = require("express");

const path = require("path");
const router = express.Router();
const rootDir = require("../util/path");
const products = [];
router.get("/add-product", (req, res, next) => {
  res.sendFile(path.join(rootDir, "views", "add-product.html"));
});

router.post("/add-product", (req, res, next) => {
  products.push({ title: req.body.title });
  res.redirect("/");
});

exports.routes = router;
exports.products = products;

//shop.js
const express = require("express");

const path = require("path");
const router = express.Router();

const adminData = require("./admin");
const rootDir = require("../util/path");
router.get("/", (req, res, next) => {
  console.log("shop.js", adminData.products);
  res.sendFile(path.join(rootDir, "views", "shop.html"));
});

module.exports = router;

//server.js
const path = require("path");
const express = require("express");
const app = express();
const bodyParser = require("body-parser");

const adminData = require("./routes/admin");
const shopRoutes = require("./routes/shop");

app.use(bodyParser.urlencoded({ extended: false }));
app.use(express.static(path.join(__dirname, "public")));
app.use("/admin", adminData.routes);
app.use(shopRoutes);

app.use((req, res, next) => {
  res.status(404).sendFile(path.join(__dirname, "views", "404.html"));
});
app.listen(3000);
```

admin에서 product 배열을 만든 다음에 post로 데이터가 전송되면 product 배열에 push를 해서 넣어준다 이떄 req.body는 bodyparser 때문에 undefined가 아닌 데이터로 뜬다 우리가 만약 {’age’:20,’name’:’뽀뽀뽀’,’hobby’: 캠핑} 이렇게 받으면 그 요청에서 title만 products에 저장하는거다 키-값 !!!

그리고 exports를 통해서 router랑 products를 내보내면 server.js에서 use를 통해서 adminData (router랑 products를 둘다 합한거 로직이랑 product 배열을 내보내는거임) 에서 routes를 실행하는 미들웨어를 추가한다

shop.js에서는 adminData.products를 받아서 콘솔로 출력

## 템플릿 엔진

템플릿이란? 

→ 맞춤제작하기 쉽도록 미리 만들어 놓은 웹 페이지를 의미한다. 일정한 틀, 형식을 의미한다.

### 서버 사이드 템플릿 엔진

서버에서 db 혹은 api에서 가져온 데이터를 미리 정의된 template에 넣어 html을 그려서 클라이언트에 전달해주는 역할을 한다.

과정

1) 클라이언트 요청을 받아서

2) 필요한 데이터 가져온다

3) 미리 정의된 template에 해당 데이터를 적절하게 넣는다

4) 서버에서 html을 그린다

5) 해당 html을 클라이언트에 전달한다

ex) ejs , pug, handlebars 등

### 클라이언트 사이드 템플릿 엔진

html 형태로 코드를 작성할 수 있으며 동적으로 dom을 그리게 해주는 역할을 한다.

과정

1) 클라이언트에서 공통적인 프레임을 미리 template으로 만든다

2) 서버에서 필요한 데이터를 받는다

3) 데이터를 template 을 적절한 위치에 replace하고 dom 객체에 동적으로 그려준다

ex) mustache, handlebars(Handlebars.js)

클라이언트 사이드 템플릿 엔진 필요성

javascript 라이브러리로 렌더링이 끝난뒤( 즉 html dom이 다 그려진 뒤) 서버 통신 없이 화면 변경이 필요한 경우

[https://gmlwjd9405.github.io/2018/12/21/template-engine.html](https://gmlwjd9405.github.io/2018/12/21/template-engine.html)[https://velog.io/@hi_potato/Template-Engine-Template-Engine](https://velog.io/@hi_potato/Template-Engine-Template-Engine)

[https://insight-bgh.tistory.com/252](https://insight-bgh.tistory.com/252)

### 템플릿 엔진의 필요성

1. 많은 코드를 줄일 수 있다

대부분의 template engine은 기존의 html에 비해서 간단한 문법을 사용한다.

1. 재사용성이 높다

웹페이지 혹은 웹 앱을 만들때 똑같은 디자인 페이지에 보이는 데이터만 바뀌는 경우가 굉장히 많다.

1. 유지보수에 용이하다

하나의 template을 만들어 여러 페이지를 렌더링 하는 작업에는 또 다른 이점이 있다. 

템플릿 엔진이 실사용에는 많이 안 씀

그냥 html인데 기능이 추가된 html로 생각하면 됨

path join이 그 경로를 합쳐서 경로를 만들어준다고 생각하면 됨

어느 코딩을 하든 쓰는 느낌이라 한번 감 잡아놓으면 좋겠음

# pug 사용하기

pug를 사용해서 shop.html  파일을 수정

### app.set

우리는 express에게 렌더링 하려고 하는 동적 템플릿이 있으며 이를 실시하기 위해서 이런 템플릿 엔진이 있고 템플릿은 어디있는지 express에 알려야 한다 (node로 진행하면 이거 다 수동)

이 함수를 통해 알려줘야한다

```java
app.set("view engine", "pug");
app.set("views", "views");
//app.set("views","./views");
```

첫번째 코드는 pug를 템플릿 엔진으로 사용할거라는 의미이고

두번째 코드는 views 파일이 모여있는 폴더를 지정하는거다 우리의 템플릿들이 저 폴더에 있다

실제 views 폴더가 하위폴더였으면 두번째 폴더처럼 적었을 수도 있다.

### res.render()

이 렌더함수는 뷰를 렌더링하는데 사용되며 렌더링된 html 문자열을 클라이언트에 보낸다.

### 코드

```java
//shop.pug
doctype html
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(http-equiv="X-UA-Compatible", content="IE=edge")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        title Document
        link(rel="stylesheet",href="/css/main.css")
        link(rel="stylesheet",href="/css/product.css")
    body 
        header.main-header 
            nav.main-header_nav
                ul.main-header_item-list 
                    li.main-header_item 
                        a.active(href="/") Shop
                    li.main-header_item 
                        a(href="/admin/add-product") Add Product
```

우리가 html로 쓰던걸 pug 방식에 맞게 쓴거

header 태그에 main-header 클래스 ..

아이디는 저기에 . 말고 #으로 적어야함

a태그를 적으면 href가 자동으로 나오고 

그런데 pug에서 가장 중요한 건 **들여쓰기**이다

들여쓰기로 어디서 부터 어디까지인지 구분하기 때문에 닫는 태그를 적을 필요가 없고 들여쓰기가 중요하다.

```java
router.get("/", (req, res, next) => {
  res.render("shop");
});
//shop.js
```

우리는 원래 shop.js파일에 sendFile함수를 사용해서 html 파일을 보내주었다. 하지만 render함수를 사용해서 응답을 변경함

❓ res.sendFile()과 res.render()의 차이는?

sendFile은 어떤 파일을 그대로 보내고 싶을 때 쓰고 render 파일을 보내기전에 ejs파일,pug파일 이런 파일을 html을 바꾸고 싶을 때 사용하는 것이다.

브라우저는 ejs 파일 pug 파일을 모르고 html만 알기 때문이다.

# 동적 컨텐츠 출력

우리가 실제 관리하는 데이터를 템플릿에 투입해야 함

html에서는 데이터 관리를 안하니까 !

render 함수는 실제

```java
res.render(view [, locals] [, callback])
```

이런식으로 구상이 되어있다. 여기서 local은 우리가 뷰(템플릿)에다가 전달할 객체이다. 

[https://www.geeksforgeeks.org/express-js-res-render-function/](https://www.geeksforgeeks.org/express-js-res-render-function/)

우리가 view에 추가되어야하는 데이터를 전달하는 것

이때 데이터를 전달하면 객체 형태로 전달해야 한다. 키랑 값을 맵핑한뒤에

우리는 전에 product를 입력하면 그 값이 title로 담겨있는 배열인 products를 전달하고 싶다 그런데 이때 객체 형태로 전달하고 싶으니까 

```java
res.render('shop',{prods:products, docTitle:'Shop'}););
```

이런 식으로-!

객체를 담은 배열인 products의 키를 prods로 설정해 docTitle과 객체로 묶어 전달한다.

```java
main
            if prods.length > 0
                .grid
                    each product in prods
                        article.card.product-item
                            header.card__header
                                h1.product__title #{product.title}
                            div.card__image
                                img(src="data:image/jpeg;base64,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", alt="A Book")
                            div.card__content
                                h2.product__price $19.99
                                p.product__description A very interesting book about so many even more interesting things!
                            .card__actions
                                button.btn Add to Cart
            else
                h1 No Products
```

(태그를 안 적고 .class만 적으면 div로 인지)

prods 즉 products를 담은 게 길이가 0보다 크냐 products가 하나라도 있으면 밑에 코드를 실행

each product in prods

### pug의 반복문

중에서 each in을 사용해서 값을 가져올 수 있다.

[https://inpa.tistory.com/entry/PUG-📚-제어문-조건문-반복문](https://inpa.tistory.com/entry/PUG-%F0%9F%93%9A-%EC%A0%9C%EC%96%B4%EB%AC%B8-%EC%A1%B0%EA%B1%B4%EB%AC%B8-%EB%B0%98%EB%B3%B5%EB%AC%B8)

products는 상품으로 보고

prods는 상품 나열

**+우리가 데이터 변경으로 인한 화면 변경은 서버 재시작이 아니다**

템플릿만 변경은 서버 재시작이 아니다

# Pug 레이아웃 기능

### 레이아웃 왜 사용해?

우리가 html을 계속 반복적으로 쓰는 구간이 있다 이것은 굉장히 성가신 일 !

우리는 레이아웃을 만들어서  block으로 다른 템플릿을 상속 받을 수 있음

block으로 다른 코드에서 가져와다가 그 부분에 넣는 느낌이라고 생각하면 좋음

**즉 extends 키워드로 템플릿을 상속 받고 block 키워드로 원하는 데이터만 쏙 넣어줄 수 있다**

extends에 폴더명 파일명 적고  block이 코드를 담고 있다..?

extends layout.pug를 통해서 부모 템플릿을 상속 받으면 레이아웃.pug의 해당 block에 body.pug에서 설정한 block이 입력되서 html이 완성되는 식이다

동적으로 추가도 가능

Include 기능을 사용하여 한 pug 파일의 내용을 다른 pug 파일에 삽입할 수도 있다

하지만 include를 사용하더라도 중복되는 코드가 많이 발생하고 include 조차 중복해서 작성해줘야하는 불편함 이때 사용하면 좋은 키워드가 extends와 block 키워드다. layout 하나의 파일에 작성해 extends 키워드를 사용해 다른 파일에서 가져다 쓸 수 있따

`include와 extends의 차이점`은 extends는 include처럼 단순히 가져다 쓰는 것이 아니라 내부 코드에 block 키워드를 이용해서 각 파일별로 필요한 코드들을 원하는 위치에 추가하는 식으로 확장

상속해서 사용 !!

[https://on1ystar.github.io/node.js/2021/05/05/Node-7/](https://on1ystar.github.io/node.js/2021/05/05/Node-7/)

### 동적으로 active 클래스 추가

```java
//main-layout.pug
doctype html
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(http-equiv="X-UA-Compatible", content="IE=edge")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        title #{PageTitle}
        link(rel="stylesheet", href="/css/main.css")
        block styles 
    body 
        header.main-header
            nav.main-header__nav
                ul.main-header__item-list
                    li.main-header__item
                        a(href="/" , class = (path ==='/' ? 'active' : '')) Shop
                    li.main-header__item
                        a(href="/admin/add-product" , class = (path ==='/admin/add-product' ? 'active' : '')) Add Product
        block content
//shop.js
res.render("shop", { prods: products, PageTitle: "Shop", path: "/" });
```

a 태그 부분을 보면 class를 . 으로 설정하지 않고 객체에서 받은 path 값이 현재 이 경로랑 같으면 이 css 선택자를 ‘active’로 해준다 이 경로는 shop.js처럼 path 키해서 값을 전달해준다

**handlbars 장점**

handlebars는 다른 템플릿 엔진에 비해 상대적으로 html을 거의 훼손하지 않았고 서버 사이드 뿐만 아니라 클라이언트 사이드에서도 단독으로 동작할 수 있다는 장점이 있다

[https://www.hanumoka.net/2018/11/28/node-20181128-node-express-handlerbars/](https://www.hanumoka.net/2018/11/28/node-20181128-node-express-handlerbars/)

먼저 handlebars는 pug와 달리 express의 built in 엔진이 아니여서 engine 설정을 해주어야 한다

```jsx
const expressHbs = require("express-handlebars");

app.engine("hbs", expressHbs());
app.set("view engine", "hbs");
app.set("views", "views");
```

일단 import 해준다

app.engine()에서 첫번째 인자는 엔진 이름을 설정해주고 두번째는 아까 객체가 함수 역할 하는거 expressHbs()가 초기화된 뷰 엔진을 return 함으로써 엔진을 설정해준다

엔진이름을 설정 해주었으면 pug랑 똑같이 set에 놓는다

[https://moonheekim-code.tistory.com/105](https://moonheekim-code.tistory.com/105)

## IF문에 헬퍼 함수 사용

handlebars는  `{{#if a==3}}`과 같은 if문을 제공하지 않는다

if 자체가 헬퍼 메서드로 등록되어 있는데 if 헬퍼 함수에 전달하는 값을 true/false 여부만 판단할 뿐이고 일반적인 자바 스크립트 처럼 저런식의 표현은 제공하지 않는다

그래서 객체에서 논리를 따질 수 있는 걸 키 값 매핑해서 전달해야한다.

또한 블록의 마지막을 닫으려면 똑같이 적어줘야함

expressHbs 함수 안에 레이아웃 설정

```java
app.engine(
  "hbs",
  expressHbs({
    layoutsDir: "views/layouts/",
    defaultLayout: "main-layout",
    extname: "hbs",
  })
);
```

폴더는 views/layouts 파일은 main-layout  extname을 사용해서 확장명 설정

- defaultLayout: 기본 값으로 **main** 으로 되어 있으며, 설정된 값에 따라 main layout의 파일명을 설정 할 수 있다.
- extname: 확장자를 설정할 수 있습니다. 기본은 handlebars 이다.
- partialsDir: **partial** 이란 레이아웃을 채울 파일들을 의미하며, **partial** 의 위치를 설정 할 수 있습니다. 기본은 partials 이다.
- layoutsDir: **layout** 파일 위치를 지정 합니다. 기본 값은 layouts 이다.

키-값으로 논리 넘겨 주기

ex)

```java
//main-layout.hbs
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{{ pageTitle }}</title>
    <link rel="stylesheet" href="/css/main.css">
    {{#if formsCSS}}
        <link rel="stylesheet" href="/css/forms.css">
    {{/if}}
    {{#if productCSS}}
        <link rel="stylesheet" href="/css/product.css">
    {{/if}}
</head>

<body>
    <header class="main-header">
        <nav class="main-header__nav">
            <ul class="main-header__item-list">
                <li class="main-header__item"><a class="{{#if activeShop }}active{{/if}}" href="/">Shop</a></li>
                <li class="main-header__item"><a class="{{#if activeAddProduct }}active{{/if}}" href="/admin/add-product">Add Product</a></li>
            </ul>
        </nav>
    </header>
    {{{ body }}}
</body>

</html>
```

handlebar에서는 pug와 다르게 block이 없다

body가 그 역할을 한다고 생각

블록을 정의할 수는 없고 여는 중괄호 닫는 중괄호 3개씩 사용하여 정의

객체에 받은 값 중 하나인 formsCSS가 true면 forms.css파일을 가져오고 ..

 

```java
//shop.js
const express = require("express");

const path = require("path");
const router = express.Router();

const adminData = require("./admin");
const rootDir = require("../util/path");
router.get("/", (req, res, next) => {
  const products = adminData.products;
  res.render("shop", {
    prods: products,
    PageTitle: "Shop",
    path: "/",
    hasProducts: products.length > 0,
    activeShop: true,
    productCSS: true,
  });
});

module.exports = router;
```

객체 안에 아까 위에서 보았던 productCSS나 activeShop이나 설정 …

그리고 layout:false라는 특별한 키값이 있는데 이걸 적어주면 기본 레이아웃을 사용하지 안는다 이 설정이 없으면 사용하게 된다.

또한 전체 파일이 아닌 레이아웃에만 적용되는 확장명을 설정해 레이아웃을 제외한 모든 파일에 이 부분이 적용되게 한다( 저.. 위에 코드)

### ejs란?

- embedded javascript의 약자로 자바스크립트가 내장되어 있는 html 파일이다
- node.js에서 많이 사용하는 템플릿 엔진으로 ejs는 html 안에서  <% %>를 이용해서 서버의 데이터를 사용하거나 코드를 실행할 수 있다
- html 태그처럼 자바스크립트 내용을 삽입할 수 있다.매우 큰 강점 -!!

### 문법

- <% %>

이 태그는 자바스크립트를 실행할 수 있다

- <%= %>

이 태그는 변수 값을 내장시킬 수 있다

- Include?

<% -include(’경로입력’%> 페이지 내 반복되는 header나 footer등의 코드를 includes 폴더안에 작업하고 include를 이용해서 파일로 임포트하면 간편하게 레이아웃을 작업할 수 있다(ejs는 따로 제공 레이아웃 기능이 없어서  일부 코드 블록을 템플릿 내부의 다양한 부분들에서 재사용함으로써 템플릿에서 이들을 공유)