# TEAM 민수 - 5주차 RBF

# 동적 페이지

- 여러번 사용되는 페이지 레이아웃
- 사용자와 상호작용을 통해 받은 파라미터에 따라 다른 페이지를 출력하는 웹 페이지 
- 사용자가 고유한 콘텐츠를 생성할 수 있다
- query parameter에 따라 다른 검색 결과를 보여준다


# Dynamic Routing

**의미**: 라우트 경로에 특정 값을 넣어 해당 페이지로 이동 할 수 있게 하는 것.

Express 라이터 경로에 :(콜론)을 추가함으로써 동적 경로 세그먼트를 전달할 수 있다.

:(콜론) 다음에 오는 이름 식별자로서 req.params에서 데이터를 추출할 수 있다.

Express는 자동으로 요청을 redirect해야하는 subpath를 결정한다

**—참고로 한 라우트는 여러개의 세그먼트를 가질 수 있다.**

## Dynamic Web
- 사용자와 상호작용 하여 전달받는 파라미터에 따라 다른 데이터를 출력하는 웹페이지를 말한다.
  - 즉 , 페이지명은 같지만 그 뒤에 설정되는 변수 값(파라미터)에 따라 각각 다른 페이지가 보여진다.
-  실시간으로 데이터 베이스와 연동이되어 이에대한 정보는 클라이언트에 제공하기도 하기때문에 웹 페이지를 열때 매번 새로운 내용을 표시해주고 변경도 가능하다. 
- 동적 프로그래밍의 경우 수정사항이 있으면 서버를 리부팅 해야한다.

# Static Routing
장점 : 관리자의 의도대로 제어할 수 있다. 

경로 정보를 주고 받을 필요가 없어 효율이 높다.

단점: 경로를 설정하고 유지하는데 공수가 든다.

적절한 경로 구현을 위해 관리자의 이해도가 필요하다.

네트워크 규모가 커지면 힘들다.

# Static Web
 - 이미지, css, js파일등 컨텐츠를 바로 전달한다. 
 - 정적 프로그래밍은 정적으로 코드를 구성하면 서버를 재시작하지 않고 출력결과를 즉시 반영이 가능하다.
<br>
⇒ 웹페이지가 변경되거나 수정되지 않는 한 언제나 고정된 웹 페이지만 볼 수가 있다.

# Dynamic Web 더 자세히 알아보기 / 예시 : 쇼핑몰
- parameter 값에 따라 웹페이지를 다르게 출력하기 위해 각 상품마다 parameter 값을 다르게 설정하고, 이에 따라 각각 다른 웹페이지를 출력해보는경우

<hr>

이를 간략하게 설명하자면 아래와 같다.

data를 저장할 때 해당 id 값을 설정해준다.
클라이언트가 특정 상품을 클릭하면 해당 상품의 id값이 url 파라미터로 전달된다.

백엔드에서는 전달받은 파라미터(id)에 해당하는 상품을 데이터 영역에서 찾고, 그 상품과 관련된 정보를 전송해준다.
이를 하나하나 살펴보도록 하겠다.

1. id값 설정은 Models영역에서 처리해준다. 일단은 특정 데이터를 저장할 때 id값을 랜덤으로 부여해서 저장한다.

```js
save(){
this.id = Math.random().toString(); //id값 저장
getProductfromFile(products=>{
products.push(this);
fs.writeFile(p, JSON.stringify(products), (err)=>{
console.log(err);
});
});
}
```

+id 중복체크하기
- checkid와 같은 함수를 만들어 중복확인을 해줄 수 있다
- 요청아이디와 새 아이디가 같은지 


2. id값이 파라미터로 전달 되는 것은 views영역에서 처리해준다. 아래는 상품 리스트 페이지의 버튼 부분이다. detail 버튼을 클릭하면 /products+(해당 상품의 id값) 이 파라미터로 전송된다.

```html
<div class="card__actions">
   a href="/products/<%= product.id%>" class="btn">Details</a>
 </div>
```

3. 일단 위와 같이 /products+(동적 파라미터) path를 받아서 라우팅을 하기 위해서는 라우팅할 path를 '/ products/:productId' 라고 설정해야 한다.

