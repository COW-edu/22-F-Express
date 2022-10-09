# Section 7

# 모델 뷰 컨트롤러 (MVC)

## 97. MVC란?

코드의 여러 부분들이 각기 다른 기능을 가지는 것이다. Model View Controller의 약자로 Model, View, Controller로 이루어진다. Models은 데이터를 나타내고, 주고 받는 작업을 한다. Views는 사용자의 화면을 나타낸다. Controllers는 모델과 뷰의 연결점이다.

## 98. 컨트롤러 추가하기

router에 실행된 논리는 전형적인 컨트롤러 논리이다. 하지만 routes파일에 모든 것을 넣으면 크기가 커지기에, controllers 폴더를 만들어 여러 파일로 나눠 넣자.

1. controllers 폴더를 만들고, products.js 파일도 생성하여 제품 관련 논리를 옮겨보자.
2. 미들웨어 함수 중에 add-product 라우터를 렌더링 하는 코드를 옮기자.
3. exports를 사용하여 controllers에서 내보내자.
4. 이제는 사용하지 않게 된, const rootDir = require(’../utill/path’);는 삭제하자.
5. 이제 사용할 controller를 불러오기 위해, const productsController = require(’../controllers/products’);를 작성하자.
6. routes에서 받아오자(불러오자).
router.get(’/add-product’, products.Controller.getAddProduct);
7. 제품 추가 함수도 같은 과정으로 작성하자.
8. const products = []; 배열 코드도 controllers로 옮기자.
9. admin.js 파일에는 배열 코드가 없어졌으니 exports 코드도 필요없게 되었으니 삭제.
10. 대신 라우터를 내보내야 한다.
module.exports = router;
11. 이제, 이걸 불러오는 app.js 파일을 조정하자.
adminData가 아닌, adminRoutes
12. shop.js에서도 동일한 방식으로, 제품 관련 코드를 가져오자.
controllers 파일에는 배열이 있으므로 배열 추출 관련 코드는 삭제.
13. shop.js에서도 마찬가지로 rootDir과 adminData 삭제.
대신 불러오는 코드인 const productsController = require(’../controllers/products’); 작성.

## 99. 컨트롤러 마무리하기

404페이지도 컨트롤러로 작성해보자.

## 100. 제품 모델 추가하기

Controller와 View를 추가하였으니 이제 Models를 생성해보자.

1. models 폴더와 product.js 파일 생성하자.
주의) products.js라 하면 중복되니 안 된다.
2. 제품의 형태를 정의하는 클래스를 생성해보자.

```jsx
const products = []

module.exports = class Product { //제품의 형태를 정의하기 위한 구축자 함수를 생성하자.
		constructor(t) { //제품에 대한 제목 수신, 컨트롤러 내부에서 생성
				this.tilte = t;
		}
		save() {//제품 배열에 제품 생성(저장)하기
				products.push(this);
		}
		static fetchAll(){//배열로부터 모든 제품 회수하기
		//static을 사용하면, 예시된 객체가 아니라, 클래스 자체에 직접 호출하게 된다.
				return products;
		}
}
```

1. controllers > products.js 파일로 가서, 기존에 사용했던
const products = []; 삭제
products.push({ title: req.body.title }); 삭제
const products = adminData.products; 삭제
이제 이 파일에는 제품 배열 관련 논리가 없어졌다.
2. 새로운 상수인 Product를 추가하여 클래스를 임포트하자.
const Product = require(’../models/product’);
3. postAddProduct에서 클래스 설계도를 토대로 새로운 객체를 생성하자.
const product = new Product(req.body.title);
4. product.save를 호출하여 저장하자.
product.save();
5. getProducts에서는 모든 제품을 가져오도록 하자.
const products = Product.fetchAll();

## 101. 모델을 통해 파일에 데이터 저장하기

제품을 배열에 저장하는 것이 아닌, 파일에 저장해보자.

