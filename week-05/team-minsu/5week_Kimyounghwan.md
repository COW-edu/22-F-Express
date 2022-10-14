# week-05

## SECTION 8,9

# 동적 페이지

페이지의 URL이 고정되어 있는 정적 페이지와 다르게 사용자와 상호작용을 통해 받은 파라미터에 따라 다른 페이지를 출력하는 웹 페이지를 말한다. 

**페이지 명은 같지만 URL 뒤에 부분에 설정 되어있는 파라미터 값에 따라 보여지는 웹 페이지가 달라진다.** 

# Dynamic Routing

**의미**: 라우트 경로에 특정 값을 넣어 해당 페이지로 이동 할 수 있게 하는 것.

Express 라이터 경로에 :(콜론)을 추가함으로써 동적 경로 세그먼트를 전달할 수 있다.

:(콜론) 다음에 오는 이름은 req.params에서 데이터를 추출할 수 있다.

**—참고로 한 라우트는 여러개의 세그먼트를 가질 수 있다.**

### 정적 라우팅, 동적 라우팅 차이

백문이 불여일견이다. 예시를 한번 보자.

Static routing

```jsx
router.get('/Bibi', function(req, res, next) {
  const name = req.originalUrl.split('/')[2];
  const user = data.filter(u => u.name == name);
  return res.json({ message: 'User Show', data: user });
});

router.get('/Colt', function(req, res, next) {
  const name = req.originalUrl.split('/')[2];
  const user = data.filter(u => u.name == name);
  return res.json({ message: 'User Show', data: user });
});

router.get('/Jessie', function(req, res, next) {
  const name = req.originalUrl.split('/')[2];
  const user = data.filter(u => u.name == name);
  return res.json({ message: 'User Show', data: user });
});
```

반복되어 있는 부분(비효율적인 부분)들이 보인다.

Dynamic routing

 

```jsx
router.get('/:name', function(req, res, next) {
  const name = req.params.name;
  const user = data.filter(u => u.name == name );
  return res.json({ message: 'Users Show', data: user });
});
```

Static routing보다 훨씬 깔끔해 보인다. 

## 쿼리 파라미터

URL 마지막에 ?를 덧붙인 매개변수이다.

&를 통해 여러 개를 가질 수 있다.

req.query.{query string 키 이름}를 통해 값을 추출할 수 있다.

```jsx
//값 추출을 예를 들어 설명하자면, 
//HTTP GET /search?q=김영환 으로 요청이 들어왔을때
const q = req.query.q //로 하면 "김영환"이라는 value를 얻을 수 있다.
```