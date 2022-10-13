# 섹션 7

### MVC란?

관심사를 분리하는 것. 코드의 여러 부분들이 각기 다른 기능을 가져서 어떤 부분이 어떤 작업을 책임지는지 한눈에 알게 함.

- 모델 : 객체나 데이터를 나타내는 코드의 부분. 데이터를 저장하거나 파일을 주고 받는 등 데이터 관련 작업을 할 수 있게 함.
- 뷰 : 사용자가 보게 되는 화면을 책임짐. 어플리케이션 코드와 분리되어 있으며 뷰를 생성하기 위한 템플릿 엔진에 주입하는 데이터와 아주 약간만 통합되어 있음.
- 컨트롤러 : 모델과 뷰 사이의 연결점.

### 컨트롤러  만들기

```jsx
//controllers 폴더 안의 products.js 파일
//routes 폴더의 admin.js 파일에서 긁어온 middleware 함수
exports.getAddProduct = (req, res, next) => {
	res.render('add-product', {
	pageTitle: 'Add Product',
	path: '/admin/add-product',
	formsCSS: true,
	productCSS: true,
	activeAddProduct: true
	});
};

//routes 폴더의 admin.js 파일에서 위의 코드를 긁어내고 남은 자리에서 가져옴
//실행하지 않도록 뒤에 괄호(getAddProduct())는 생략 -> 함수에 참조만 전달
exports.postAddProduct = (req, res, next) => {
	products.push({title: req.body.title});
	res.redirect('/');
};

//admin.js 파일의 product 배열도 일단 긁어옴. -> 차후 제거
const Product = require('../models/product');

//shop.js 파일에서도 동일함. (제품관련 함수를 긁어옴.)
exports.getProducts = (req, res, next) => {
	//const products = adminData.products;
	//product 배열을 이동해 두어서 더이상 다른 곳에서 받아올 필요 없으므로 삭제

	res.render('shop', {
	prods: products,
	pageTitle: 'Shop',
	path: '/',
	hasProducts: products.length > 0,
	activeShop: true,
	productCSS: true
	});
}
```

```jsx
//routes 폴더의 admin.js 파일
//코드를 나눈 부분에 내보내기 함수를 사용해야함. 
const producsController = require('../controllers/products');

router.get('/add-product', productsController.getAddProduct)
router.get('/add-product', productsController.postAddProduct)

//admin.js 파일에 더이상 product 배열이 없기 때문에 아래 부분도 삭제 
exports.routes = router;
exports.products = products;
//대신 shop.js 파일에서처럼 라우터 내보내기를 해야 함
module.exports = router;
```

```jsx
//app.js 파일. admin Data를 adminRoutes로 모두 수정
const adminData = require('./routes/admin/);
//수정
const adminRoutes = require('./routes/admin/);
```

```jsx
//shop.js 파일
const producsController = require('../controllers/products');

router.get('/', productsController.getAddProduct)
```

**[정리] 컨트롤러 만드는 법 (404 페이지 기준)**

- 무슨 컨트롤러를 사용할지 결정. (새로운 컨트롤러 또는 기존의 제품 컨트롤러)
    - 새 컨트롤러를 만듬. → 404 페이지는 product와  관련이 없기 때문
- 404는 어떻게 추출할 것인지 생각.
- 404 페이지 논리를 컨트롤러에 넣어 기존의 라우트를 연결하면 됨!
    - app.js 파일에서 미들웨어 함수 긁어와서 404.js 파일에 넣어줌 → 404 페이지 반환하는 함수
    - app.js에서 컨트롤러(404.js)를 불러옴.

### 모델 만들기

```jsx
//model 폴더 생성. product.js 파일 만들기.
moudle.exports = function Product() {}
//대신에
module.exports = class Prdoduct {}
//도 가능

const product = [];

module.exports = class Prdoduct {
	constructor(title) {
		this.title = title;
	}

	save() { //fuction 글자가 빠진.. 함수
		products.push(this); //배열에 제품을 넣음
	}

	static fetchAll() {
		return products;
	}
}
```

```jsx
//products.js 파일
//products 배열 제거 
const Product = require('../models/product');

exports.postAddProduct = (req, res, next) => {
	//products.push({title: req.body.title});
	//model 사용하기 때문에 제거
	const product = new Product(req.body.title);
	product.save();
	res.redirect('/');
};

exports.getProducts = (req, res, next) => {
	//const products = adminData.products;
	//product 배열을 이동해 두어서 더이상 다른 곳에서 받아올 필요 없으므로 삭제
	const products = Product.fetchAll();
	//여기서 product를 만들지 않기 때문에 정적 방법 사용
	//모든 제품 제공 받음
	res.render('shop', {
	prods: products,
	pageTitle: 'Shop',
	path: '/',
	hasProducts: products.length > 0,
	activeShop: true,
	productCSS: true
	});
}
```

### 모델을 통해 파일에 데이터 저장하기

```jsx
//model 폴더의 product.js 파일
const fs = require('fs');  
//모든 운영체제에서 작동하는 경로 구축
const path = require('path');

save() {
	//헬퍼함수 사용
	//json 형식으로 파일 저장
	const p = path.join(
		path.dirname(process.mainModule.filename), 
		'data', 
		'products.json'
	);
	fs.readFile(p, (err, fileContent) => {
		let products = [];
		if (!err) {
			//json 파일 읽고 그 내용을 알려줌
			products = JSON.parse(fileContent);
		}
		products.push(this);
		fs.writeFile(p, JSON.stringify(products), (err) => {
			console.log(err);
		});
	});
}
	
	static fetchAll(cb) {
		const p = path.join(
			path.dirname(process.mainModule.filename), 
			'data', 
			'products.json'
		);
		fs.readFile(p, (err, fileContent) => {
			if (err) {
				cb([]);
			}
			//항상 객체나 제품 목록을 반환 함
			//콜백 함수로 오류 해결
			cb(JSON.parse(fileContent));
		});
	}

```

```jsx
//data 폴더 생성 -> 데이터를 저장할 폴더
```