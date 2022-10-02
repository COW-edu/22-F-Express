# 템플릿 엔진

HTML은 정적인 언어

주어진 기능만 사용할 수 있으며, 직접 기능을 추가할 수 없음

**템플릿 엔진은 자바스크립트를 사용하여 HTML을 렌더링할 수 있게 해줌**

**웹사이트 화면을 어떤 형태로 만들지 도와줌.**

Pug,  Nunjucks, EJS, Handlebars

## 서버 사이드 템플릿 엔진

서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 Template에 넣어 Html을 그려서 클라이언트에 전달해줌.

HTMl 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 부분만 템플릿 특정 장소에 끼워 넣는 방식으로 동작할 수 있도록 해줌.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d7916e9-b934-4c97-a7fb-d9452c8bbeec/Untitled.png)

### 동작 과정

1. 클라이언트의 요청을 받는다.
2. 필요한 데이터 DB나 API에서 가져온다.
3. 미리 정의된 Template에 해당 데이터를 배치한다.
4. 서버에서 HTML(데이터가 반영된 Template)을 그린다.
5. 해당 HTML을 클라이언트에 전달한다.

## 클라이언트 사이드 템플릿 엔진

HTML 형태로 코드를 작성할 수 있으며 동적으로 DOM을 그리게 해줌.

데이터를 받아서 DOM 객체에 동적으로 그려주는 프로세스를 담당함.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0b071a32-0915-42e3-b73a-344b6a7f087c/Untitled.png)

<aside>
💡 문서 객체 모델(DOM, Document Object Model): **XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스**

</aside>

### 동작 과정

1. 클라이언트에서 공통적인 프레임을 미리 Template로 만든다.
2. 서버에서 필요한 데이터를 받는다.
3. 데이터를 Template에 배치하고 DOM 객체에 동적으로 그려준다.

- 문서 내의 모든 요소를 정의하고, 각각의 요소에 접근하는 방법을 제공
- Handlebars

### 템플릿 엔진의 필요성

- 많은 코드를 줄일 수 있음

 대부분의 템플릿 엔진은 기존의 HTML에 비해 간단한 문법을 사용함

- 재사용성이 높음

똑같은 디자인의 페이지에 보이는 데이터만 바뀌는 경우 多

- 유지보수에 용이

하나의 템플릿을 만들어 여러 페이지를 렌더링

## Pug

: express의 패키지 view engine

HTML 을 PUG 문법으로 작성하면 HTML 로 바꿔줌

```jsx
app.set('views', path.join(__dirname, 'views'); // 폴더 경로 지정
app.set('view engine' , 'pug'); // 확장자 지정
 
// => 해당 폴더에서 확장자가 pug인걸 고르겠다는 의미
```

### 특징

1. html에서 닫는 태그가 없다 (</head>, </body>, </title> 등)들여쓰기로 어디서 부터 어디까지인지 구분하기 때문에 닫는 태그를 적을 필요가 없다.
2. 들여쓰기한 이후에 공백까지가 태그로 된다.
3. 태그 사이가 아닌 태그의 속성으로 넣으려면 ()괄호 사용

```jsx
 html(lang='en') , script(type='text/javascript')
```

### app.set()

: Express 애플리케이션 전체에 어떤 값이든 설정할 수 있음

## Handlebars

PUG와 달리 Express에 의해 자동으로 설치되는 패키지가 아님

```jsx
const expressHbs = require('hbs');

const app = express();
app.set('hbs', expressHbs());
app.set('view engine', 'hbs');
```

### block문

: 단순 텍스트가 아닌, 조건부로 출력되거나 루프에 있는 내용을 포함함

### Pug vs Handlebars

Handlebars 템플릿은 논리를 실행할 수 없고 단일 속성 혹은 변수 값만 출력할 수 있다. 

템플릿에 논리를 많이 두지 않고 Express 코드에 논리를 많이 두는 것이 원칙이다.

장점: 모든 논리를 Express 코드로 넣도록 만들어 템플릿을 깔끔하게 유지됨. 

Pug에서는 레이아웃을 확장한 view 내부로부터 렌더링 되어야 하는 콘텐츠를 동적으로 추가하는 블록을 정의할 수 있다. 

## EJS

: PUG처럼 Express에서 즉시 지원되는 템플릿 엔진임

Handlebars와 달리 논리를 실행할 수 있음

<% ~ %> 태그 사용

### <%=  %>

: 동적으로 출력하는 과정에서 이 플레이스홀더를 두는 위치에 값을 출력할 때 사용

### <%- %>

:  공유된 view 부분을 출력할 때 사용

“-” 기호는 Unescaped HTML코드를 출력할 때 사용하는 코드임.

 Unescaped란 HTML코드를 가진 문자열에 해당하는 변수를 렌더링하는 경우 사이트 간 스크립트 공격을 피하기 위해 해당 코드가 아닌 텍스트로 렌더링되는 것

 

```jsx
<%- include('includes/head.ejs') %>
```

include 키워드는 특정 요소를 페이지에 포함할 수 있게 함

- Quiz