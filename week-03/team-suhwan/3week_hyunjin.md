- html 페이지에 동적 콘텐츠를 넣기 위해 템플릿 엔진을 사용할 것

### 템플릿 엔진

- HTML과 유사한 템플릿이 있음. 여기에 일반적으로 코드, 많은 HTML을 포함하는 파일, HTML 구조와 마크업, 스타일, 불러온 JS 등 일반적으로 포함된 모든 요소들을 작성.
- **웹사이트 화면을 어떤 형태로 만들 지 도와주는 양식이라고 생각하면 됨**
- Node/Express content, 우리가 현재 사용하고 있는 제품 층이 있고 템플릿 엔진도 있음.
→특정 구문을 이해하며 이를 위해 html과 유사한 템플릿을 스캔한 뒤 사용하고 있는 엔진 종류에 따라 플레이스홀더나 특정 스니펫을 실제 html 콘텐츠로 교체함. 
→ 콘텐츠는 상황에 따라 해당 동적 콘텐츠를 반영하는 템플릿 엔진을 통해 서버에 생성됨
    
    → 마지막 결과: 동적으로 상황에 따라 생성된 html 파일이 되며 다시 사용자들에게 전달됨. 사용자들은 템플릿, 플레이스홀더, 서버에서 일어나는 일을 전혀 볼 수 없고 일반적인 html 페이지만을 보게 됨
    

### 서버 사이드 렌더링, 클라이언트 사이드 렌더링

- 서버사이드 렌더링(SSR): 서버에서 데이터를 렌더링 → 완전한 HTML 형태를 만들어서 전달
    - 서버를 이용해 페이지를 구성하기 때문에 CSR보다 페이지 구성 속도는 느리지만 전체적으로 사용자에 보여주는 컨텐츠 구성이 완료되는 시점이 빨라짐
    - 동적 컨텐츠 렌더링이 반복되는 경우 사용
    
- 클라이언트 사이드 렌더링(CSR): 서버로부터 제공받은 데이터를 클라이언트에서 렌더링 ~> HTML 화면을 동적으로 만듬
    - 사용자와 많은 상호 작용이 필요할 때 사용