이렇게 하면 뒤에 :productId는 동적 파라미터를 담게 되어 req.params.productId 로 접근 가능하다.

route.get('/products/:productId',shopController.getProduct);
이렇게 전달받은 동적 파라미터에 해당하는 데이터를 모든 데이터가 저장된 곳에서 찾아야한다. 이는 데이터를 다루는 Models영역에서 처리해주어, 추출한 데이터를 render해주는 Controller 영역에 전달해준다.

Controller 영역에서는 Models영역의 findById 즉, 해당 id 값과 일치하는 데이터를 찾아주는 메서드의 인자로, id값과 , 콜백함수를 넣어주고, id값에 해당하는 데이터를 찾은 후 , render해주도록 한다.

```js
export default getProduct = (req, res, next) => {
  const prodId = req.params.productId; //파라미터 추출
  Product.findById(prodId, (product) => {
    res.render("shop/product-detail", {
      //동적 페이지 render
      productInfo: product,
      path: "/products",
      pageTitle: "product detail",
    });
  });
};
```

Models 영역에서 전달받은 id값과 일치하는 데이터를 찾아주는 findById 메서드는 일단 데이터가 저장된 json파일로부터 전체 데이터를 array형태로 읽어온 후, 자바스크립트의 find() 메서드를 이용해서 전달받은 id에 해당하는 데이터를 찾찾아, 인자로 전달 받은 콜백함수를 실행한다.

```js
static getProductById(id,cb){
getProductfromFile(products=>{
const product = products.find(p => p.id === id);
// p.id === id 가 true이면 현재 보고있는 p (products 중에 하나) 를 return 하여 product 변수에 저장
cb(product);
})
}
const getProductfromFile = cb =>{ //전체 데이터 읽어서 parse해주는 함수
fs.readFile(p,(err,fileContent)=>{
if(err){
return cb([]);
}
cb(JSON.parse(fileContent));
});
}
```



# Query parameter & Path variable

## Query parameter

경로 뒤에 입력 데이터를 함께 제공하는 식으로 사용한다.

URL 마지막에 ?를 덧붙인 매개변수이다.

## Query paramter로 배열 받기
1. &을 여러번 사용하여 배열을 받는다
2. nestjs 내장 기능을 이용하여 자동으로 query key를 배열로 만들어 준다
3. ?cars[]=Saab&cars[]=Audi (Best way- PHP reads this into an array)

&를 통해 여러 개를 가질 수 있다.

req.query.{query string 키 이름}를 통해 값을 추출할 수 있다.

### 예시

http://ccccc.com/topic?id=1

각 의미하는것은 다음과 같다.

http: ⇒ hyper text transfer protocol

[ccccc.com](http://ccccc.com) ⇒ domain

topic ⇒ path

id=1 ⇒ query string

```jsx
//HTTP GET /search?q=김영환 으로 요청이 들어왔을때
const q = req.query.q //로 하면 "김영환"이라는 value를 얻을 수 있다.
```

## Path variable

이름에서도 알 수 있듯이 경로를 변수로 사용한다.

예)게시판이 존재하고 그 안에 각각의 게시물을 볼 수 있는 경우를 생각하자. 게시물을 볼라면 각각의 id를 서버에 넘겨줘야 한다. 
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

# Query String vs Request Body

- query string
  - 같은 리소스, 다른 동작
  - 일반적인 것, 디버깅 가능한 것
  - 인수가 디버깅하는 동안 보고 싶을 때
  - 코드를 개발하는 동안 예를 들어 curl로 수동으로 호출할 수 있기를 원할 때
  - 여러 웹 서비스에서 인수가 공통적인 경우
  - 이미 application/octet-stream과 같은 다른 콘텐츠 유형을 보내는 경우
- request body
  - 계층구조를 가질 때 혹은 리스트
  - 일반적인 것, 디버깅 가능한 것 외 나머지
  - 인수에 플랫 키: 값 구조가 없는 경우
  - 값이 사람이 읽을 수 없는 경우 (예: 직렬화된 이진 데이터)
  - 매우 많은 수의 인수가 있을 때
  - json과 같은 파일