1. 배열을 삭제하고, 코어 fs 모듈에서 fs를 임포트하자.
const fs = require(’fs’);
2. 이 파일은 특별한 경로에서 생성되어야 하므로 Path 도구 즉 Path 모듈을 사용하여 모든 운영체제에서 작동하는 경로를 구축하자.
const path = require(’path’);
3. save를 호출할 때 파일에 저장하자.
products.push(this); 삭제하고
path 경로를 생성하고, 그 안에 새로 데이터 폴더를 미리 생성하여 권한 문제를 방지하자.
const p = path.join(path.dirname(procss.mainModule.filename), ‘data’, ‘products.json’);
4. 새 제품을 저장하려면 먼저 기존 제품 배열을 가져와야 한다.

```jsx
fs.readFile(p, (err, fileContent) ⇒ { //p 파일을 읽고, 파일 콘텐츠(오류나 데이터)를 가져오자.
    let products = []; //초기에는 공백 배열에 해당. 오류가 발생한는 경우, 공백
    if (!err) { //오류가 없다면
        products = JSON.parse(fileContent); //Parse 메서드로 제품 읽어오기
    }
    products.push(this); //새로운 제품 덧붙이기
    fs.writeFile(p, JSON.stringify(products), (err) => { //파일로 다시 저장
		//읽은 경로였던 p와 동일한 경로인 p에 작성
		//JSON 데이터에 배치. JSON으로 변환하여 올바른 형식을 지니도록 하는 STringify 메서드 사용.
        console.log(err); //오류가 발생하는 경우 콜백
    });
});
```

1. 데이터를 저장소에서 가져오자.

```jsx
static fetchAll(){
		const p = path.join(
				path.dirname(process.mainModule.filename),
				'data', 'products.json'
		);
		fs.readFile(p, (err, fileContent) => { //p 파일을 읽고, 파일 콘텐츠(오류나 데이터)를 가져오자.
				if (err) { //오류가 발생했다면, 제품이 없다는 뜻이므로
						return []; //공백 배열을 반환.
				}
				return JSON.parse(fileContent); //단순 문자열이 아니라, 텍스트로 회수되겠금 해주자.
		});	
}
```

fetchAll에서 데이터를 반환하고는 있지만 이건 비동기식 코드이다.

return 문은 바깥쪽 함수가 아니라 안쪽 함수에 포함되기에 fetchAll이 아무것도 반환하지 않는다. 그래서 오류가 발생한다. 수정해보자.

## 102. 모델을 통해 파일에서 데이터 가져오기

1. 콜백 인수(cb)를 포함시키자.

```jsx
static fetchAll(cb){
		const p = path.join(
				path.dirname(process.mainModule.filename),
				'data', 'products.json'
		);
		fs.readFile(p, (err, fileContent) => { //p 파일을 읽고, 파일 콘텐츠(오류나 데이터)를 가져오자.
				if (err) { //오류가 발생했다면, 제품이 없다는 뜻이므로
						cb([]); //공백 배열을 반환.
				}
				cb(JSON.parse(fileContent)); //단순 문자열이 아니라, 텍스트로 회수되겠금 해주자.
		});	
}
```

1. controllers > products.js의 fetchAll 함수 내부로 cb를 전달하자.

## 103. 파일 스토리지 코드 리팩토링

```jsx
const fs = require('fs');
const path = require('path');

//p를 파일 전체에서 사용할 수 있도록하자.
const p = path.join( 
  path.dirname(process.mainModule.filename),
  'data',
  'products.json'
);

const getProductsFromFile = cb => { //헬퍼 함수를 생성하고 상수에 저장하자.
//경로를 구축하고 파일을 읽어오는 역할을 한다.
  fs.readFile(p, (err, fileContent) => {
    if (err) {
      cb([]);
    } else {
      cb(JSON.parse(fileContent));
    }
  });
};

module.exports = class Product {
  constructor(t) {
    this.title = t;
  }

  save() {
    getProductsFromFile(products => { //products를 불러오는 새로운 익명 함수 생성
      products.push(this); //새로운 제품 추가
      fs.writeFile(p, JSON.stringify(products), err => {
        console.log(err);
      });
    });
  }

  static fetchAll(cb) {
    getProductsFromFile(cb);
  }
};
```