# section 6

## 섹션 6 - 동적 콘텐츠 작업 및 템플릿 엔진 추가.

# 모듈 소개

- Managing Data (without a DB)
- 동적 컨텐츠 렌더링
- Templating Engines 이해

# 요청 및 사용자 간의 데이터 공유

하나의 파일에서 배열을 만든 후에 POST 요청시 배열에 데이터 저장.

이후 다른 파일로 export 하여 받아낼 수 있음.

그 데이터를 console.log()로 보면 잘 보임.

그런데 다른 브라우저에서 이 주소를 불러오면 원래 브라우저에서 입력해두었던 데이터가 그대로 남아있다.

이러한 경우는 다른 사용자들의 데이터가 남에 의해 만들어질 수 있으므로 피하는 것이 좋다.

# 템플릿 엔진

### Templating Engines

HTMLish Template

Node/Express Content + Templating Engine

→ 실제 HTML 로 교체.

사용 가능한 템플릿 엔진

- EJS - HTML 형식
  - 일반 HTML & JS
- PUG - 최소화된 버전 활용
  - 최소화된, 커스텀.
- Handlebars - 이중 중괄호
  - 일반 HTML + 커스텀

# Pug 설치 및 구현

프로덕션 의존성으로 다운.

### app.set()

app.set으로 동적 템플릿 엔진을 express에게 사용하라고 알려줌.

```jsx
app.set("view engine", "pug");
app.set("views", "views"); //위치 설명
```

### render()

기본 템플릿 엔진을 사용하여서 템플릿 반환.

```jsx
res.render("shop"); //shop.pug 불러옴
```

render 메서드에 키 값으로 데이터 전달 가능.

```jsx
#{ value };
```

중괄호 안에 원하는 값 넣어서 전달 가능함.

### pug 문법

html 의 태그 이름만 쓰고, 닫는 태그 또한 쓰지 않는다.

하위 태그의 구분은 들여쓰기로 진행.

# 핸들바 작업

사용하기 위해서는 뷰 엔진을 변경해야함.

익스프레스에 자동으로 다운되는 것이 아님.

논리 실행 x

### app.engine()

등록되지 않은 새로운 템플릿 엔진 등록

```jsx
app.engine("handlebars", expressHbs()); //엔진 초기 설정
```

### 동적출력

키: 값 방식은 동일.

```jsx
{
  {
    value;
  }
}
```

중괄호 두개 사이에 전달해준 값으로 렌더링.

# EJS 작업

pug의 확장된 기능을 갖추고 있음.

익스프레스에 내장된 엔진.

일반 HTML 활용.

```jsx
<%= value %> // 변수 값 저장

<% JS %> // JS 코드 사용 가능.
```

### include()

반복되는 코드들을 따로 빼내서 사용 가능.
