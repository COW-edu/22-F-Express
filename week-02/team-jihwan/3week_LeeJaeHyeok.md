# 동적 콘텐츠 및 테플릿 엔진

# 요청 및 사용자 간의 데이터 공유

```jsx
const products = [];

.
.
.

//module.exports = router;
exports.routes = router;
exports.products = products;
```

상수 이기는 하지만 요소를 추가하거나 제거해도 전체적인 객체에는 영향이 없다.

배열 자체는 같은 객체이기 때문에 새로운 요소를 받을 수 있다.

배열을 사용해서 데이터를 저장하면 실행 중인 노드 서버에 내재된 데이터에서 사용자 간에 공유된다. 개인정보 일 수 있으므로 다른 사용자에 데이터를 보여주면 안된다

데이터는 한 사용자의 여러 요청 간에 공유되는 것이지 요청 및 사용자 간에 공유되지는 않는다

# 템플릿 엔진(Templating Engines)

HTML의 **정적인 단점을 개선**

- 반복문, 조건문, 변수 등을 사용할 수 있다
- 동적인 페이지 작성 가능
- PHP, JSP와 유사

**BUT @**, 사용성이 매우 줄어들었다.

React, Vue, Angular 들이 쓰이고 있기때문에 사용성이 줄어들었다.

그러나, 같이 쓰는 경우가 있다.

(예시: React와 Pug를 같이 쓰는 경우가 있다)

### 템플릿 엔진의 작동

HTML과 유사한 템플릿 - 일반적으로 코드, 많은 HTML을 포함하는 파일, HTML 구조와 마크업, 스타일 그리고 불러온 JavaScript 등 일반적으로 포함된 모든 요소들을 작성하는데 플레이스홀더에 해당하는 공백도 일부 있다.

앱에 더미 배열과 같은 Node/Express 컨텐츠, 우리가 사용 중인 제품 층이 있고, 템플릿 엔진도 있다.

이는 특정 구문을 이해하며 이를 위해 HTML과 유사한 템플릿을 스캔한 뒤 사용하고 있는 엔진 종류에 따라 플레이스홀더나 특정 스니펫을 실제 HTML 컨텐츠로 교체한다. 

이 HTML 컨텐츠는 상황에 따라 해당 동적 컨텐츠를 반영하는 템플릿 엔진을 통해 서버에 생성된다.

예를들어, 템플릿 엔진의 도움을 통해 Node/Express 앱에 있는 데이터에 대한 목록 항목들이 포함된 순서 없는 목록을 출력할 수 있다. 

마지막에는 HTML 파일이 될 것이며 다시 클라이언트에게 전달될 것 이다.

### 템플릿 엔진의 종류

- Pug(Jade)
    - 실제 HTML을 사용하지 않고 최소화된 버전 내지는 최소 버전으로 대체하며 동적 컨텐츠와 함께 출력하게 한다
    - 최소 HTML 버전과 확장 가능하지만 일반적으로 일련의 요소나 작업 종류만을 제공하는 맞춤형 템플릿 언어를 사용하지만 if 문들과 목록도 포함될 수 있고 반복문도 포함될 수 있다
- 넌적스
- EJS
    - 일반 HTML을 사용하고 단순한 JavaScript를 사용할 수 있게 하는 플레이스홀더가 있어 for 루프를 위해 if문도 작성할 수 있다
- Handlebars
    - 일반 HTML을 사용하지만, 제한된 기능의 맞춤형 템플릿 언어도 사용하고, if 문이나 목록 등의 공통 요소들을 포함한다

```jsx
//조건문이 사용된 pug 코드
main 
            if prods.length > 0
                .grid
                    each product in prods
                        article.card.product-item
                            header.card__header
                                h1.product__title #{product.title}
                            div.card__image
                                img(src="https://cdn.pixabay.com/photo/2016/03/31/20/51/book-1296045_960_720.png", alt="A Book")
                            div.card__content
                                h2.product__price $19.99
                                p.product__description A very interesting book about so many even more interesting things!
                            .card__actions
                                button.btn Add to Cart
            else 
                h1 No Products
```

### 템플릿 엔진 설치&설정

```jsx
npm install --save ejs pug express-handlebars

//handlebars 패키지가 있긴 하지만 잘못된 패키지이며 여기엔 express-handlebars가 필요하다
//Express 내부에 통합되어 있기 때문이고 ejs와 pug는 이미 핵심 패키지에 이 요소가 구축되어
//있다
```

3가지 엔진은 모두 Node 코드의 일부이며 마지막에 일부 컴퓨터에 배포하게 되는 코드와 함께 전달되므로 프로덕션 의존성으로 이들을 설치한다

동적 템플릿을 렌더링할때 node.js의 진입점인 app.js에 express를 준수하는 템플릿 엔진이 있다고 express에 알리면 설치한 3개 모두에 적용된다

▼Express에게 pug 엔진을 사용해서 동적 견본을 컴파일하고 싶다는 것과 견본들을 찾을 장소를 알려주고 있다

```jsx
app.set('view engine', 'pug');
app.set('views', 'views'); //view 기본값을 다른 폴더명에 저장하면 이 구성 항목을 설정해야 한다
```

view 엔진은 express에게 우리가 렌더링 하려고 하는 동적 템플릿이 있으며 특별한 함수가 있으니 이 엔진을 사용해 달라고 알리며 view는 Express에게 이러한 동적 view를 어디에서 찾을 수 있는지 알리는 기능이 있다