![Untitled](https://t1.daumcdn.net/cfile/tistory/2527A54557AD0BAE2D)

### 사용할 수 있는 템플릿 엔진

- EJS
    
    <p><%= name %></p>
    
    - 일반 HTML을 사용하고 단순한 JS를 사용할 수 있게 하는 플레이스 홀드가 있어 for 루프를 위해 if문도 작성할 수 있음
- Pug
    
    p #{name}
    
    - 일반적으로 일련의 요소나 작업 종류만을 제공하는 맞춤형 템플릿 언어를 사용하지만 if문들과 목록도 포함될 수 있고 반복문도 포함될 수 있음.
- Handlebars
    
    <p>{{name}}</p>
    
    - 일반 html을 사용하는 동시에 제한된 기능의 맞춤형 템플릿 언어도 사용하고, if 문이나 목록 등의 공통 요소들을 포함함

- render()
    - 우리의 view에 추가되어야 하는 데이터를 전달할 수 있게 함
    - js 객체의 형태로 전달함
        - 객체를 키 이름에 맵핑한 뒤 템플릿 내부에서 전달하는 데이터를 참조하기 위해 사용함

### Pug

- 각 파일마다 동일한 코드를 수동으로 작성하기 번거로움 → layout을 이용
    - extends를 통해 레이아웃을 지정해줌
    - block content 를 이용해 고유의 커스텀 콘텐츠를 작성할 수 있음
    - 레이아웃 파일 중
    
    ```jsx
    ul.main-header__item-list
                        li.main-header__item 
                            a(href="/") Shop     
                        li.main-header__item 
                            a(href="/admin/add-product") Add Product
    ```
    
    → 어느 페이지에 있는 지에 따라 올바른 링크에 활성화 클래스를 작성해주어야 함
    
- shop.pug 코드

```jsx
extends layouts/main-layout.pug 

block styles
    link(rel="stylesheet", href="/css/product.css")

block content
    main 
        if prods.length>0
            .grid 
                each product in prods
                    article.card.product-item
                        header.card__header 
                            h1.product__title #{product.title}
                        .card__image 
                            img(src="https://play-lh.googleusercontent.com/_tslXR7zUXgzpiZI9t70ywHqWAxwMi8LLSfx8Ab4Mq4NUTHMjFNxVMwTM1G0Q-XNU80", alt="A book")
                        .card__content 
                            h2.product__price $19.99
                            p.product__description A very interesting book about cook.
                        .card__actions 
                            button.btn Add to Cart
        else 
            h1 No Products!
```

→ 간단한 html 형태

→ if, else문 사용

### Handlebars

- app.engine() : 내장되어 있지 않은 새로운 템플릿 엔진을 등록
    - Pug는 내장된 엔진이었지만 express handlebars는 내장되어있지 않음
    
    ```jsx
    const expressHbs = require('express-handlebars');
    app.engine('handlebars',expressHbs());
    ```
    
    → 함수가 초기 설정된 뷰 엔진을 반환해 engine에 넣는 것
    
- shop.hbs 코드 → {{}} 형태
    
    ```jsx
    <main>
        {{#if hasProducts}}
            <div class="grid">
                {{#each prods}}
                <article class="card product-item">
                    <header class="card__header"> 
                        <h1 class="product__title">{{this.title}}</h1>
                    </header>
                    <div class="card__image"> 
                        <img src="https://play-lh.googleusercontent.com/_tslXR7zUXgzpiZI9t70ywHqWAxwMi8LLSfx8Ab4Mq4NUTHMjFNxVMwTM1G0Q-XNU80", alt="A book">
                    </div>
                    <div class="card__content"> 
                        <h2 class="product__price">$19.99</h2> 
                        <p class="product__description">A very interesting book about cook.</p>
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
    
- if문

```jsx
{{#if }}
{{/if }}
```

→ if 안에 true/false 만 들어갈 수 있음

- 모든 논리를 express 코드로 넣도록 만들어 템플릿을 깔끔하게 유지

<aside>
💡 express 코드에 논리를 많이 넣어두어서 템플렛이 js 코드를 가지지 않도록 하는 것이 원칙

</aside>

### ejs

```jsx
<%- include() %>
```

- ‘-’ Unescaped HTML 코드를 출력할 때 사용하는 기호
    - Unescaped: 만약 이 구문과 = 기호가 있을 때 HTML 코드를 가진 문자열에 해당하는 변수를 렌더링하는 경우 사이트 간 스크립팅 공격을 피하기 위해 해당 HTML코드가 렌더링되지 않으며 대신 텍스트로 렌더링 됨
    - ‘-’ 를 붙이면 이 현상을 피해 HTML 코드를 렌더링 할 수 있음
- include() : 특정 요소를 이 페이지에 포함할 수 있게 함
- shop.ejs 코드
    
    ```jsx
    <%- include('includes/head.ejs') %>
        <link rel="stylesheet" href="/css/product.css">
    </head>
    <body>
        <%- include('includes/nav.ejs') %>
        <main>
            <% if (prods.length>0) { %>
                <div class="grid">
                    <% for(let product of prods) { %>
                        <article class="card product-item">
                            <header class="card__header"> 
                                <h1 class="product__title"><%= product.title %></h1>
                            </header>
                            <div class="card__image"> 
                                <img src="https://play-lh.googleusercontent.com/_tslXR7zUXgzpiZI9t70ywHqWAxwMi8LLSfx8Ab4Mq4NUTHMjFNxVMwTM1G0Q-XNU80", alt="A book">
                            </div>
                            <div class="card__content"> 
                                <h2 class="product__price">$19.99</h2> 
                                <p class="product__description">A very interesting book about cook.</p>
                            </div>
                            <div class="card__actions">
                                <button class="btn">Add to Cart</button> 
                            </div> 
                        </article>
                    <% } %>
                </div>
            <% } else{ %>
                <h1>No Products Found!</h1>
            <% } %>
        </main>
    <%- include('includes/end.ejs') %>
    ```
    

- <% 빨간색으로 뜨는 오류

 → ejs language support 확장 설치

- 404 페이지 제대로 뜨지 않는 오류

```jsx
app.use((req,res,next)=>{
    res.status(404).render('404',{pageTitle:'Page Not Found'});
})
```

→ app.js 파일 내부

```jsx
app.use((req,res,next)=>{
    res.status(404).render('404',{pageTitle:'Page Not Found', path: 'Error'});
})
```

→ path: ‘Error’ 추가

→ 경로가 정의되어 있지 않아서 nav.ejs에서 경로 값을 찾을 수 없기 때문에 이를 해결하려면 렌더 기능에 추가해야 함.