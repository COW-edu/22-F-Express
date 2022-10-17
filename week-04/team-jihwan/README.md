# TEAM 지환 - 4주차 RBF
# 4주차

## Model View Controller (MVC)

앱 애플리케이션과 백엔드 애플리케이션의 관심사를 나누는 것. 특정 패턴에 따라 코드를 논리적으로 나누는 것이다. 

- Model
- View
- Controller

## Model

- 어플리케이션의 정보, 데이터를 나타낸다. 데이터베이스, 상수, 초기화 값, 변수 등을 뜻 한다.
- 데이터를 저장하고, 불러오고, 최신으로 유지하는 등의 관리를 수행한다.
- 메모리, 파일, 데이터 베이스 등 어디서 데이터를 관리하더라도 항상 모델이 데이터를 담당하는 컴포넌트를 의미한다.
- 모델의 규칙
    1. 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다
        
        -즉, 화면 안의 네모 박스에 글자가 표현된다면, 네모 박스의 화면 위치 정보, 크기 정보, 글자 내용 등을 가지고 있어야 한다.
        
    2. 뷰나 컨트롤러에 대해서 어떤 정보도 알지 말아야 한다
        
        -데이터 변경이 일어났을 때 모델에서 화면 UI를 직접 조정해서 수정할 수 있도록 뷰를 참조하는 내부 속성 값을 가지면 안된다
        
    3. 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현해야만 한다
        
        -모델의 속성 중 텍스트 정보가 변경이 된다면, 이벤트를 발생시켜 누군가에게 전달해야 하며, 누군가 모델을 변경하도록 요청하는 이벤트를 보냈을 때 이를 수신할 수 있는 처리 방법을 구현해야 한다. 또한 모델은 재사용 가능해야 하며 다른 인터페이스에서도 변하지 않아야 한다
        

## View

- 사용자에게 표시되는 부분으로, input 텍스트, 체크박스 항목 등과 같은 사용자 인터페이스 요소를 나타낸다.
- 데이터 및 객체의 입력, 출력을 담당하며 html 자료에 올바른 내용을 렌더링 해 사용자에게 보내는 역할을 한다.
- View의 규칙들
    1. 모델이 가지고 있는 정보를 따로 저장해서는 안된다.
        
        - 화면에 글자를 표시하기 위해서, 모델이 가지고 있는 데이터를 전달받는데, 그 데이터를 유지하기 위해서 뷰 내부에 데이터를 저장하면 안된다. 단순히 박스를 그리라는 명령을 받으면, 화면에 표시만 하고 그 화면을 그릴 때 필요한 정보들은 저장하지 않아야 한다.
        
    2. 모델이나 컨트롤러와 같이 다른 구성요소들을 몰라야 된다.
        
        -어플리케이션 코드와는 분리되어 있으며, 데이터를 전달 받으면 화면에 표시해주는 역할만 가진다.
        
    3. 변경이 일어나면 변경 통지에 대한 처리 방법을 구현해야만 한다.
        
        -모델과 같이 변경이 일어났을 때 이를 누군가에게 알려줘야 하는 방법을 구현해야 한다.
        

## Controller

- 작업할 모델과 렌더링 할 뷰 사이의 연결점 기능을 한다.
- model과 함께 데이터를 저장하거나 저장 프로세스를 유발하며, view에서 가져온 데이터를 전달하는 경우에도 돕는 등, 사용자가 데이터를 클릭하고, 수정하는 것에 대한 ‘이벤트’들을 처리한다.
- 미들웨어 기능들 간에 분할되며, 일부 논리가 분리되어 다른 미들웨어 기능으로 이동한다.
- Routes: 어떤 경로에 따른 http 메소드에 따라 어떤 controller 코드를 실행할지 정의한다.
- 사용자가 데이터를 클릭하고, 수정하는 것에 대한 ‘이벤트’들을 처리하는 부분

- Controller의 규칙들
    1. 모델이나 뷰에 대해서 알고 있어야 한다
        
        -모델이나 뷰는 서로의 존재를 모르고, 변경을 외부로 알리고, 수신하는 방법만 가지고 있는데 이를 컨트롤러가 중재하기 위해 둘을 알고 있어야 한다
        
    2. 모델이나 뷰의 변경을 모니터링 한다
        
        -모델이나 뷰의 변경 통지를 받으면 이를 해석해서 각각의 구성 요소에게 통지를 해야한다
        
        -애플리케이션의 메인 로직은 컨트롤러가 담당하게 된다
        