### .pug 템플릿 렌더링

shop.pug 파일을 렌더링 하려면 

pug 템플릿 엔진을 설정하고 라우터에서 res.render()

```jsx
router.get('/', (req, res, next) => {
	res.render('shop');
});
```

HTML 페이지 소스를 보면 일반 HTML 코드라는 것이 보인다

브라우저가 읽을 수 없는 최소 버전(pug 코드)가 아니라 

해당 최소 버전을 기반으로 pug가 생성해 준 HTML 코드이다

마지막 단계는 템플릿을 추가하는 것이다

views로 가서 pug 파일을 추가한다

이 파일에 html을 입력하면 pug에서 동작하는 코드로 자동으로 변환 시킨다

### 동적 컨텐츠 렌더링

관리자 데이터 결과에서 결과를 가져와서 우리의 템플릿으로 전달한 뒤 템플릿에 투입함으로써

템플릿 파일에서 사용하고 출력하도록 하려면

렌더링 메서드에 두 번째 인수를 전달할 수 있는데 렌더링 메서드는 우리의 view에 추가되어야 하는 데이터를 전달할 수 있게 한다

자바스크립트 객체 형태로 전달하며 이 객체를 키 이름에 맵핑 후 템플릿 내부에서 전달하는 데이터를 참조하기 위해 사용한다

템플릿으로 전달되어 템플릿 내에서는 prods에만 접근할 수 있게 된다

하나 이상의 필드를 전달할 수 있다

### 템플릿에서 코드 수정 시 영향

템플릿은 단지 상황에 따라 선정되는 요소들이므로

서버를 재시작할 필요가 없고 노드몬도 작동하지 않는다

다음번 요청을 위해 템플릿을 변경하는 경우 새로운 버전을 자동으로 받아들인다

### Pug - HTML 표현

```jsx
//퍼그
doctype html
html
	head
		title= title
		link(rel='stylesheet', href='/stylesheets/style.css')

//HTML
<!DOCTYPE html>
<html>
	<head>
		<title>익스프레스</title>
		<link rel="stylesheet" href="/style.css" />
	</head>
</html>
```

```jsx
//퍼그
#login-button
.post-image
span#highlight
p.hidden.full

//HTML
<div id="login-button"></div>
<div class="post-image"></div>
<span id="highlight"></span>
<p class="hidden full"></p>
```

※퍼그는 div를 생략할 수 있다, 다수의 클래스는 마침표(.)로 이들을 결합시킨다 

```jsx
//퍼그
p Welcome to Express
button(type='submit') 전송

//HTML
<p>Welcome to Express</p>
<button type="submit">전송</button>
```

```jsx
//퍼그
p
	| 안녕하세요.
	| 여러 줄을 입력합니다.
	br
	| 태그도 중간에 넣을 수 있습니다.

//HTML
<p>
	안녕하세요. 여러 줄을 입력합니다.
	<br />
	태그도 중간에 넣을 수 있습니다.
</p>
```

```jsx
//퍼그
style.
	h1 {
		font-size: 30px;
	}
script.
	const message = 'Pug';

//HTML
<style>
	h1 {
		font-size: 30px;
	}
</style>
<script>
	const message = 'Pug';
	alert(message);
</script>
```

### Pug - inclue

- 퍼그 파일에 다른 퍼그 파일을 넣을 수 있다
    - 헤더, 푸터, 네비게이션 등의 공통 부분을 따로 관리할 수 있어 편리하다
    - include로 파일 경로 지정한다
    
    ```jsx
    //header.pug
    header
    	a(href='/') Home
    	a(href='/about') About
    
    //footer.pug
    footer
    	div 푸터입니다
    
    //main.pug
    include header
    main
    	.
    	.
    include footer
    ```
    
    ### Pug - extends와 block
    

- 레이아웃을 정할 수 있음
    - 공통되는 레이아웃을 따로 관리할 수 있어 좋음, include와도 같이 사용

```jsx
//layout.pug
doctype html
.
.

block style
.
.
block script

//body.pug
extends layout

block content
	main
		p 내용입니다.

block script
	script(src="/main.js")
```

### HandleBars 원리

- HandleBars 템플릿은 논리를 실행할 수 없다
- 단일 속성, 단일 변수나 그 값만 if 블록을 사용하여 출력할 수 있다
- 모든 논리를 express에서 처리하기 때문에 템플릿은 깔끔하게 유지된다

### 사용방법

- 템플릿 html을 세팅한다. 데이터 바인딩 시킬 부분을 {{프로퍼티명}}의 형태로 작성한다
- html템플릿을 가져온 후 HandleBars로 compile한다
- 컴파일한 템플릿에 데이터를 동적으로 넣고 html로 리턴한다
- 리턴받은 html을 렌더링한다

### EJS

- 템플릿에 JavaScript 코드를 쓸 수 있는 유연성이 좋다
- 자바스크립트가 내장된 HTML 파일이라고 볼 수 있다
- 페이지를 동적으로 생성되도록 효율적으로 코드를 작성 할 수 있다
- 서버에서 보낸 변수를 가져와서 사용할 수 있다

### 사용방법

```jsx
<% %>
이 태그는 자바스크립트를 실행할 수 있다.

<%= %>
이 태그는 변수 값을 내장시킬 수 있다.
```
