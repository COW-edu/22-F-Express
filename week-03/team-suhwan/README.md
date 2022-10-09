# TEAM 수환 - 3주차 RBF


# 템플릿 엔진
## 정의

- html 페이지에 동적 콘텐츠를 넣기 위해 사용
- 템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 sw | sw Component
- HTML 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 데이터만 템플릿 특정 장소에 끼워 넣는 방식으로 동작할 수 있도록 해줌.

## 작동 방법

1.  
    - HTML template
        
        : 마크업, 스타일, HTML 기본 구조, placeholder에 해당하는 공백, JS 등 일반적으로 html 파일에 포함된 모든 요소가 담김
        
    - node/express content
        
        : DB 혹은 API에서 가져온 데이터
        
2.  
    - 템플릿 엔진
        
        : HTML과 유사한 템플릿(HTML template)을 스캔한 뒤, 템플릿엔진 종류에따라 실제 html로 교체→html content는 동적 콘탠츠를 반영하는 템플릿엔진을 통해 서버에서 생성
        
3.  
    - 동적으로 생성된 html파일이 사용자에게 전달

## Template Engine 종류

### EJS

- `<p><%= name %></p>`
- template에 js코드를 쓸 수 있는 유연성
- include( )를 통해 특정 요소를 이 페이지에 포함할 수 있게 한다
- Unescaped HTML코드를 출력할 때 사용

<aside>
💡 Unescaped
: HTML코드를 가진 문자열에 해당하는 변수를 렌더링하는 경우, 사이트 간 스크립트 공격을 피하기 위해 해당 코드가 아닌 텍스트로 렌더링되는 것

</aside>

- 예시 코드

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

### Pug

- `p #{name}`
- html에서 닫는 태그가 없음 (</head>, </body>, </title> 등) 
→ 들여쓰기 이후 공백까지 태그
- tag의 속성으로 넣으려면 ( )괄호 사용
→ `button.btn(type="submit")Add Product`
- 일반적으로 일련의 요소나 작업 종류만을 제공하는 맞춤형 템플릿 언어를 사용하지만 if, else문 사용도 가능
- 각 파일마다 동일한 코드를 수동으로 작성하기 번거로움 → layout을 이용
    - extends를 통해 레이아웃을 지정해줌
    - block content 를 이용해 고유의 커스텀 콘텐츠를 작성할 수 있음
    
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
    

### Handlebars

- `<p>{{ name }}</p**>**`
- Handlebars 템플릿은 논리를 실행 불가, 단일 속성 혹은 변수 값만 출력 가능
- 일반 html을 사용하는 동시에 제한된 기능의 맞춤형 템플릿 언어도 사용하고, if 문이나 목록 등의 공통 요소들을 포함함 → 원칙은 Express코드에 논리를 많이 두는 것
- 모든 논리를 Express 코드로 넣도록 만들어 템플릿을 깔끔하게 유지됨  → if 안에 true/false 만 들어갈 수 있음

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
    {{/if}}</main>
```

---

### 설치 방법

- express 앱을 저장한 변수 ex)app 에서 `set()` 사용
    - `set()` 을 통해 Express 애플리케이션 전체에 어떤 값이든 설정 가능
    - `get()` 을 통해 값을 가져올 수 있음

```jsx
app.set("view engine", "pug");
//view engine 으로 pug를 등록해 동적 template을 렌더링한다는 의미
```

### data 전달 방법

- render()
    - 우리의 view에 추가되어야 하는 데이터를 전달할 수 있게 함
    - js 객체의 형태로 전달함
        - 객체를 키 이름에 맵핑한 뒤 템플릿 내부에서 전달하는 데이터를 참조하기 위해 사용함

---

## 분류

- web template engine은 view code(HTML)과 data logic code (DB connection)을 분리
- 서버 사이드 템플릿 엔진과 클라이언트 사이드 템플릿 엔진으로 분류
    - Server Side Template Engine : 서버에서 DB | API에서 가져온 데이터를 미리 정의 된 template에 넣어 html그려 client에게 전달
    - Client Side Template Engine : HTML 형태로 코드를 작성할 수 있고, data를 받아서 동적으로 DOM 객체에 그려주는 process담당

# CSR & SSR

## ****CSR(Client Side Rendering)****

- 서버에서 제공받은 data를 client에서 rendering

### 장단점

- 장점
    - 빠른 속도 및 서버 부하 감소 → 필요한 부분에 대한 data만 rendering
    - 사용자 친화적 UX → contents 전환과정에서 link가 없기때문에 깜빡거림이 없음
    - 사용자와 interaction이 많은 동적 컨텐츠에 유리
- 단점
    - 초기 로딩 시간 → 모든 script 파일 load를 기다려야 함
    - SEO(search engine optimization, 검색 엔진 최적화) → html file에 content가 존재하지 않기때문에 crawling할 수 있는 내용이 없어 SEO에 취약

## SSR(Server Side Rendering)

- 서버에서 data를 rendering하여 client에게 완전한 HTML 형태를 만들어서 전달

### 장단점

- 장점
    - SEO(search engine optimization, 검색 엔진 최적화) → 완성된 형태의 html file을 서버로부터 전달받기 때문에  검색엔진의 page 크롤링에 적합
    - 초기 로딩 시간 → 클라이언트가 JS 파일을 모두 다운 및 적용 전 까지 기능 동작하지 않지만 user가 빠르다고 느낌
- 단점
    - UX 불편 → 페이지를 요청하는 경우, 새로 리랜더링
    - 페이지 요청이 들어오는 경우 불필요한 ****템플릿도 중복해서 로딩→ 부하로 인한 성능 저하
    - 개발 복잡도 → 모바일 앱 개발시 추가적인 백엔드 작업이 요구되어 생산성이 낮음