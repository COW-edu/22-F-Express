### 동적 라우팅

**express는 자체적으로 동적 세그먼트를 지원**

라우팅 경로에 `:` 를 통해 지정한 동적 세그먼트는 경로 인식을 안하도록 함

<aside>
💡 동적 세그먼트를 활용한 라우터보다 구체적인 경로를 가진 라우터를 상위에 위치해야 한다.
</aside>

<br>

`:` 를 통해 설정한 동적 세그먼트에 엑세스하는 방법

`localhost:3000/products/:productId`

`req.params.productId;` → 라우트 경로의 productId에 엑세스

<br>

### Post 요청 데이터 전달

- `req.body.productId` : HTML폼을 통한 post요청의 요청메시지를 수신
  - productId는 input태그의 name 속성으로 지정

<br>


### 쿼리 파라미터

URI 경로에 `?key=value` 형태로 사용, `&`로 다수의 쿼리파라미터 사용 가능

`req.query.key` → 쿼리파라미터 접근
