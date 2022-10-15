Passing Route Params & Using Query Params

- http://www.domain.com:1234/path/to/resource?a=b&x=y
    - http : protocol
    - [www.domain.com](http://www.domain.com) : host
    - 1234: port
    - path/to/resource : resource path
    - a=b&x=y : query

Header란?

request 헤더에 포함된 파라미터 보통 인증 혹은 권한 부여와 관련되어 있다.

HTTP 헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다

EX) 요청자, 컨텐트 타입, 캐싱..

Http Header는 다음과 같이 4가지가 있다.

1. **General header** : 요청과 응답 모두에 적용되지만 바디에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더.
2. **Request header** : 페치될 리소스나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더. == 내가 보내는 메세지의 헤더
3. **Response header** : 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더. == 내가 받은 메세지의 헤더
4. **Entity header**: 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더.

request params

- api/posts/:id
- req.params
- resource path에서 값을 전달하는 방법

Using Query Params(query string 파라미터)

- 클라이언트가 입력 데이터를 전달하는 방법 중의 하나

```
https://www.inflearn.com/courses?order=seq&skill=python
```

- 위 URL에서 물음표 다음에 오는 것들이 쿼리 스트링이다.쿼리스트링은 key=value 형태라고 보면 된다.위 예시에선 order가 key이고 seq가 value이다.
- 그리고 쿼리스트링을 여러개 보내고 싶으면 &(앤드 연산자)를 붙여서 추가할 수 있다.위 예시에선 두개의 쿼리스트링을 보내고 있는 것이다.
- 1. order(key) = seq(value)2. skill(key) = python(value)

```jsx
import { Router } from "express";

const api = Router();

let postData = [
];
api.get("/posts/:id", (req, res) => {
  const { id } = req.params;
  if (!postData[id - 1]) {
    return res.json({
      error: "Post not exist",
    });
  }
  return res.json({
    data: postData[id - 1],
  });
});
```