![mvc](https://ifh.cc/g/YG6Omf.png)

## Controller 만드는 법

- 컨트롤러를 작업할 파일을 생성한 뒤, 해당 파일에서 작업을 진행한다.
- 컨트롤러 함수에 링크를 추가해야한다.
- 내보낼 함수를 exprots를 써서 내보내준다.

```jsx
//controllers 폴더 안의 products.js 파일
//routes 폴더의 admin.js 파일에서 긁어온 middleware 함수
exports const getAddProduct = (req, res, next) => {
	res.render('add-product', {
	pageTitle: 'Add Product',
	path: '/admin/add-product',
	formsCSS: true,
	productCSS: true,
	activeAddProduct: true
	});
};

//routes 폴더의 admin.js 파일에서 위의 코드를 긁어내고 남은 자리에서 가져옴
// 컨트롤러를 작업한 파일을 가져온 뒤 사용할 컨트롤러명을 입력해준다.
// express routes에 이 함수를 저장하라고 알려주는 역할을 한다.
// 괄호가 쓰이지 않고 사용이 될 경우 실행이 되지 않는다. 참조만 전달
exports const postAddProduct = (req, res, next) => {
	products.push({title: req.body.title});
	res.redirect('/');
};

//아래 부분은 model을 생성하며 차후 제거한다.
const Product = require('../models/product');

//shop.js 파일에서도 동일하다.
exports const getProducts = (req, res, next) => {
	//const products = adminData.products;
	//product 배열을 이동해 두어서 더이상 다른 곳에서 받아올 필요 없으므로 삭제한다.

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
//코드를 나눈 부분에 내보내기 함수를 사용해야한다.
const producsController = require('../controllers/products');

router.get('/add-product', productsController.getAddProduct)
router.get('/add-product', productsController.postAddProduct)

//대신 shop.js 파일에서처럼 라우터 내보내기를 해야 한다.
export default {
    router;
}
```

```jsx
//app.js 파일. admin Data를 adminRoutes로 모두 수정한다.
const adminData = require('./routes/admin/');
//수정
const adminRoutes = require('./routes/admin/');
```

```jsx
//shop.js 파일
const producsController = require('../controllers/products');

router.get('/', productsController.getAddProduct)
```

## Models 만드는 법

- 단일 entity를 만드는 것을 목표로 한다. (제품의 모습, 제품이 보유한 영역, 이미지나 제목의 보유 여부 등이 코어 데이터이다. )

```jsx

const product = [];

export default {
    constructor(title) {
		this.title = title;
	}

	save() { //fuction 글자가 없지만 함수와 동일하다.
		products.push(this); //배열에 제품을 넣는다.
	}

	static fetchAll() {
		return products;
	}
}
```

```jsx
//products.js 파일
//products 배열 제거한다.
const Product = require('../models/product');

exports const postAddProduct = (req, res, next) => {
	//products.push({title: req.body.title});
	//model 사용하기 때문에 제거한다.
	const product = new Product(req.body.title);
	product.save();
	res.redirect('/');
};

exports const getProducts = (req, res, next) => {
	//const products = adminData.products;
	//product 배열을 이동해 두어서 더이상 다른 곳에서 받아올 필요 없으므로 삭제한다.
	const products = Product.fetchAll();
	//여기서 product를 만들지 않기 때문에 정적 방법 사용한다.
	//모든 제품 제공 받는다.
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

- 파일에 데이터 저장하기.
    - fs 모듈을 import해서 가져온다.
    - 해당 파일은 어느 경로에서든 생성이 가능해야하기 때문에 모든 운영체제에서 작동이 가능한 경로를 구축해야한다. 그렇기 때문에 Path 모듈을 사용해준다.
    - parse() : 들어오는 JSON을 받아 자바스크립트 배열 , 객체 ,파일에 있는 내용을 우리에게 돌려주는 역할을 하는 메소드이다.

- JSON.stringfy() :  헬퍼 객체를 사용한다. 객체나 배열을 가져다가 JSON으로 변환하여 올바른 형식을 지니도록 하는 stringfy 메소드를 사용한다.

```jsx
fs.writeFile(p,JSON.stringfy(products));
// 남아있는 작업은 파일로 다시 저장해줘야 한다. 그렇기 때문에 다시 파일 시스템 모듈을 사용한다.
// writeFile을 사용하여 읽은 경로와 동일한 경로에 작성하고 json 데이터를 배치한다.  
```

- 오류가 발생하는 경우를 대비해서 콜백함수를 입력해준다.
- 데이터를 저장소에서도 가져올 수 있어야한다. →fetchAll() 안에도 파일을 읽을 수 있도록 해준다.
    - 에러가 존재한다면 빈 배열을 반환하도록 해주고, 에러가 존재하지 않는다면  JSON.parse(fileContent)를 활용해서 fileContent를 반환해준다. (배열로 반환이 된다.)
    - 파싱된 형태로 파일 콘텐츠를 반환하면 항상 객체나 해당 데이터 목록을 반환하게 된다.
- 데이터를 배열로 받았음에도 해당 데이터에 length가 없다는 오류가 생긴다.
    - 두 return 문 모두 데이터를 반환하고는 있지만, 해당 코드는 비동기식 코드이다.
    
    ```jsx
    static fetchAll() {
     fs.readFile(p, (err, fileContent) => {
    	if (err){
       return [];
       }
      return JSON.parse(fileContent);
    });
    };
    ```
    
    - fetchAll 메소드는 path,fs모듈의 콜백을 이벤트 이미터 레지스트리에 등록되여 실행이 된 뒤 끝나버린다.
    - return문이 바깥쪽 함수가 아닌 안쪽 함수에 포함되어 있기 때문에 fetchAll이 아무것도 반환하지 않게 되는 문제가 생기는 것이다.
    - 따라서 shop.ejs 파일의 조건문에서  (if (prods.length >0) prods의 length에 접근을 하려고 하면 정의되지 않은 length에 접근하므로 에러가 발생하는 것이다.
    - 해결 방법 : fetchAll에 콜백 인수를 포함시킨다.
        - 해당 콜백을 호출하게 되면 fetchAll을 호출하는 컨트롤러로 이동하고 (controllers/product.js 의 exprots.getProducts)  , 함수 내부로 전달해야 하는 함수를 통해 데이터를 받게된다.

```jsx
static fetchAll(cb) {
 const p = path.join(
  path.dirname(require.main.filename), 
  'data',
  products.json
);
  fs.readFile(p, (err, fileContent) => {
  if (err){
   cb([]);
  }
  cb(JSON.parse(fileContent));
});
}
```

- 최종.

```jsx
const p = path.join(
  path.dirname(require.main.filename), 
  'data',
  'products.json'
  );
// path.dirname 은 루트 디렉토리이며, 이 안에 데이터 폴더를 미리 생성함으로써 
//권한 문제를 미리 방지해야한다. products는 파일이름.
const getProductsFromFile = cb => {
  fs.readFile(p, (err, fileContent)=> {
  if(err){
      return cb([]);
  } else {
      cb(JSON.parse(fileContent));
  }
  });
};
// fileContent에 대응하는 값이 JSON이기 때문에 형식을 JSON으로 저장해준다. 

module.exports = class Product {
 constructor(t) {
	this.title = t;
	}// 생성자 
 save() {
	getProductsFromFile(products => {
   products.push(this);
   fs.writeFile(p,JSON.stringify(products), err => {
    console.log(err);
	    });
	  });
  }
static fetchAll(cb) {
 getProductsFromFile(cb);
}
}
```
# MVC 패턴 외에 다른 패턴 

## MVP : Model View Presenter

- Model , View는 MVC 모델과 같다. MVC에서 Controller가 하는 역할을 Presernter가 한다고 생각하면 된다.
    - MVP패턴은 MVC 패턴의 단점이었던 view와 model의 의존성을 해결했다. (presenter을 통해서만 데이터를 전달받기 때문이다.)
- Presenter : Model과 View 사이의 매개체이다. Model과 View를 매개한다는 점에서 Controller와 유사하지만, View에 직접 연결되는 대신 인터페이스를 통해 상호작용한다는 차이가 있다.
    - 인터페이스를 통해 상호작용하므로 MVC가 가진 모듈화/유연성 문제 해결이 가능하다.
    - View에게 표시할 내용 (Data)만 전달하며 어떻게 보여줄 지는 View가 담당한다.
- 장점
    - View와 Model의 의존성이 없다. (둘의 결합도를 낮추면, 새로운 기능 추가 및 변경을 할 때 관련된 부분만 코드를 수정하면 되기 때문에 확장성이 개선된다. )
    - View와 Model의 역할이 분명해진다.
- 단점 : view와 presenter 사이의 의존성이 높다는 단점이 생긴다. 앱이 복잡해질수록 view와 presenter 사이의 의존성이 강해진다.

![Untitled](https://ifh.cc/g/QYtpjX.png)

## MVVM
- MVVM 패턴은 모바일 개발에서 흔히 사용되는 디자인패턴인데, 웹 프론트엔드도 복잡해지면서
이 패턴을 차용하는 프로젝트가 생겼다.
- MVVM은 Model + View + View Model 를 합친 용어이다.
    - View와 Model은 MVC에서의 역할과 같다
    - View Model은 View를 표현하기 위해 만든 View를 위한 Model이다.
    - View에 들어온 input들을 View Model이 Model과 View 사이에서 데이터를 가공하고 주고받는다.
- container 내부에서 함수를 선언하고 조합하는 코드가 View Model로 빠져 좀 더 효율적으로 코드를 관리할 수 있으며, 추후 메모이제이션 등을 적용할 때 편리하다