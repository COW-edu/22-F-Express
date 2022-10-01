# Section6

## 79. 요청 및 사용자 간의 데이터 공유

입력 받은 객체를 저장해보자.

1. admin.js파일에 변수를 추가하자. 새로운 상수로 products 배열을 만들자.
const products = [];
참고로, 배열 자체는 객체이기 때문에 새로운 요소를 받을 수 있다! 요소를 추가하거나 제거해도 전체적인 객체에는 영향이 없다.
2. products 배열 내보내자.
exports.products = products;
3. router.post에 products.push로 배열에 새로운 객체를 푸시하자.
4. title은 req.body.title로 추출하자.
5. 모든 제품을 출력하는 shop.js파일에서 products를 접근 가능하게 하자.
const adminData = require(’./admin’);

```jsx
const products = [];

router.post('/add-product', (req, res, next) => {
  products.push({ title: req.body.title });
  res.redirect('/');
});

exports.routes = router;
exports.products = products;
```

## 81. Pug 설치 및 구현

1. 패키지 이름은 ejs로 하는 템플릿 엔진을 설치해보자.
npm install —save ejs pug express-handlebars
2. pug를 사용하려면 app.js 파일로 가서 Express.js에게 알려야 한다. Node만 단독으로 사용하면 직접 수동으로 해야하는 부분이 많기에, Express를 준수하는 템플릿 엔진을 사용하겠다고 Express에게 알리기만 하면 된다.
app.set(’view engine’, ‘pug’);
3. views폴더에 shop.pug 파일을 생성하자.
4. pug는 html과 코드 형식이 다르다. html:5를 입력하면 템플릿이 제공된다.
5. link 태그로 css 경로 입력하자. pug 코드 형식에 맞게 body 부분을 작성해보자.

```html
<!DOCTYPE html>
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        meta(http-equiv="X-UA-Compatible", content="ie=edge")
        title My Shop
        link(rel="stylesheet", href="/css/main.css")
        link(rel="stylesheet", href="/css/product.css")
    body
        header.main-header
            nav.main-header__nav
                ul.main-header__item-list
                    li.main-header__item
                        a.active(href="/") Shop
                    li.main-header__item
                        a(href="/admin/add-product") Add Product
```

1. shop.js에서 응답을 변경해주자.
res.render(’shop.pug’);
참고로, 기본값이 pug로 설정되어 있기에, res.render(’shop’);이라 입력해 주어도 된다.

## 82. 동적 콘텐츠 출력

1. 관리자 데이터를 가져와 템플릿으로 전달한 뒤, 출력해보자. render 메서드의 두 번째 인수에 전달하면 된다. JavaScript 객체 형태로 전달하며 키 이름에 맵핑하면 된다.
res.render(’shop’, {prods: products, docTitle: ‘Shop’});
2. ‘Shop’를 출력하고 싶은 부분에 #{docTitle}을 입력하면 된다.
3. pug 문서의 코드 작성을 완료해보자.

