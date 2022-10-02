# Section6

# 템플릿 엔진

### 정의

- html 페이지에 동적 콘텐츠를 넣기 위해 사용
- 템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 sw | sw Component
- HTML 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 데이터만 템플릿 특정 장소에 끼워 넣는 방식으로 동작할 수 있도록 해줌.

### Template Engine 종류

- EJS
    - `<p><%= name %></p>`
    - template에 js코드를 쓸 수 있는 유연성
    - Unescaped HTML코드를 출력할 때 사용
    
    <aside>
    💡 Unescaped
    : HTML코드를 가진 문자열에 해당하는 변수를 렌더링하는 경우, 사이트 간 스크립트 공격을 피하기 위해 해당 코드가 아닌 텍스트로 렌더링되는 것
    
    </aside>
    
- Pug
    - `p #{name}`
    - express의 패키지 view engine
    - html에서 닫는 태그가 없음 (</head>, </body>, </title> 등) → 들여쓰기 이후 공백까지 태그
    - tag의 속성으로 넣으려면 ( )괄호 사용 `button.btn(type="submit")Add Product`
    - Pug에서는 레이아웃을 확장한 view 내부로부터 렌더링 되어야 하는 콘텐츠를 동적으로 추가하는 블록을 정의할 수 있다.
- Handlebars
    - `<p>{{ name }}</p**>**`
    - 내장 엔진
    - Handlebars 템플릿은 논리를 실행 불가, 단일 속성 혹은 변수 값만 출력 가능
    - 모든 논리를 Express 코드로 넣도록 만들어 템플릿을 깔끔하게 유지됨 
    → Express코드에 논리를 많이 두는 것이 원칙

### 설치 방법

- express 앱을 저장한 변수 ex)app 에서 `set()` 사용
    - `set()` 을 통해 Express 애플리케이션 전체에 어떤 값이든 설정 가능
    - `get()` 을 통해 값을 가져올 수 있음

```jsx
app.set("view engine", "pug");
//view engine 으로 pug를 등록해 동적 template을 렌더링한다는 의미
```

### 작동 방법

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

### 장단점

- 장점
    - HTML과 비교하여 간단한 문법 사용
    - 높은 재사용성
    - 유지보수에 용이
    - 속도(SSR)
- 단점
    - user interation이 많은 화면 구현이 어려움

### 분류

- web template engine은 view code(HTML)과 data logic code (DB connection)을 분리
- 서버 사이드 템플릿 엔진과 클라이언트 사이드 템플릿 엔진으로 분류
    - Server Side Template Engine : 서버에서 DB | API에서 가져온 데이터를 미리 정의 된 template에 넣어 html그려 client에게 전달
    - Client Side Template Engine : HTML 형태로 코드를 작성할 수 있고, data를 받아서 동적으로 DOM 객체에 그려주는 process담당