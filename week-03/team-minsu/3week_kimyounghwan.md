# week-03

# Templating Engines

---

**의미:** 템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 소프트웨어 또는 소프트웨어 컴포넌트를 말한다.

**사용 이유**

1. 코드 길이를 줄일 수 있다.
    
    템플릿 엔진을 사용하면 기존의 HTML에 비하여 간단한 문법을 사용한다. 따라서 코드를 훨씬 간결하게 작성할 수 있다.
    
2. 재 사용성을 높일 수 있다.
    
    웹 페이지는 똑같은 디자인의 페이지에 데이터만 바뀌는 경우가 매우 많다. 이럴 때 템플릿 엔진을 사용하면 미리 템플릿을 만들어 놓고 데이터를 바꿔가면서 수많은 페이지를 만들어 낼 수 있으므로 효율적이다.
    
3. 유지 보수에 용이하다.
    
    하나의 템플릿을 미리 만들어 놓고 관리하는 것은 수백 개의 비슷한 모양의 HTML 페이지를 관리하는 것보다 훨씬 효율적이다.
    

**구동 방식**

1. 클라이언트 요청을 받는다.
2. 필요한 데이터를 DB, API에서 가져온다.
3. 미리 정의된 템플릿에 해당 데이터를 배치한다.
4. 서버에서 HTML을 그린다.
5. 해당 HTML을 클라이언트에 전달한다.

**Example** 

- **EJS(Embedded JavaScript) :** 쉽게 말해 js가 내장된 HTML 파일.
    
    구문 형태 Ex) <p><%= name %> </p>
    
    **<특징>**
    
    Use normal HTML and plain  js in your templates.
    
- **Pug**
    
    구문 형태 Ex) p #{name}
    
    **<특징>** 
    
    Use minimal HTML and custom template language.
    
- **Handlebars**
    
    구문 형태 Ex) <p>{{name}}  </p>
    
    **<특징>**
    
    Use normal HTML and custom template language.
    

## PUG, EJS, Handlebars 설치

---

**설치 방법**

```jsx
npm install --save ejs pug express-handlebars
```

## app.set(name, value)

---

Express 애플리케이션 전체에 **어떤 값이든지** 설정할 수 있다.

**app.get(name)을 사용해 설정한 value값을 얻을 수 있다.**

## view engine

---

서버에서 처리한 데이터 결과 값을 HTML파일에 보다 편리하게 출력해주기 위해 사용한다. 

View Engine에서 요구하는 형태로 템플릿 파일을 만들고, 이 파일에 서버에서 처리한 데이터를 전달하면, 데이터를 시각화 할 수 있다.

## PUG 구현

---

ㅁㅁㅁ.pug 파일을 만들고 HTML5 템플릿을 사용하면 아래의 pug 구조가 나온다.

```jsx
doctype html
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(http-equiv="X-UA-Compatible", content="IE=edge")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        title Document
    body
```

이 코드에는 일반 HTML 코드가 없지만 pug 템플릿 엔진이 HTML로 컴파일해 줄 것이다.

pug 코드는 HTML5와 유사하며 **들여쓰기**가 가장 중요하다.

또한 HTML에서 닫는 태그가 없는 형식이다. 

**(아무런 코드 없이, .main-header 형식이라면 자동으로 div다.)**

```jsx
doctype html
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(http-equiv="X-UA-Compatible", content="IE=edge")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        title My Shop 
        link(rel="stylesheet", href="/css/main.css")
        link(rel="stylesheet", href="/css/product.css")
    body 
        header.main-header 
            nav.main-header_nav 
                ul.main-header_item-list 
                    li.main-header_item 
                        a.active(href="/") Shop 
                    li.main-header_item
                        a(href="/admin/add-product") Add Product
```

### Each 문

pug에서 지원하는 두 가지 반복문 중 하나다. 간단히 말하면 Python for문이랑 굉장히 흡사하다.

```html
 ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

이렇게 Array를 반복하는 형식으로 li를 간단하게 만들 수 있다.

```html
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
```

또한 index도 같이 얻을 수 있다.

# Handlebars

---

**의미**

자바스크립트의 템플릿 엔진 중 하나이다. 

‘{{ }}’로 데이터를 표현하는 Mustache를 기반으로 구현한 템플릿 인자이다.

HTML 페이지를 HTML+ Mustache의 구성되어 가시성이 좋기 때문에 디자이너와 협업에 용이하다.

**구현**

```jsx
import expressHbs from 'express-handlebars';

app.engine('handlebars', expressHbs());

app.set('view engine', 'handlebars');
```

**기본 표현식**

```jsx
<p>{{firstName}} {{lastName}}</p>
```

**Dot-notation도 사용 가능하다.**

```jsx
{{person.firstName}}{{person.lastName}}
```

**HTML-escaping**

```jsx
{{specialChart}} //이 상태면 HTML-escaping 함
{{{specialChart}}} //이렇게 삼중 중괄호를 사용하면, HTML-escaped가 되지 않음.
```

## EJS

---

**의미**

위에서 설명 하듯, JS가 삽입된 HTML이다.

일반 HTML도 JS를 삽입할 수 있다. 하지만 EJS는 HTML에서 태그처럼 JS를 삽입할 수 있다.

**구현**

  

```jsx
app.set('view engine', 'ejs');
```

**기본 표현식**

```jsx
<% %> //모든 js코드는 <% %>가 들어가야된다.
<%=(변수명) %> //변수를 출력 할 때에는 <%=(변수명) %>을 사용한다. 
```

## Lay-out

---

**사용하는 이유**

-편의성

예를 들어 header 부분이 마음에 안 들어 바꾸고 싶다면, 모든 템플릿들의 header부분을 바꿔줘야 할 것이다.  이런 비효율적인 행위를 하기 싫어서 사용하는 것이다.

#