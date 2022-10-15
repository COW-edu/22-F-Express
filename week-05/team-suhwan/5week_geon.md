# Section 9

## 동적 라우트

```jsx
*router.get*('/products/:productId');
```

- : 콜론: Express에게 productId와 같은 라우트를 탐색하지 말라는 신호를 보냄

```jsx
router.get('/products/delete'); 

router.get('/products/:productId'); 
```

- 동적 세그먼트와 특정 라우트가 있으면 더 구체적인 경로를 가진 라우트를 앞에 두어야 한다.

### POST 요청으로 데이터 전달하기

```jsx
exports.postCart = (req, res, next) => {
	const prodId = req.body.productId;
	console.log(prodId);
	res.redirect('/cart');
};
```

- 상품의 아이디를 가져와 cart 페이지로 렌더링한다.

### Query String

- 어떠한 애플리케이션에 정보를 전달할 때 사용되는 URL에 약속되어 있는 국제적인 표준

- `req.query` 를 사용하여 url 내의 query String을 가져올 수 있다.

```jsx
app.get('/topic',function(req, res) {
	// url이 http://a.com/topic?id=1&name=siwa 일때
	res.send(req.query.id + ',' + req.query.name);// 1,siwa 출력
});
```