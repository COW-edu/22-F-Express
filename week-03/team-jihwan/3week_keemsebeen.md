# section 6

## Managing Data (without database)

- 데이터베이스를 들어가기전, 데이터를 자바스크립트 함수를 통해 저장하는 방법에 대해 설명.
- 데이터가 공유된다 ? 객체/배열같은 참조 유형을 내보내면 다른파일에서 변경 사항이 생겼을 때 저절로 업데이트가 됨.
    - 그닥 유쾌한 상황은 아니다. 불필요한 데이터까지 알게될 수도 있기 때문이다. 그렇기 때문에 특정요청에 대한 데이터만 불러오는 방법을 공부할 것이다. ( node.js 앱의 메모리에 있는 데이터를 공유하는 법에 초점을 맞출 것이다.  )
- 저장하려면 ? 데이터를 저장할 변수 추가를 통해 가능하다.

## Render Dynamic Content in our Views

- Templating Engines : 동적 콘텐츠를 넣기 위해 사용한다.
    - ① HTMLish Template : HTML과 유사한 코드. 일반적으로 코드, HTML을 포함하는 파일, html 구조와 마크업, 스타일, javascript 등 일반적으로모든 요소들을 작성한다. (palceholder에 해당하는 공백도 존재.)
    - ② Node/ Express Content : 더미 배열과 같은 node/express 콘텐츠, html과 유사한 템플릿을 스캔한 뒤 사용하고 있는 엔진 종류에 따라 플레이스 홀더나 특정 스니펫을 실행한다.
    - ③ Templating Engines : 동적 콘텐츠를 넣기 위해 사용한다.
    - ④ Replaces Placeholders / Snippets with HTML content  : 상황에 따라 동적 콘텐츠를 반영하는 템플릿 엔진을 통해 서버에  생성된다.
    - ⑤ HTML File : 동적으로 상황에 따라 생성된 HTML 파일이 사용자에게 전달될 것이다.
        - 따라서 사용자는 placeholder, snippets 등 서버에서 일어나는 일들을 전혀 볼 수 없고 일반적인 html 페이지만을 보게 된다.
- set() : 전체 구성 값 설정
    - set()을 통해 전체 구성 값을 설정해줘야 express 앱 전체에 어떤 값이든 설정이 가능하며 express가 이해할 수 없는 키라던가 구성값 또한 알 수 있다.
    - view의 역할 : Express에게 우리가 렌더링 하려고 하는 동적 Template이 있으며 이를 실행하기 위해 특별한 함수가 있으니 우리가 등록하는 엔진을 사용해달라고 알린다.
        - view는 express에게 이러한 동적 view를 어디에서 찾을 수 있는지 알리는 기능이 있다.
    
    ```jsx
    app.set('view engine', 'pug'); 
    // 내장된 express 지원과 함께 전달된다. express에 스스로를 자동으로 등록함. 
    app.set('views', 'views'); 
    //view를 찾을 지점을 알려줄 수 있다.
    // 이후 views 폴더에 shop.pug 파일 추가. 
    ```
    

## Understanding Templating Engines - Pug

- Pug (Jade) : 실제 html을 사용하지 않고 최소화된 버전 또는 최소 버전으로 대체.  최소 html 버전과 확장이 가능한 일련의 요소나 작업을 제공하는 맞춤형 템플릿 언어를 사용한다.

```jsx
p #{name}
```

