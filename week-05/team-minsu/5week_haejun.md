# Dynamic Routing
- 경로 설정을 동적으로 관리하는 방법
### 동적 경로 설정하기
- 콜론(:)을 사용하여 식별자를 선언하면 된다
- Express는 자동으로 요청을 redirect해야하는 subpath를 결정한다
- 예시
```js
router.get('/:name', function(req, res, next) {
  var name = req.params.name;
  var user = data.filter(u => u.name == name );
  return res.json({ message: 'Users Show', data: user });
});
```
  - 경로를 : 뒤 params으로 받아 사용한다

# Static Routing
- routing table에 경로를 수동으로 추가하여 사용한다
- Dynamic routing에 비해 초기설정이 많지만 관리자만 라우팅이 허용되기에 보안이 좋다

# Query Params
- params는 : 뒤의 내용물을 말함
- &을 사용해 여러 query params 사용이 가능하다
### 예시
- http://localhost:3000/users/34/books/8989 인 경우
> Route path : /users/:userId/books/:bookId
> req.params : {"userId":"34", "bookId":"8989"}
- userId, bookID는 Route Parameters이다
# Query string
- /search?searchWord=cow 인 경우
- dict 형식으로 query에 저장되어 있는 형태이다
  - searchWord라는 key값
  - cow라는 value값을 가진다
  - req.query.search 를 입력 시 "cow"가 나온다
### 예시
```js
router.get('/artists', function (req, res) {
  console.log("이름은 " + req.query.name + " 입니다")
  res.send("name : " + req.query.name)
});
```
--> /artists/name=cow