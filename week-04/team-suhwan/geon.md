# MVC (Model - View - Controller)

: 모델, 뷰, 컨트롤러 세 가지 역할을 나누어 개발하는 프로그래밍 방식

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/61519efa-57ea-4f23-aafa-97076e9beb21/Untitled.png)

- Model: 객체나 데이터를 나타내는 코드의 한 부분

데이터 저장이나 파일로부터 데이터를 주고받는 등의 작업을 할 수 있음

- Views: 사용자가 보게 되는 화면

html 자료에 올바른 내용을 렌더링해 사용자에게 보내는 역할

- Controllers: 모델과 뷰 사이의 연결점

컨트롤러는 모델과 함께 데이터 저장 역할을 하고 뷰에서 가져온 데이터를 전달하는 역할도 함

### 장점

- 비즈니스 로직과 사용자 인터페이스를 분리해 개발할 수 있음
- 설계 및 코드의 가독성, 관리성, 효율성이 좋다

### 컨트롤러 추가

이미 routes 파일에 컨트롤러를 구성하는 In-between Logic을 갖춘 코드가 있지만, routes 파일에 모든 것을 넣으면 용량이 너무 커지므로 controllers 파일을 따로 만들어 준다.

```jsx
const Product = require('../models/product');

exports.getAddProduct = (req, res, next) => {
  res.render('add-product', {
    pageTitle: 'Add Product',
    path: '/admin/add-product',
    formsCSS: true,
    productCSS: true,
    activeAddProduct: true
  });
};

exports.postAddProduct = (req, res, next) => {
  const product = new Product(req.body.title);
  product.save();
  res.redirect('/');
};

exports.getProducts = (req, res, next) => {
  Product.fetchAll(products => {
    res.render('shop', {
      prods: products,
      pageTitle: 'Shop',
      path: '/',
      hasProducts: products.length > 0,
      activeShop: true,
      productCSS: true
    });
  });
};
```

다음과 같이 routes파일의 admin.js와 shop.js에 있는 미들웨어 함수를 controllers 파일로 옮겨옴

```jsx
const productsController = require('../controllers/products');

const router = express.Router();

// /admin/add-product => GET
router.get('/add-product', productsController.getAddProduct);

// /admin/add-product => POST
router.post('/add-product', productsController.postAddProduct);

module.exports = router;
```

다음과 같이 routes 파일의 admin.js 에선 router.get 함수에 productsController 를 추가하는데 직접 실행하라는 것이 아니라 참조만 전달하는 것이다. router에 맞는 요청이 도달할 때마다 이 함수를 실행하게 된다.

### 모델 추가

- 빈 배열 추가

```jsx
const products = [];
```

제품을 저장할 빈 배열을 생성

- 구축자 함수

```jsx
module.exports = class Product {
  constructor(t) {
    this.title = t;
  }

```

제품에 대한 제목을 수신하고 컨트롤러 내부에서 생성하는 역할

- 저장 함수

```jsx
save() {
	  products.push(this);
    }
```

제품을 생성한 뒤 저장 함수를 통해 제품을 저장하는 역할

```jsx
static fetchAll() {
	  return products;
  }
}
```

```
// ../controllers/products.js
exports.getProducts = (req, res, next) => {
  Product.fetchAll(products => {
.
.
.

  });
};
```

controllers파일에 fetchAll 함수를 호출해 모든 제품을 가져와 products에 저장해 주는 역할

위와 같은 빈 배열에 제품을 저장하는 방법은 더 복잡한 모델, 필드, 메서드가 동원될 때 더 좋은 효과를 낼 수 있다. 들어오는 제품을 전부 모델이 처리하고 컨트롤러에서 신경을 쓰지 않아도 되기 때문이다.

- Quiz
    
    들어오는 JSON 형식의 파일을 받아 JavaScript 배열, 객체 또는 파일에 있는 내용을 돌려주는 메소드를 하나 적어보세요.
    
    Answer - Parse 메소드
    

질문 - 404 page ReferenceError
ReferenceError: C:\Users\sec\OneDrive\바탕 화면\nodejs\views\404.ejs:5
    3|
    4| <body>
 >> 5|     <%- include('includes/navigation.ejs') %>
    6|     <h1>Page Not Found!</h1>
    7|
    8| <%- include('includes/end.ejs') %>

C:\Users\sec\OneDrive\바탕 화면\nodejs\views\includes\navigation.ejs:4
    2|   <nav class="main-header__nav">
    3|       <ul class="main-header__item-list">
 >> 4|           <li class="main-header__item"><a class="<%= path === '/' ? 'active' : '' %>" href="/">Shop</a></li>
    5|           <li class="main-header__item"><a class="<%= path === '/admin/add-product' ? 'active' : '' %>" href="/admin/add-product">Add Product</a></li>
    6|       </ul>
    7|   </nav>

path is not defined