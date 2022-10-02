# Templating Engines

## **What is a template engine?**

웹 프로그래밍을 할 때 주로 사용하는 마크업 언어인 HTML은 정적인 언어이다.

따라서 Javascript로 표현하면 반복문으로 간단하게 처리할 수 있는 동적 연산을 HTML으로만 표현하게 되면 일일이 직접 적어주어야 하는데, 이런 과정이 불편해서 나온것이 '템플릿 엔진' 이다.

`템플릿 엔진은 Javascript를 사용해서 HTML을 렌더링할 수 있게 도와주는 도구이다.`

## **Why Use?**

- `코드 길이를 줄일 수 있다.`

  템플릿 엔진을 사용하면 기존의 HTML에 비하여 간단한 문법을 사용한다. 따라서 코드를 훨씬 간결하게 작성할 수 있다.

- `재사용성을 높일 수 있다.`

  웹 페이지는 똑같은 디자인의 페이지에 데이터만 바뀌는 경우가 매우 많다. 이럴 때 템플릿 엔진을 사용하면 미리 템플릿을 만들어 놓고 데이터를 바꿔가면서 수많은 페이지를 만들어 낼 수 있으므로 효율적이다.

- `유지 보수에 용이하다.`

  하나의 템플릿을 미리 만들어 놓고 관리하는 것은 수백 개의 비슷한 모양의 HTML 페이지를 관리하는 것보다 훨씬 효율적이다.

<hr>

## **서버 사이드 템플릿 엔진 vs 클라이언트 사이드 템플릿 엔진**

### `서버 사이드 템플릿 엔진`

서버 사이드 템플릿 엔진은 서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 템플릿(Template)에 넣어 HTML 문서를 만들어 클라이언트에 전달해주는 역할을 한다. 즉, HTML 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 부분만 템플릿의 특정 부분에 끼워 넣는 방식으로 동작한다.

- 이러한 서버 사이드 템플릿 엔진으로는 JSP, Thymeleaf, Velocity, Freemarker 등이 있다.

### `클라이언트 사이드 템플릿 엔진`

클라이언트 사이드 템플릿 엔진은 HTML 형태로 코드를 작성할 수 있으며 동적으로 DOM을 그리게 해주는 역할을 한다.
즉, 데이터를 받아 DOM 객체에 동적으로 그려주는 프로세스를 담당한다. 예를 들어, 웹 페이지 내에 여러 카테고리 중 하나를 선택할때마다 같은 형식의 프레임에 내용만 바뀌어 변경되는 경우가 있다. 이때 클라이언트는 매번 템플릿을 입력하거나 바꾸기보다는 Script 타입을 템플릿으로 미리 만들어 사용하며 안의 내용을 replace 하는 식으로 동작한다.

- 이러한 클라이언트 사이드 템플릿 엔진으로는 Mustache, Squirrelly 등이 있다.

<hr>

## **템플릿엔진의 종류**

- Pug(Jade)
- EJS
- Handlebars
- ......

템플릿엔진에는 굉장히 다양한 종류가 있지만, 대표적으로는 `Pug와 EJS를 많이 사용한다.`

## Pug(Jade)

예전 이름인 Jade로 더 유명한 Pug는 꾸준히 많은 인기를 얻고 있는 엔진이다.

### Pug 구조와 HTML 구조 비교

## `Pug`

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

`pug는 들여쓰기를 통해 계층구조를 표현한다.`

`또한, 태그의 속성은 속성명=속성값 형태로 표현한다.`

장점 : 문법이 간단하여 코드의 양이 줄어든다.

단점 : HTML과는 문법이 매우 달라 호불호가 갈린다.

<hr>

## EJS

EJS는 Pug의 HTML 문법 변화에 적응하기 힘든 사람을 위한 템플릿 엔진이다.

EJS 구조와 HTML 구조 비교

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

`EJS는 HTML과 비슷한 구조를 가지고 있다.`

장점 : HTML 문법을 그대로 사용하되, 추가로 Javascript문법을 사용할 수 있다.

단점 : HTML 문법과 별 차이가 없어서 간단하지는 않다.

## Handlebars

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

`대체로 EJS와 비슷한 구조를 가지고 있다.`
