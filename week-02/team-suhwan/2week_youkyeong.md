# ~section5

# Express

:  http와 connect컴포넌트(middleware)를 기반으로하는 웹프레임워크

: Node.js(Chrome의 V8엔진을 이용하여 javascript로 브라우저가 아니라 서버를 구축하고, 서버에서 JavaScript가 작동되도록 해주는 런타임 환경)을 이용한 프레임워크

- 어플리케이션 구축 및 요청을 처리하는 특정 방식 제공함으로 확장성을 갖음
- 웹 앱에 MVC형태의 구조 제공
- 미들웨어 기능을 제공하는 제 3자 패키지를 쉽게 장착하여 기능을 추가

## 미들웨어

: 들어오는 요청을 express.js에 의한 다양한 함수를 통해 자동으로 이동하는 것

**→express는 일련의 미들웨어 함수호출**

→ **요청이 통과하는 퍼널(funnel)에 연결된 기능을 추가하고 next로 다음 미들웨어로 넘어가거나 응답을 보냄**

- express.js는 단일요청 핸들러를 보유하는 대신 응답 전송 전까지 요청이 통과하게 될 함수들을  연결할 가능성 확보
- 하나의 함수를 사용하여 처리하기 보다 코드를 다수의 블록이나 조각으로 분할하는 express.js특징
- 미들웨어의 실행순서는 위에서 아래
- 응답을 보내면 아래에 작성된 미들웨어 코드가 존재하더라도 도달하지 않음

### 미들웨어 함수 추가 방식

```jsx
const app =express();
app.use('경로',(req,res,next));//use()를 사용하여 미들웨어 함수 추가
//각 인수의 이름은 변경 가능

app.get()//GET요청만 받고 싶은 경우
app.post()//POST요청만 받고 싶은 경우
```

- 경로
- 경로로 시작하는 라우터를 미들웨어로 들임
- 기본값 : /
- req
- 요청
- 들어오는 요청의 본문을 분석하지 않기때문에 필요한 경우에는 미들웨어를 추가해서 분석기 등록 구현이 요구
    
    <aside>
    💡 요청이 어디로 향하든 본문 분석이 일어날 수 있게 하기 위해 경로 설정 이전에 미들웨어를 작성
    
    </aside>
    
- res
- 응답
- next
- 호출하는 경우에 한하여 다음 미들웨어로 요청을 이동시킴

[미들웨어 사용](https://expressjs.com/ko/guide/using-middleware.html)

Express에서 사용하는 다양한 미들웨어의 종류

### 오류페이지를 발생시키는 미들웨어

- 미들웨어의 가장 하단에 모든 요청을 처리하는 미들웨어를 추가

## Router

- 파일의 규모가 커지는 경우에는 여러파일로 내보내서 파일로 임포트하는 방식 권장 → 어떻게

## utility method

### **send**

: 응답 및 any유형의 본문을 첨부가능

- html content type
- 기본 응답 헤더 : text/html
- Express.js에서 입력한 유형에 따라서 HTML content type을 인식하고 선택
- setHeader를 사용해서 수동으로 설정 가능
- write보다 쉬움

[Express utilities](https://expressjs.com/en/resources/utils.html)