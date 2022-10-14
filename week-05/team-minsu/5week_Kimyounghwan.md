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

### Query parameter & Path variable

## Query parameter

경로 뒤에 입력 데이터를 함께 제공하는 식으로 사용한다.

URL 마지막에 ?를 덧붙인 매개변수이다.

? 뒷 부분을 **Query string**이라고 한다.

&를 통해 여러 개를 가질 수 있다.

req.query.{query string 키 이름}를 통해 값을 추출할 수 있다.

```jsx
//값 추출을 예를 들어 설명하자면, 
//HTTP GET /search?q=김영환 으로 요청이 들어왔을때
const q = req.query.q //로 하면 "김영환"이라는 value를 얻을 수 있다.
```

## Path variable

이름에서도 알 수 있듯이 경로를 변수로 사용한다.

예를 들어 게시판이 존재하고 그 안에 각각의 게시물을 볼 수 있는 경우를 생각하자.
게시물을 볼라면 각각의 id를 서버에 넘겨줘야 한다. 
URL 예를 들면 아래와 같다.
```jsx
/post/6
```

## Path variable VS Query parameter

가령 예를 들어 존재하지 않는 resource의 id가 들어온다면,
Path variable은 404 ERROR를 발생시킬 것이다.
Query parameter는 해당하는 들어온 resource의 해당하는 데이터가 없는 경우, 따로 에러 핸들링을 해야한다.

**즉, resource의 식별이 필요한 상황이라면 Path variable이 더 적절하다.**

이번엔 정렬이나 필터링을 해야되는 상황을 가정해보자.
만약 게시판에서 명지대와 관련된 글만 필터링 하는 예시는 다음과 같다.
```jsx
post/명지대
post?category=명지대
```
이 때 이 게시판에 명지대와 관련된 게시물이 없을 경우
**Path variable은 404를 일으키고, Query parameter는 빈 리스트를 반환할 것이다.**

필터링 시 404에러가 발생하는 것은 부적절할 수 있으므로  **정렬이나 필터링 시에는 Query parameter가 적절하다.**