- 사용법 및 특징
    
    ```jsx
    header.main-header 
    	nav.main-header__nav    
    // 헤더 요소를 css class와 함께 생성한다. 
    // 헤더에 뭔가를 중첩시키고 싶으면 들여쓰기를 해준 후 코드를 붙여준다. 
    ```
    
    - html 태그를 불러올때 어떠한 요소도 입력하지 않게되면 기본값으로 div를 받는다.
    - 다수의 클래스가 중첩되어 있을 경우 이들을 합쳐서 결합시키고 마침표로 분류한다.
    - 속성이 존재할 때는 괄호 안에 넣어준다.
    - 닫는 태그를 설정하지 않아도 자동으로 추가되기 때문에 신경쓸 필요가 없다.
    
    ```jsx
    each product in prods 
    // 반복되길 원하는 배열(prods)을 입력한다. 
    #{product.title} 
    // prods에 추가하는 단일 결과는 title 키를 지닌 객체이다.
    // admin.js에서 생성된다. title 키를 갖춘 새로운 객체를 푸시한다. 
    ```
    
    - each 키워드를 추가하면 루프를 생성할 수 있다.
        - 값을 추가해준다. 이 값은 반복을 위해 저장되는 값이다.
    - 서버를 재시작할 필요도 없고 nodemon도 작동 x. 템플릿은 서버측 코드에 포함되지 않기 때문이다. (템플릿은 단순히 상황에 따라 선정되는 요소).
        - 따라서 다음에 들어오는 요청을 위해 템플릿을 변경하는 경우 새로운 버전을 자동으로 받아들인다.
    - 그렇기 때문에 뭔가를 옆이 아니라 body 내부에 추가하고 싶다면 들여쓰기를 한 뒤 진행.
- Pug를 사용하면 응답에도 변화가 생긴다. → express.js에서 사용하는 render() 메소드 이용
    
    ```jsx
    router.get('/',(req, res, next) => {
    	const products = adminData.products; 
    	res.render('shop',{prods : products, docTitle: 'Shop'});
    });
    //shpo.pug가 아닌 이유는, pug를 기본 엔진으로 설정했기 때문이다. (자동으로 탐색할거임.)
    ```
    
    - render() : 기본 템플릿 엔진을 사용해서 템플릿을 반환할 것이다.  (등록된 뷰 엔진을 찾음.)
    - adminData에서 결과를 가져옴 → 내 템플렛으로 전달 → 템플릿에 투입 → render()의 두번째 인자에 객체형태로 넣어준다.
    - docTitle을 지정하고 title을 #{docTitle}으로 지정하면 html title의 이름이 ‘Shop’으로 변경된다.
    - prods 키에 존재하는 view로 결과를 전달한다.
    - get은 app객체에서 값을 읽을 수 있다. 앱에서 데이터를 공유하는 방법 중 하나이다.
- 레이아웃에 플레이스홀더나 훅을 정의하여 다른 veiw의 콘텐츠를 유입시킬 수 있다.
    - 뼈대가 될 레이아웃 안에 block을 생성해주고 이를 상속 받은 뒤 block 내부를 채워줌으로써 객체처럼 템플릿을 이용할 수 있다.
    
    ```jsx
    extends layouts/main-layout.pug
    
    block content
    	h1 Page Not Found!
    ```
    

## Understanding Templating Engines - Handlebars

- Handlebars : HTML을 사용하지만 동적 콘텐츠의 경우 ejs와 비슷하게 이중괄호 플레이스 홀더가 존재한다. 일반 html을 사용하지만 일부 사용자 지정 구문도 사용한다.
    
    ```jsx
    <p>{{name}}</p>
    ```
    
- 사용법 및 특징
    - pug, ejs와는 달리 즉시 사용이 불가능하다. express-handlebars를 불러오는 과정이 필요하다.
    - engine()을 사용해서 등록되어 있지 않은 새로운 템플릿 엔진을 등록한다.
        - 첫번째 인자로는 엔진의 이름을 정의해주고, 두번째 인자로는 실제로 무슨 도구를 사용하는지 알려줘야 된다.
    
    ```jsx
    const expressHbs = require('express-handlebars');
    
    app.engine('handlebars', expressHbs()); 
    // 엔진의 이름을 handlebars라고 정의해둔 상태이다. 
    //expressHbs()를 불러옴으로써 호출할 수 있는 함수로 바뀐다. 
    app.set('view engine', 'handlebars'); 
    // 앞서 설정한 이름과 동일한 엔진 이름을 입력.
    ```
    
    - if block문 : 단순 텍스트 출력이 아닌 조건부로 출력하거나 루프에 있는 내용을 포함한다.
        - if helper 조건부 렌더링 : 해시태그 #를 붙여서 사용하고, 사용이 끝났으면 if문 앞에 ‘/’ 를 붙여준다.  (else의 경우는 해시태그를 떼고 써줘야함. )
        - handlebars는 조건문으로 문장을 받지 않고 , true/false를 출력하는 키만 받는다. (+ 논리를 실핼할 수 없고 단일변수나 단일 속성 등 해당 값만 출력이 가능하다.)
    - each block 문
    
    ```jsx
    {{#if prods.length >0}}
    	{{#each prods}}
    		<article>
    		...
    		</article>
    	{{/each}}
    {{else}}
    	...
    {{/if}}
    ```
    
    - 템플릿에 논리를 많이 두지 않고 express코드에 논리를 많이 두는게 원칙이다. exrpess코드에 모두 넣어서 템플릿에서 자바스크립트 표현식을 가지지 않도록한다.
