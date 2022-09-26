# week-02

# Framework

---

특정 프로그램을 개발하기 위한 여러 요소들과 메뉴얼인 룰을 제공하는 프로그램

간단하게 말하자면, 클래스와 라이브러리의 집합체라고 보면 된다.

# Express.js

---

제 3자 패키지로 설치 할 수 있고,

더 나은 Node.js 코드를 작성하게 도와주고, 비즈니스 처리에 도움을 주는 Framework.

 Node.js는 Chrome의 V8엔진을 이용하여 javascript로 브라우저가 아니라 서버를 구축하고, 서버에서 JavaScript가 작동되도록 해주는 런타임 환경(플랫폼)이라고 했다. Express는 이런 Nodejs의 원칙과 방법을 이용하여 웹애플리케이션을 만들기 위한 프레임워크이다.

# Middleware

---

들어오는 요청을 express.js에 의한 다양한 함수로 인해 자동으로 이동하는 것

즉, 단일 핸들러를 사용하는 대신 우리가 응답을 전송하기 전까지 요청이 통과하게 될 다양한 함수들을 연결할 가능성을 확보한다.

이를 통해 코드를 하나의 블럭으로 보는 것이 아닌 다수의 블럭으로 보는 것이다.

### 미들웨어 함수는 다음과 같은 태스크를 할 수 있다.

- 모든 코드를 실행.
- 요청 및 응답 오브젝트에 대한 변경을 실행.
- 요청 응답 주기를 종료.
- 스택 내의 그 다음 미들웨어 함수를 실행.

# Middleware Level

---

1. **애플리케이션** 레벨
2. **라우터** 레벨

 일단은 두 가지로 나뉜다.

실행 순서는 애플리케이션 -> 라우터로 진행된다.

**애플리케이션과 라우터 레벨은 실행 순서 차이지 기능적으론 차이가 없다.**

# app.use(path,req, res, next)

---

새로운 미들웨어 함수를 추가하는 기능을 가졌다.

path를 인수로 넣었을 때는 접두어만 사용해도 작동이 된다. 하지만 정확히 기입해야된다. 오류가 발생할 수 있다.

next()를 사용해야 다음 미들웨어로 넘어간다.

![캡처](https://user-images.githubusercontent.com/108862575/192126232-f68dc5af-4661-4a9a-9f75-3569c1180fa5.PNG)

# res.send()

---

자동으로 UTF-8 인코딩을 해준다. 이 말인 즉, 대부분의 언어를 사용할 수 있다.

send()는 write()와 end()를 합쳤다고 생각하면 된다.

그래서 send()는 하나밖에 적용이 안되고,  두번 이상 적으면 맨 위에 send()빼고 다 무시된다. 

# res.redirect()

---

인자에 url을 입력하면 GET요청을 자동으로 해주는 메소드이다. 이 처리 과정은 매우 빠르기 때문에 알아차리기 힘들다. 

# Router

---

Express를 사용하는 이유 중 하나는 Router를 깔끔하게 관리할 수 있기 때문이다.

예를 들어 app.get() 같은 부분이 router이다. 그러나 app.get()을 많이 사용하면, 코드가 무진장 길어진다.

그래서 Express.js는 router를 분리할 수 있는 방법을 제시한다. 

### Router 매개변수

---

```jsx
import express from 'express';
const router = express.Router();
router.get('/user/:id', (req, res) => {
console.log([req.params.id](http://req.params.id/)); // 매개변수라서 .params로 접근을 한다.
});
```

- 이처럼 :id는 /user/:id 그대로 url 들어 가는 것이 아닌 :id대신 다른 값으로 등록된다.
- req.prams에는 {id: ‘goo’} 형식으로 저장된다.

```jsx
import express from 'express';
const router = express.Router();
 
router.get('/user/:id', (req, res) => {
    console.log(req.params, req.query);
    res.send(``);
});
 
app.use('/', router);
```

- 이처럼 주소에 쿼리 스트링을 사용할 수 있다.
- **/users/123?limit=5&skip=10** 라는 주소로 요청을 하면,

```jsx
{ id: '123' } { limit: '5', skip: '10' }
```

- 이런 결과를 얻는다.

# 헬퍼함수

---

개발자의 편의를 위한 함수

이 강의에서는 .dirname이라는 함수를 통해 개발자의 편의를 도와줬다.
