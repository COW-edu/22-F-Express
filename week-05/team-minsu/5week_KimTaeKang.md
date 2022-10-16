# 동적 페이지

### 페이지 URL이 고정된 데이터로 구성된 정적 페이지와 반대로, `사용자와 상호작용 하여 전달받는 파라미터에 따라 다른 데이터를 출력하는 웹페이지를 말한다.`

### 즉 , 페이지명은 같지만 그 뒤에 설정되는 변수 값(파라미터)에 따라 각각 다른 페이지가 보여진다.

<br>

### 예를 들면 쇼핑몰의 상품 정보 조회페이지가 그러하다. '상품 조회'라는 속성은 같지만, 각각의 상품들에 따라 다른 페이지가 클라이언트에게 전송된다.

<br>

### 그러면 어떻게 파라미터값에 따라 웹페이지를 출력해줄 수 있을까?

<br>

### 쇼핑몰을 예시로 들자면, 가장 간단한 방법은 각 상품마다 url 주소 뒤 파라미터 값을 다르게 설정하여, 이에 따라 각각 다른 웹페이지를 출력해주는 것이다.

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

# req.body, req.params, req.query에 대해

## 📌 req.body

### JSON과 같은 데이터를 받을 때 사용한다.

```js
// axios로 요청보내기
await axios.({
  url: "http://localhost:4000",
  method: "POST",
  data: {
    title: "hello",
    content: "hello world",
  },
});
```

서버에서 받을 때에는 아래 설정을 해줘야한다.

```js
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

+) express 4.16.0버전 이후에는 bodyParser의 기능 일부가 express에 포함되었기 때문에 아래 코드처럼 사용해도 된다.

```js

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
위의 설정을 해주면 req.body로 값을 받을 수 있다.

exports.uploadPost = async (req, res, next) => {
console.log(req.body); // { title: "hello", content: "hello world" }
// ...
};
```

## 📌 req.params와 req.query

http://localhost:4000/post/1?name=kim 라는 url이 있고

서버 라우터가 다음과 같을 때

```js
router.get("/:id", function);
```

1. req.params

req.params의 값은 `{ id: 1 }`이다.

```js
exports.test = async (req, res, next) => {
  console.log(req.params); // { id: 1 }
  // ...
};
```

2. req.query

req.query의 값은 `{ name: "kim" }`이다.

```js
exports.test = async (req, res, next) => {
  console.log(req.query); // { name: "kim" }
  // ...
};
```

# Query String 과 Path Variable 비교 및 활용

Query String, 쿼리 스트링 활용하기
'명지대'라는 글자가 들어가는 특정 대학을 찾기 위해선 어떻게 해야 할까?

아래와 같이 쿼리스트링을 활용하면 된다.

## univRoute.js

```js
export default = fubction(app){
  const univ = require('../controllers/univController');
  const jwtMiddleware = require('../../../config/jwtMiddleware');

  app.get('/univ',univ.getUniv);
```

## univcontroller.js

```js
// 대학 조회
export default getUniv = async function(req,res){
  univName = req.query.univName;

  try {
    const showUniv = await univDao.Univ1();

    if(univName){
      const showUniv2 = await univDao.Univ2(univName);
      return res.json({
        isSuccess: true,
        code: 200,
        message: "대학 조회 성공",
        result: {Univ: showUniv2}
      });
      });
    }
  }
}
```

사용자가 쿼리 스트링 값을 입력하지 않고

/univ와 같이 요청을 하게 되면 모든 대학을 출력하고,

/univ?univName=명지대

와 같이 요청을 하게 되면 univName에 값이 들어가게 되어 그 대학만을 출력하게 된다.

univDao.js

```js
// 대학 쿼리스트링 검색 부분
async function Univ2(univName) {
  const connection = await pool.getConnection(async (conn) => conn);
  const Univ2Query = `
  select univId, univName, univAddress, univPhone, univEmail, univHomepage, univLogo, univIntro, univType, univLocation, univLatitude, univLongitude
  from Univ
  where univName like concat('%',?,'%');
  `;
  const [showUniv2] = await connection.query(Univ1Query, univName);
  connection.release();
  return showUniv2;
}
```

쿼리 스트링 처리 부분 쿼리다.

**여기서 중요한 것은 쿼리를 짤 때 Like concat ('%',?,'%')을 사용해야 한다는 것이다**

<hr>

## **`Query String VS Path Variable`**

Path Variable은 특정 인덱스에 대한 조회.

Query String 은 특정 값으로 필터링.

Example) 아이디가 20번인 유저 조회 -> Path Variable 사용 -> GET /user/:userid

이름이 태강이고 20살인 유저 조회 -> Query String 사용 -> GET /user?userName=teakang&?age=20

만약 어떤 resource를 식별하고 싶으면 Path Variable을 사용하고 정렬이나 필터링을 한다면 Query Parameter를 사용하는 것이 가장 이상적이다.
