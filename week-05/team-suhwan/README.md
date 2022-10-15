# TEAM 수환 - 5주차 RBF
# TEAM 수환 - 5주차 RBF
## 동적 라우트

**express는 자체적으로 동적 세그먼트를 지원**

라우트를 정적으로 고정시켜서, 하나의 위치를 도착장소로 갖는 route가 아니라, 해당 변수에 따라 동적인 값을 갖는 route

```jsx
//router
router.get('/products/:productId');
//html
<a href="/products/<%= product.id %>">text</a>
```

- express 라우터 경로에 :를 통해 변수 세그먼트가 있음을 express 라우터에게 정보제공
→ 이후에 prodictId의 이름으로 정보를 추출할 수 있도록 하며, prodictId와 같은 라우트를 탐색하지 말라는 의미
- 동적 라우트는 코드 실행구조를 고려하여 가장 하단에 배치해야함
- 라우팅 경로에 `:` 를 통해 지정한 동적 세그먼트는 경로 인식을 안하도록 함

`:` 를 통해 설정한 동적 세그먼트에 엑세스하는 방법

`localhost:3000/products/:productId`

`req.params.productId;` → 라우트 경로의 productId에 엑세스

```jsx
router.get('/products/delete');
router.get('/products/:productId');
```

<aside>
💡 동적 세그먼트를 활용한 라우터보다 구체적인 경로를 가진 라우터를 상위에 위치해야 한다.

</aside>

- **쿼리 문자열 방식**보다 경로명이 조금 더 깔끔해지는 효과를 얻을 수 있다.
    - 여러 형태로 쓰일 수 있다.
        - 경로의 뒤에 쓰이는 경우
            - /user/:id
        - 경로의 중간에 쓰이는 경우
            - /user/:id/edit
        - 혼합 형태
            - user/:id/edit/:attr
            - 좋은 방식은 아님

### Post 요청 데이터 전달

- `req.body.productId` : HTML폼을 통한 post요청의 요청메시지를 수신
    - productId는 input태그의 name 속성으로 지정

### 쿼리 파라미터

URI 경로에 `?key=value` 형태로 사용, `&`로 다수의 쿼리파라미터 사용 가능

`req.query.key` → 쿼리파라미터 접근

```jsx
app.get('/topic',function(req, res) {
	// url이 <http://a.com/topic?id=1&name=siwa> 일때
	res.send(req.query.id + ',' + req.query.name);// 1,siwa 출력
});
```

- URL의 마지막에 물음표를 덧붙인 매개변수로 `edit=true` 등의 양식으로 =표시를 통해 분할한 키 값 쌍입력 가능
- &를 사용하여 여러개의 쿼리 매개변수를 가질 수 있고 req.query.myParam을 통한 값 추출 가능
- 라우트 registry에 매개변수를 등록하거나 이름 붙일 필요없이 등록 없이 추출 시작하는데 매개변수에 기반하면 매개변수가 잘 전달되었는지 확인하는 과정이 필요
- edit=true의 설정해서 쿼리 매개변수를 첨부하면 편집가능한 option이 준비됨
- 검색 및 필터링에서 자주 사용되는 방법
- 사용자를 추적하거나 사용자가 페이지에 설정한 특정 필터를 유지하기 위해 사용된다.
    - 웹 서버에서는 쿼리 문자열로 전달한 값을 GET 요청으로 받아들인다.