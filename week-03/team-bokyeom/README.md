# TEAM 보겸 - 3주차 RBF
- 전체 RBF
    
    ## 섹션 6 - 동적 콘텐츠 작업 및 템플릿 엔진 추가.
    
    # 모듈 소개
    
    - Managing Data (without a DB)
    - 동적 컨텐츠 렌더링
    - Templating Engines 이해
    
    # 요청 및 사용자 간의 데이터 공유
    
    하나의 파일에서 배열을 만든 후에 POST 요청시 배열에 데이터 저장.
    
    이후 다른 파일로 export 하여 받아낼 수 있음.
    
    router에 배열을 push.
    
    req.body.title로 title 추출
    
    모든 제품을 출력하는 shop.js파일에서 products 접근
    const adminData = require(’./admin’);
    
    데이터를 console.log()로 보면 잘 보임.
    
    그런데 다른 브라우저에서 이 주소를 불러오면 원래 브라우저에서 입력해두었던 데이터가 그대로 남아있다. 
    
    이러한 경우는 다른 사용자들의 데이터가 남에 의해 만들어질 수 있으므로 피하는 것이 좋다.
    
    ```jsx
    const products = [];
    
    router.post('/add-product', (req, res, next) => {
      products.push({ title: req.body.title });
      res.redirect('/');
    });
    
    exports.routes = router;
    exports.products = products;
    ```
    
    # 템플릿 엔진
    
    템플릿이란? 
    
    → 맞춤제작하기 쉽도록 미리 만들어 놓은 웹 페이지를 의미한다. 일정한 틀, 형식을 의미한다.
    
    ### Templating Engines
    
    HTMLish Template
    
    Node/Express Content + Templating Engine
    
    →  실제 HTML 로 교체.
    
    사용 가능한 템플릿 엔진
    
    - EJS - HTML 형식
        - 일반 HTML & JS
    - PUG - 최소화된 버전 활용
        - 최소화된, 커스텀.
    - Handlebars - 이중 중괄호
        - 일반 HTML + 커스텀
        
    
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
    
    # Pug 설치 및 구현
    
    프로덕션 의존성으로 다운.
    
    1. 템플릿 엔진 설치
    npm install —save ejs pug express-handlebars
    2. pug 사용을  Express.js에게 알리기
    app.set(’view engine’, ‘pug’);
    
    ### app.set()
    
    우리는 express에게 렌더링 하려고 하는 동적 템플릿이 있으며 이를 실시하기 위해서 이런 템플릿 엔진이 있고 템플릿은 어디있는지 express에 알려야 한다 (node로 진행하면 이거 다 수동)
    
    이 함수를 통해 알려줘야한다
    
    ```jsx
    app.set('view engine', 'pug');
    app.set('views', 'views'); //위치 설명
    //app.set("views","./views");
    ```
    
    첫번째 코드는 pug를 템플릿 엔진으로 사용할거라는 의미이고
    
    두번째 코드는 views 파일이 모여있는 폴더를 지정하는거다 우리의 템플릿들이 저 폴더에 있다
    
    실제 views 폴더가 하위폴더였으면 두번째 폴더처럼 적었을 수도 있다.
    
    1. pug 파일 생성 및 코드 작성
    2. 응답 render()
    
    ### render()
    
    이 렌더함수는 뷰를 렌더링하는데 사용되며, 기본 템플릿 엔진을 사용하여서 렌더링된 html 문자열을 클라이언트에 보낸다.
    
    ```jsx
    res.render('shop');//shop.pug 불러옴
    ```
    
    ❓ res.sendFile()과 res.render()의 차이는?
    
    sendFile은 어떤 파일을 그대로 보내고 싶을 때 쓰고 render 파일을 보내기전에 ejs파일,pug파일 이런 파일을 html을 바꾸고 싶을 때 사용하는 것이다.
    
    브라우저는 ejs 파일 pug 파일을 모르고 html만 알기 때문이다.
    
    ### pug 문법
    
    html 의 태그 이름만 쓰고, 닫는 태그 또한 쓰지 않는다.
    
    하위 태그의 구분은 들여쓰기로 진행.
    
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
    
    render 메서드에 키 값으로 데이터 전달 가능.
    
    ```jsx
    #{value}
    ```
    
    중괄호 안에 원하는 값 넣어서 전달 가능함.
    
    예를 들어,
    res.render(’shop’, {prods: products, docTitle: ‘Shop’});
    의 경우
    
    ‘Shop’ 출력하고 싶은 부분에 #{docTitle}
    
    ### pug의 반복문
    
    중에서 each in을 사용해서 값을 가져올 수 있다.
    
    [https://inpa.tistory.com/entry/PUG-📚-제어문-조건문-반복문](https://inpa.tistory.com/entry/PUG-%F0%9F%93%9A-%EC%A0%9C%EC%96%B4%EB%AC%B8-%EC%A1%B0%EA%B1%B4%EB%AC%B8-%EB%B0%98%EB%B3%B5%EB%AC%B8)
    
    products는 상품으로 보고
    
    prods는 상품 나열
    
    루프 배열이 prods 이므로, each product in prods
    
    # Pug 레이아웃 기능
    
    ### 레이아웃 왜 사용해?
    
    우리가 html을 계속 반복적으로 쓰는 구간이 있다 이것은 굉장히 성가신 일 !
    
    우리는 레이아웃을 만들어서  block으로 다른 템플릿을 상속 받을 수 있음
    
    block으로 다른 코드에서 가져와다가 그 부분에 넣는 느낌이라고 생각하면 좋음
    
    ⇒ 레이아웃을 사용하게 되면서 훨씬 간결한 전근법이 되었다. 이제, 수정할 때 모든 파일을 다 변경하는 것이 아닌, 레이아웃 파일만 변경해주면 된다. extend가 연결된 파일들은 자동으로 반영해준다.
    
    **즉 extends 키워드로 템플릿을 상속 받고 block 키워드로 원하는 데이터만 쏙 넣어줄 수 있다**
    
    extends에 폴더명 파일명 적고  block이 코드를 담고 있다.
    
    extends layout.pug를 통해서 부모 템플릿을 상속 받으면 레이아웃.pug의 해당 block에 body.pug에서 설정한 block이 입력되서 html이 완성되는 식이다
    
    동적으로 추가도 가능
    
    Include 기능을 사용하여 한 pug 파일의 내용을 다른 pug 파일에 삽입할 수도 있다
    
    하지만 include를 사용하더라도 중복되는 코드가 많이 발생하고 include 조차 중복해서 작성해줘야하는 불편함 이때 사용하면 좋은 키워드가 extends와 block 키워드다. layout 하나의 파일에 작성해 extends 키워드를 사용해 다른 파일에서 가져다 쓸 수 있따
    
    `include와 extends의 차이점`은 extends는 include처럼 단순히 가져다 쓰는 것이 아니라 내부 코드에 block 키워드를 이용해서 각 파일별로 필요한 코드들을 원하는 위치에 추가하는 식으로 확장
    
    상속해서 사용 !!
    
    [https://on1ystar.github.io/node.js/2021/05/05/Node-7/](https://on1ystar.github.io/node.js/2021/05/05/Node-7/)
    
    # 핸들바 작업
    
    사용하기 위해서는 뷰 엔진을 변경해야함.
    
    익스프레스에 자동으로 다운되는 것이 아님. 
    
    논리 실행 x
    
    **handlbars 장점**
    
    handlebars는 다른 템플릿 엔진에 비해 상대적으로 html을 거의 훼손하지 않았고 서버 사이드 뿐만 아니라 클라이언트 사이드에서도 단독으로 동작할 수 있다는 장점이 있다
    
    [https://www.hanumoka.net/2018/11/28/node-20181128-node-express-handlerbars/](https://www.hanumoka.net/2018/11/28/node-20181128-node-express-handlerbars/)
    
    먼저 handlebars는 pug와 달리 express의 built in 엔진이 아니여서 engine 설정을 해주어야 한다
    
    1. 불러오기. 
    const expressHbs = require(’express-handlebars’);
    2. Express handlebars라는 엔진이 있다고 알려주자.
    이름은 hbs로, 어떤 툴을 사용하는지 알려주기 위해 express.Hbs를 입력하자.
    
    ### app.engine()
    
    등록되지 않은 새로운 템플릿 엔진 등록
    
    ```jsx
    app.engine('hbs', expressHbs());//엔진 초기 설정
    ```
    
    1. vies 엔진도 hbs로 변경하자.
    app.set(’view engine’, ‘hbs’);
    2. hbs 파일 만들고 작성하자.
    3. title 동적 출력하기. Handlebars는 중괄호 두 개를 열고 닫는 방식이다.
    {{ pageTitle }}
    
    ### 동적출력
    
    키: 값 방식은 동일. 
    
    ```jsx
    {{ value }}
    ```
    
    중괄호 두개 사이에 전달해준 값으로 렌더링. 
    
    ## IF문에 헬퍼 함수 사용
    
    handlebars는  `{{#if a==3}}`과 같은 if문을 제공하지 않는다
    
    if 자체가 헬퍼 메서드로 등록되어 있는데 if 헬퍼 함수에 전달하는 값을 true/false 여부만 판단할 뿐이고 일반적인 자바 스크립트 처럼 저런식의 표현은 제공하지 않는다
    
    그래서 템플릿의 논리를 Express 코드로 옮겨서 확인하고, 객체에서 논리를 따질 수 있는 걸 키 값 매핑해서, 결과를 템플릿에 다시 넣어 전달해야한다.
    
    - defaultLayout: 기본 값으로 **main** 으로 되어 있으며, 설정된 값에 따라 main layout의 파일명을 설정 할 수 있다.
    - extname: 확장자를 설정할 수 있습니다. 기본은 handlebars 이다.
    - partialsDir: **partial** 이란 레이아웃을 채울 파일들을 의미하며, **partial** 의 위치를 설정 할 수 있습니다. 기본은 partials 이다.
    - layoutsDir: **layout** 파일 위치를 지정 합니다. 기본 값은 layouts 이다.
    
    ⇒ 기억하자, Handlebars 템플릿은 논리를 실행할 수 없고 단일 속성/변수/값만 출력할 수 있다. 복잡할 수 있지만, 모든 논리를 Express 코드에 넣기에, 템플릿은 깔끔하다. 템플릿 이해가 수월해질 것이다.
    
    ## 핸들바에 레이아웃 추가하기
    
    1. app.js에서 handlebars 엔진 등록하기
    app.engine(’hbs’, expressHbs({layoutsDir: ‘views/layouts/’, defaultLayout: ‘main-layout’}));
    2. view>layouts>main-layout.hbs 파일 생성 및 코드 작성
    3. title 동적 출력 {{pageTitle}}
    4. 스타일링은 if 블록
    {{#if formsCSS}}
        <link real=”stylesheet” href=”/css/forms.css”>
    {{#if productCSS}}
        <link real=”stylesheet” href=”/css/product.css”>
    5. class 추가하자. activeShop의 참/거짓을 확인하고, 참이면 active를 실행하겠금 하면 된다.
    class=”{{#if activeShop }}active{{/if}}
    add product 라우트에 대해서도 동일하게 진행해주자.
    class=”{{#if activeAddProduct }}active{{/if}}
    6. shop.js로 가서 activeShop 전달하자. res.render 부분에, 
    activeShop: true, productCSS: true
    라 작성하자.
    7. 모든 파일에 hbs 적용
    extname: ‘hbs’
    8. formsCSS와 productCSS가 참이 되도록 설정하자. admin.js의 res.render 부분에
    formsCSS: true, productCSS: true를 작성하자.
    더하여, 내비게이션에서 active 링크도 얻어야하므로,
    activeAddProduct: true도 작성하자.
    
    # EJS 작업
    
    ### ejs란?
    
    - embedded javascript의 약자로 자바스크립트가 내장되어 있는 html 파일이다
    - node.js에서 많이 사용하는 템플릿 엔진으로 ejs는 html 안에서  <% %>를 이용해서 서버의 데이터를 사용하거나 코드를 실행할 수 있다
    - html 태그처럼 자바스크립트 내용을 삽입할 수 있다.매우 큰 강점 -!!
    
    ### ejs의 특징
    
    pug의 확장된 기능을 갖추고 있음.
    
    익스프레스에 내장된 엔진. 
    
    일반 HTML 활용.
    
    1. EJS는 Pug와 같이 즉시 지원되는 템플릿 엔진이기에, Handlebars처럼 따로 엔진을 등록할 필요는 없다. view engine을 ejs로 설정해 주면 된다.
    2. ejs 파일을 생성하고 코드를 작성하자.
    3. title을 작성해보자.
    pug는 {# }를 사용하였고, Handlebars는 {{ }}를 사용했었다. EJS는 <%= %>를 사용한다.
    <%= pageTitle %>
    4. shop.ejs 파일의 경우, if 문과 루프가 있으므로 ejs 형식에 맞게 작성해보자.
    <% 기호는 사용하지만, 직접 값을 출력하지는 않기에 = 기호는 쓰지 않는다.
    일반 JavaScript 코드를 작성하듯 입력하면 된다. 중괄호를 열고 닫는 형식을 사용하여 코드의 범위를 구분한다.
    <% if (prods.length > 0) % { >
    …코드…
    <% } else { %>
        <h1>No Products Found!</h1>
    <% } %>
    5. 반복하여 데이터를 출력하자.
    <% for (let product of prods) { %>
    …코드…
    <% } %>
    
    ## 부분적인 레이아웃 작업
    
    EJS에는 레이아웃은 없다. 하지만 Partials 또는 Includes 는 사용할 수 있다. 참고로 이는 Pug와 Handlebars도 지원되기에 사용할 수 있다.
    
    1. views>Includes>head.ejs, end.ejs, navigation.ejs 생성하고 코드 작성
    2. 임포트하자.
        1. 작성할 구문과 = 기호가 있을 때는 HTML 변수를 렌더링 시, 사이트 간 스크립팅 공격을 피하기 위해 HTML 코드가 렌더링 되지 않고 텍스트로 렌더링 된다. 이를 Unescaped라 한다.
        - 기호를 사용하면, Unescaped 현상을 피하여, HTML 코드가 렌더링 된다.
        2. include 키워드는 특정 요소를 이 페이지에 포함할 수 있게 해준다. 포한하고자 하는 파일의 경로가 들어있는 문자열을 추가해주면 된다.
        
        <%- include(’includes/head.ejs’) %>
        
        <%- include(’includes/navigation.ejs’ >
        
        <%- include(’includes/end.ejs’) %>
        
    
    ### 문법
    
    - <% %>
    
    이 태그는 자바스크립트를 실행할 수 있다
    
    - <%= %>
    
    이 태그는 변수 값을 내장시킬 수 있다
    
    - Include?
    
    <% -include(’경로입력’%> 페이지 내 반복되는 header나 footer등의 코드를 includes 폴더안에 작업하고 include를 이용해서 파일로 임포트하면 간편하게 레이아웃을 작업할 수 있다(ejs는 따로 제공 레이아웃 기능이 없어서  일부 코드 블록을 템플릿 내부의 다양한 부분들에서 재사용함으로써 템플릿에서 이들을 공유)
    
    ```jsx
    <%= value %> // 변수 값 저장
    
    <% JS %> // JS 코드 사용 가능.
    ```