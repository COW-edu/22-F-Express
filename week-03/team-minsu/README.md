# TEAM 민수 - 3주차 RBF

# Templating Engines

**의미**
템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 소프트웨어 또는 소프트웨어 컴포넌트를 말한다.

**사용 이유**

1. 코드 길이를 줄일 수 있다.
    
    템플릿 엔진을 사용하면 기존의 HTML에 비하여 간단한 문법을 사용한다. 따라서 코드를 훨씬 간결하게 작성할 수 있다.
    
2. 재 사용성을 높일 수 있다.
    
    웹 페이지는 똑같은 디자인의 페이지에 데이터만 바뀌는 경우가 매우 많다. 이럴 때 템플릿 엔진을 사용하면 미리 템플릿을 만들어 놓고 데이터를 바꿔가면서 수많은 페이지를 만들어 낼 수 있으므로 효율적이다.
    
3. 유지 보수에 용이하다.    
    하나의 템플릿을 미리 만들어 놓고 관리하는 것은 수백 개의 비슷한 모양의 HTML 페이지를 관리하는 것보다 훨씬 효율적이다.

**종류**

**레이아웃 템플릿 엔진**
    - 중복되는 include 코드를 사용하지 않고도 지정된 페이지 레이아웃에 따라 페이지 타일을 조합하여 완전한 페이지로 만들어준다.
        - 예) Apache Tiles, Sitemesh 등

**텍스트 템플릿 엔진**
    - 템플릿 양식에 적절한 특정 데이터를 넣어 결과 문서를 출력함
        - 예) Freemarker, Thymeleaf, JSP (Java Server Pages) 등

> 레이아웃과 텍스트는 역할이 다르며 섞어서 사용한다 (베타적인 것이 아님)
**서버 사이드 템플릿 엔진**

    - 서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 Template에 넣어 html을 그려서 클라이언트에 전달해주는 역할
    - 즉, HTML 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 부분만 템플릿 특정 장소에 끼워넣는 방식으로 동작할 수 있도록 해준다.
    - 과정
        1. 클라이언트의 요청을 받아서
        2. 필요한 데이터(DB에서 가져오거나 API에서 가져오거나)가져온다.
        3. 미리 정의된 Template에 해당 데이터를 적절하게 넣는다.
        4. 서버에서 HTML(데이터가 반영된 Template)을 그린다.
        5. 해당 HTML을 클라이언트에 전달한다.
    - Ex) javascript template engine
        - EJS, Jade(Pug), Handlebars 등

**클라이언트 사이트 템플릿 엔진**

    - html 형태로 코드를 작성할 수 있으며, 동적으로 DOM을 그리게 해주는 역할을 한다.
    - 즉, 데이터 받아서 DOM 객체에 동적으로 그려주는 프로세스를 담당한다.
    - 예) 웹 페이지에서 여러 카테고리 중 탭을 선택할 때마다 같은 형식의 프레임에 내용만 바뀌어 변경됨. 이런 공통적인 프레임을 미리 제작한 ‘template’이라고 부른다. 클라이언트에서는 이런 template을 매번 입력하거나 바꿀 수 없으므로 script 타입을 template으로 미리 만들어 사용한다 (안의 내용은 replace를 사용하여 바꾼다.)
    - 과정

        1) 클라이언트에서 공통적인 프레임을 미리 template으로 만든다.
        2) 서버에서 필요한 데이터를 받는다.
        3) 데이터를 template을 적절한 위치에 replace하고 DOM 객체에 동적으로 그려준다.

    - Ex) Mustache, Squirrelly, Handlebars(Handlebars.js)

# 클라이언트 사이드 템플릿 엔진의 필요성

    - Javascript 라이브러리로 랜더링이 끝난뒤 (즉, HTML Dom이 다 그려진 뒤)에 서버 통신 없이 화면 변경이 필요할 경우
    - 계속해서 페이지를 이동하여 서버 쪽으로 호출이 발생한다면 서버 사이드 템플릿 엔진을 이용하면 되는데, 단일 화면에서 특정 이벤트에 따라 화면이   계속 변경되어야 하는 경우는 javascript로 html을 렌더링하는 경우가 많다.
    - 즉 이렇게 단일 화면에서의 화면 변경에서는 서버 쪽을 사용하지 않고 화면을 그리기(이하 렌더링)위해 javascript 안에 html 코드를 작성해야 하는데, 이때 클라이언트 사이드 템플릿 엔진을 사용하지 않고 아래와 같이 javascript로 html을 렌더링하는 경우에는 여러 문제가 있다.
    - js
        - html 코드(문자열)의 오타를 찾기 어렵다. (태그 하나가 빠지거나, attrubute가 오타가 나도 IDE에서 감지할 수 없다.)
        - 렌더링 해야 할 코드가 늘어나면 늘어날수록 수정하기 어려워진다. (Dom 형태를 파악하기 어렵다.)    


**SERVER SIDE TEMPLATING ENGINE Example** 

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

단점으로는 HTML과 문법이 많이 달라 호불호가 갈린다. 

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
**HTML과 차이점**

```pug
doctype html
html
  head
    title=title
    link(rel='stylesheet', href='/stylesheets/style.css')
```

## `HTML`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>title</title>
    <link rel="stylehsheet" , href="/stylesheets/style.css" />
  </head>
</html>
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

단점으로는 코드가 좀 지저분 해 보인다.

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

**HTML과의 차이점**
## `Handlebars`

```handlebars
<!DOCTYPE html>
<html>
    <head>
        <title>{{title}}</title>
        <!--Handlebars에서는 변수를 사용할 시 {{ }} 로 감싼다.-->
        <link rel="stylesheet", href="/stylesheets/style.css">
    </head>
```

## `HTML`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>title</title>
    <link rel="stylehsheet" , href="/stylesheets/style.css" />
  </head>
</html>
```

## EJS

---

**의미**

위에서 설명 하듯, JS가 삽입된 HTML이다.

일반 HTML도 JS를 삽입할 수 있다. 하지만 EJS는 HTML에서 태그처럼 JS를 삽입할 수 있다.

단점으로는 코드가 HTML과 비슷해 그리 간단하지가 않다.

**구현**

```jsx
app.set('view engine', 'ejs');
```

**기본 표현식**

```jsx
<% %> //모든 js코드는 <% %>가 들어가야된다.
<%=(변수명) %> //변수를 출력 할 때에는 <%=(변수명) %>을 사용한다. 
```

**HTML과의 차이점**
## `EJS`

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%=title%></title>
    <!--EJS에서는 변수를 사용할 시 <%= %> 로 감싼다.-->
    <link rel="stylesheet" , href="/stylesheets/style.css" />
  </head>
  <html></html>
</html>
```

## `HTML`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>title</title>
    <link rel="stylehsheet" , href="/stylesheets/style.css" />
  </head>
</html>
```

**대체로 형태가 Handlebars와 비슷하다**

## Lay-out
---
**사용하는 이유**

-편의성

예를 들어 header 부분이 마음에 안 들어 바꾸고 싶다면, 모든 템플릿들의 header부분을 바꿔줘야 할 것이다. 이런 비효율적인 행위를 하기 싫어서 사용하는 것이다.