- 레이아웃 사용
    - engine에 몇가지 옵션을 전달해준다.
    - layoutsDir : 레이아웃들이 어디에 위치할지, 레이아웃을 찾을 수 있는 폴더를 설정할 수 있게 해준다.
    - defaultLayout : 모든 파일에 적용되는 기본 레이아웃을 정의해 줄 수 있다.
    - {{{ }}} : 중괄호를 3개를 사용해서 플레이스홀더를 정의해줄 수 있다.
    - extname을 hbs로 설정해줌으로써 전체파일이 아닌 레이아웃에만 적용이 되는 확장명을 설정해서 레이아웃을 제외한 모든 파일에 적용이 가능하게 해준다.
    
    ```jsx
    app.engine('hbs', 
        expressHbs({
    	      layoutDir: 'views/layouts/',
            dafaultLayout :'main-layout.hbs',
            extname: 'hbs'
        }));
    // 레이아웃이 views 폴더의 하위폴더인 layouts 폴더에 설정.
    // default로 사용되는 layout은 main-layout.
    // 확장명은 hbs임. hbs로 저장된 모든 파일은 기본적으로 main-layout을 지니고 있음.
    ```
    

## Understanding Templating Engines - EJS

- EJS : 일반 html을 사용하고 단순한 javascript를 사용할 수 있게 하는 플레이스 홀더가 있다.
    
    ```jsx
    <p><% = name %></p>
    ```
    
- 사용법 및 특징
    - pug에서와 같이 view engine 값을 ejs로 넣어줌으로써 바로 사용이 가능하다.
    - handlebars에서는 사용하지 못하던 논리적 연산이 가능하다.
    - 레이아웃을 사용할 수 없다는 단점이 존재한다.
    - 일반적인 자바스크립트 코드 사용이 가능하다.
    
    ```jsx
    <% %> 을 사용. 일정 코드 블럭을 둘러싼다. 조건문에서 사용. 
    <%= &> 플레이스 홀더에 두고싶으면 =을 추가해서 사용. 직접 값을 대입. 
    ```
    
- 하나의 마스터 레이아웃을 사용해서 각각의 view부분을 입력하는 대신 공유된 view 부분들을 생성하고자 하는 view들에 통합 시킬 수 있다. ( 일부 코드 블록을 탬플릿 내부의 다양한 부분들에서 재사용함으로써 탬플릿에서 일부 코드들을 공유.)
    - view 폴더에 하위 폴더로 includes를 만들어 준 뒤, 해당 폴더에서 작업을 진행한다.
    - includes 폴더에 재사용 할 head.ejs, end.js, navigation.ejs를 생성해준다.
    - unescaped HTML 코드 : 만약 해당 구문과 = 기호가 존재할때 html 코드를 가진 문자열에 해당하는 변수를 렌더링하는 경우  사이트 간 스크립팅 공격을 피하기 위해 해당 html코드가 렌더링 되지않으며 대신 텍스트로 렌더링 된다는 의미이다.
        - ‘ -’ 를 붙이면 스크립팅 공격을 피해 html 코드를 렌더링할 수 있다.
        
        ```jsx
        <%- include() %>
        ```
        
    - include 키워드 : 특정 요소를 현재 페이지에 포함할 수 있게 해준다.
        - include 안에 인자로 포함하고자하는 파일의 경로를 넣어준다.