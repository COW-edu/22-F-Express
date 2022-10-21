# TEAM 민수 - 5주차 RBF

# 동적 페이지

- 여러번 사용되는 페이지 레이아웃


페이지의 URL이 고정되어 있는 정적 페이지와 다르게 사용자와 상호작용을 통해 받은 파라미터에 따라 다른 페이지를 출력하는 웹 페이지를 말한다. 사용자가 고유한 콘텐츠를 생성할 수 있다.

# URL

동적 페이지를 만들기 위해서는 각 항목을 고유하게 식별하는 입력란이 필요합니다. 이 입력란에 URL을 추가하여 사용합니다.

> http:www.../Recipes/Title

컬렉션을 만들면 Title 입력란이 컬렉션에 추가되며 이 입력란이 항목 페이지를 만들 때 자동으로 URL에 추가됩니다.

Title이 아닌 다른 입력란을 사용하려면 URL에서 Title을 삭제하고 다른 입력란을 추가해야합니다.

컬렉션에 모든 항목에는 각 항목에 대해 고유한 기본 id 시스템 입력란이 있습니다. URL의 이 입력란을 사용하여 컬렉션의 각 동적 항목 페이지가 고유한지 확인할 수 있습니다.

<2개 이상의 입력란이 있는 항목 페이지 URL>

ex) 사이트 구성원의 정보를 표시하는 페이지
- 동일한 성을 가진 구성원이 둘 이상 존재 가능
- 성 입력란으로만 항목 페이지를 만드는 경우 동적페이지는 다른 멤버를 구별할 수 없다

- URL에 성, 이름 입력란을 모두 입력하게 하여 URL을 만들어야 함

## Category page URLs

- 동적 카테고리 페이지를 만들려면 URL에 하나 이상의 입력란을 추가해야 컬렌션에 있는 항목을 분류 가능

<예시>
1. 아침, 점심, 저녁 식사 여부에 따라 모든 요리법을 표시하는 한 페이지가 있고 URL에는 사이트 및 컬렉션 이름 (https: //www.../recipes)만 있다 

2. 아침, 점심, 저녁 요리법을 표시하는 페이지를 설정하기 위한 식사 입력란 추가 (http://www.../Recipes/meal)

3. 둘 이상의 입력란을 사용할 수도 있으니 두 입력란을 모두 기반으로 콘텐츠를 만드는 동적 카테고리 페이지 만들기 (http://www.../Recipes/meal/course)

--> 레시피 : 컵케이크 / 식사 : 저녁 / 코스 : 디저트 와 같이 설정가능



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
static findById(id,cb){
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