```html
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

1. 결과를 반복하기 위해 each 키워드를 사용해보자. 루프가 생성되었으니 값을 추가해 주며 저장한다. in을 적어준 뒤 루프하길 원하는 배열인 prods를 입력해 준다.
2. if-else 문을 활용하여 제품이 있다/없다를 출력해주자.

## 85. 레이아웃 추가

기본 설정과 main.css 및 헤더 등의 불러오기는 각 페이지의 일부이기에 매번 불러오는 것을 수동으로 반복하기는 번거롭다. 그러니, 레이아웃을 생성해보자.

1. views 폴더에서 layouts 폴더를 만들자.
layouts 폴더 내에 main-layout.pug 파일도 만들자.
2. pug이 인식하는 block 키워드를 추가하고, 이름은 styles라 정의해보자.
block styles
3. 헤더 아래의 콘텐츠도 동적으로 만들어보자. 동적이며 블록으로 보존해두자.
block content
4. 이제, 404.pug 페이지로 가서 확장하자.
5. pug에서 인식하는 extends 키위드를 추가하여 레이아웃을 확장할 수 있다.
extends layouts/main-layout.pug
6. styles의 경우 별도의 특별한 설정이 필요하지는 않다. 하지만 콘텐츠 블록은 필요하니, block content를 다시 설정해주어야 한다.
block content
    h1 Page Not Found!
7. add-product.pug에서도 같은 작업을 진행해보자.
8. 콘텐츠 블록에는 main의 내용을 추가하자.
9.  styles 블록에도 css 파일을 연결하는 link를 작성하자.

⇒ 레이아웃을 통해 훨씬 간결한 접근법이 되었다. 이제, 수정할 때 모든 파일을 다 변경하는 것이 아닌, 레이아웃 파일만 변경해주면 된다. 다른 파일들은 extend가 자동으로 반영해 주기 때문이다.

## 86. Pug 템플린 완성하기

## 88. 핸들바 작업

세가지 옵션 중 하나인 Pug에 대하여 알아보았다. 이번에는 Handlebars를 알아보자.

1. 불러오자.
const expressHbs = require(’express-handlebars’);
2. Express handlebars라는 엔진이 있다고 알려주자.
이름은 hbs, 어떤 툴을 사용하는지 알려주기 위해 express.Hbs를 입력하자.
app.engine(’hbs’, expressHbs());
3. vies 엔진도 hbs로 변경하자.
app.set(’view engine’, ‘hbs’);
4. views 폴더에 404.hbs 파일을 만들자.
5. 404.html 파일의 코드를 복사하여 404.hbs 파일에 코드를 붙여넣자.
6. class=”active” 는 삭제하자. 지금은 hbs 파일이기에, handlebars에서 사용하지 않기 때문이다.
7. title을 동적 출력으로 만들어보자.
Handlebars에서는 중괄호 두 개를 열고 닫는 방식으로 입력해야한다.
{{ pageTitle }}

## 89. 프로젝트를 핸들바로 변환

app-products.hbs 파일과 shop.hbs 파일도 구축해보자.

title 동적화와 hbs의 불필요한 class 삭제, 이미지 변경 등 404.hbs에서 해보았던 대로 해보자.

 이번엔, 제품에 대해 article을 반복하고, 제품이  없을 때는 No Products Found 텍스트가 나오도록 하는 if-else문도 작성해보자.

 Handlebars는 true나 false를 출력하는 키만 지원한다. 즉, 템플릿의 논리를 Express 코드로 옮겨서 확인하고 이후 결과를 템플릿에 다시 넣어야 한다.

shop.js 파일에 템플릿에 넣을 새로운 키-값 쌍을 추가하자.
hasProducts: products.length > 0

기억하자, Handlebars 템플릿은 논리를 실행할 수 없고 단일 속성/변수/값만 출력할 수 있다. 복잡할 수 있지만, 모든 논리를 Express 코드에 넣기에, 템플릿은 깔끔하다. 템플릿 이해가 수월해질 것이다.

{{#if hasProducts }}

… 코드 …

{{ else }}

{{/if}}

모든 제품을 루핑하는 블록을 추가하자.
{{#each prods }}

…코드…
{{/each}}

```html
<main>
    {{#if hasProducts }}
    <div class="grid">
        {{#each prods}}
        <article class="card product-item">
            <header class="card__header">
                <h1 class="product__title">{{ this.title }}</h1>
            </header>
            <div class="card__image">
                <img src="data:image/jpeg;base64,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"
                    alt="A Book">
            </div>
            <div class="card__content">
                <h2 class="product__price">$19.99</h2>
                <p class="product__description">A very interesting book about so many even more interesting things!</p>
            </div>
            <div class="card__actions">
                <button class="btn">Add to Cart</button>
            </div>
        </article>
        {{/each}}
    </div>
    {{ else }}
    <h1>No Products Found!</h1>
    {{/if}}
</main>
```

## 90. 핸들바에 레이아웃 추가하기

1. app.js에서 handlebars 엔진 등록하기
app.engine(’hbs’, expressHbs({layoutsDir: ‘views/layouts/’, defaultLayout: ‘main-layout’}));
2. view의 layouts에 main-layout.hbs 파일 생성하자.
3. 404.html 코드를 복사하여 main-layout.hbs 파일에 붙여넣자.
4. title은 동적 출력하기 위해 {{pageTitle}}로 작성하자.
5. pug에서는 head 부분에 block styles로 설정했던 것을, hbs에서는 body 부분에 중괄호 3개를 사용하여 정의한다.
{{{ body }}}
6. 스타일링을 추가해야 하는 경우, head 부분에  if 블록을 추가하면 된다.
{{#if formsCSS}}
    <link real=”stylesheet” href=”/css/forms.css”>
{{#if productCSS}}
    <link real=”stylesheet” href=”/css/product.css”>
7. class 추가하자. activeShop의 참/거짓을 확인하고, 참이면 active를 실행하겠금 하면 된다.
class=”{{#if activeShop }}active{{/if}}
add product 라우트에 대해서도 동일하게 진행해주자.
class=”{{#if activeAddProduct }}active{{/if}}
8. shop.js로 가서 activeShop 전달하자. res.render 부분에, 
activeShop: true, productCSS: true
라 작성하자.
9. shop.hbs에서 헤더를 포함한 코드는 레이아웃과 동일하므로 제거하자.
10. app.js에서 전체 레이아웃에 적용되는 확장명을 설정하여 모든 파일에 hbs가 적용되도록 하자.
extname: ‘hbs’
11. add-product.hbs에서 헤더를 포함한 코드는 레이아웃과 동일하므로 제거하자.
12. formsCSS와 productCSS가 참이 되도록 설정하자. admin.js의 res.render 부분에
formsCSS: true, productCSS: true를 작성하자.
더하여, 내비게이션에서 active 링크도 얻어야하므로,
activeAddProduct: true도 작성하자.

## 91. EJS로 작업하기

EJS는 Pug와 같이 즉시 지원되는 템플릿 엔진이기에, Handlebars처럼 따로 엔진을 등록할 필요는 없다. view engine을 ejs로 설정해 주면 된다.

1. 404.html 파일 코드를 복사하여 생성한 404.ejs 파일에 코드를 붙여넣자.
2. title을 작성해보자.
pug는 {# }를 사용하였고, Handlebars는 {{ }}를 사용했었다. EJS는 <%= %>를 사용한다.
<%= pageTitle %>
3. add-product.ejs 파일도 생성하여 작성하고 title도 동적으로 만들어보자.
4. shop.ejs 파일도 생성하여 작성하고 title도 동적으로 만들어보자.
5. shop.ejs 파일의 경우, if 문과 루프가 있으므로 ejs 형식에 맞게 작성해보자.
<% 기호는 사용하지만, 직접 값을 출력하지는 않기에 = 기호는 쓰지 않는다.
일반 JavaScript 코드를 작성하듯 입력하면 된다. 중괄호를 열고 닫는 형식을 사용하여 코드의 범위를 구분한다.
<% if (prods.length > 0) % { >
…코드…
<% } else { %>
    <h1>No Products Found!</h1>
<% } %>
6. 반복하여 데이터를 출력하자.
<% for (let product of prods) { %>
…코드…
<% } %>
7. product title또한 <% product.title %>로 변경하면 된다.

## 92. 부분적인 레이아웃 작업

EJS에는 레이아웃은 없다. 하지만 Partials 또는 Includes 는 사용할 수 있다.
참고로 이는 Pug와 Handlebars도 지원되기에 사용할 수 있다.
일부 코드 블록을 재사용하는 원리이다. 하나의 마스터 레이아웃을 사용하여 각각 view를 입력하는 대신, 공유되는 view 부분을 생성한 view에 통합시키는 것이다. 레이아웃과 정반대 개념인 셈이다.

1. views 폴더에 Includes라는 이름의 새로운 하위 폴더를 생성해보자. 폴더 안에, head.ejs, end.ejs, navigation.ejs를 만들자.
2. 404.ejs의 head 부분 코드를 잘라내어 head.ejs에 붙여넣자.
3. 404.ejs의 head가 있었던 자리에는, head.ejs를 임포트하는 구문을 추가해보자.
    1. 작성할 구문과 = 기호가 있을 때는 HTML 변수를 렌더링 시, 사이트 간 스크립팅 공격을 피하기 위해 HTML 코드가 렌더링 되지 않고 텍스트로 렌더링 된다. 이를 Unescaped라 한다.
    - 기호를 사용하면, Unescaped 현상을 피하여, HTML 코드가 렌더링 된다.
    2. include 키워드는 특정 요소를 이 페이지에 포함할 수 있게 해준다. 포한하고자 하는 파일의 결로가 들어있는 문자열을 추가해주면 된다.
    
    <%- include(’includes/head.ejs’) %>
    
4. 404.ejs의 navigation 부분 코드를 잘라내어, navigation.ejs에 붙여넣자.
5. 404.ejs의 navigation가 있었던 자리에는, navigation.ejs를 임포트하는 구문을 추가해보자.
<%- include(’includes/navigation.ejs’)
6. 404.ejs의 마지막 부분인 닫는 body 태그와 html 코드도 end.ejs 파일로 가져가자.
7. 404.ejs의 마지막 부분에도 end.ejs 파일을 임포트하는 구문을 작성하자.
<%- include(’includes/end.ejs’) %>
8. add-products.ejs와 shop.ejs에도 동일하게 적용해보자.
9. add-products.ejs의 경우, navigation.ejs를 임포트하니 active 클래스가 없어졌다.
navigation.ejs 파일(의 Shop의 li)에 클래스를 추가해야한다.
    1. = 기호를 사용하자.
    2. 정의하는 경로에 해당하는지 확인하자. path === ‘/admin/add-product’
    3. 해당한다면, if문이 맵핑 되어, ? 뒤에 오는 동작을 진행하게 된다.
    4. ‘active’를 실행하게 된다.
    5. 경로에 해당하지 않는다면, else 문이 맵핑 되어, : 뒤에 오는 동작을 진행하게 된다.
    6. ‘ ‘ 를 입력하여, 아무것도 렌더링 되지 않도록 하자.
    7. <%= path === ‘/admin/add-product’ ? ‘active’ : ‘ ‘ %>
    8. navigation.ejs 파일(의 Add Product의 li)에도 클래스를 추가하자.
    9. <%= path === ‘/’ ? ‘active’ : ‘ ‘ %>