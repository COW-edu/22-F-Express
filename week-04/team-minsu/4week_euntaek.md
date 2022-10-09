4week_euntaek




우리가 백엔드 애플리케이션을 구축할때 코드를 구조화하는 득정 패턴을 따라야한다. ⇒ 패턴 : 다른 기능에 따라 코드를 논리적으로 나누는것

# MVC가 무엇인가

MVC : Model View Controller

모델, 뷰 그리고 컨트롤러와 관련되어 있음.

모델: 객체나 데이터를 나타내는 코드의 한 부분으로 데이터를 저장하거나 파일로부터 데이터를 주고받는 등 데이터 관련 작업을 할 수 있게 함.

뷰: 사용자 화면 관리

html자료를 렌더링해 사용자에게 전달 ⇒ 애플리케이션 코드와 분리되어 있으며 뷰를 생성하기 위해 템플릿 엔진에 주입하는 데이터와 통합되어 있음.

Controllers: 모델과 뷰 사이의 연결점

뷰는 애플리케이션 논리와 상관이 없음, 모델은 데이터를 저장하고 가져오는 역할을 함 ⇒ 컨트롤러가 모델과 함께 데이터 저장+ 프로세스 유발

컨트롤러는 중개인으로서 중간 논리를 포함한다.

작업할 모델과 렌더링할 뷰를 정의한다. ⇒ 패턴

# 컨트롤러 추가하기

우리가 만들고 있는 프로젝트에 컨트롤러 관련 폴더, 모델이 라우터 함수에 섞여있다. 라우팅을 할때 미들웨어 함수를 사용하는데 여기서 실행되는 논리는 컨트롤러 논리이다.

⇒ 데이터와 교류하고있고 뷰를 반환함. ⇒ 중간 논리

미리 컨트롤러 논리가 있기에 문제가 되지 않을것 같지만

routes파일에 모두 포함시키는것 보단 여러파일에 나눠 넣는 것이 좋다. ⇒ 자칫 큰 파일이 되는것을 막는다 + 라우트마다 실행하는 코드가 무엇인지 알 수 있다. + 해당하는 컨트롤러 파일과 함수에서 쉽게 알 수 있다.

### 링크추가

controllers 폴더의 products.js 파일에 아래와 같은 코드를 추가한다.

```jsx
exports.getAddProduct = (req, res, next) => {
  res.render('add-product', {
    pageTitle: 'Add Product',
    path: '/admin/add-product',
    formsCSS: true,
    productCSS: true,
    activeAddProduct: true
  });
};
```

미들웨어 함수 중 add-product 라우트를 렌더링하는 코드를

products.js 파일로 옮긴다. ⇒ 라우트 내부로부터 이 함수에 연결하는 것이기에 링크를 추가함.

제품 컨트롤러 파일에서 함수를 내보내기 하면 됨.

⇒export.

위 함수를 보면 요청 객체를 받고 사용할 응답 객체도 받고 있다.

⇒ 아직 일반적인 미들웨어 함수이기에 admin.js 파일 getAddProduct를 불러오자.

```jsx
import productController from '../controllers/products'
```

# 컨트롤러 마무리

컨트롤러로 404라우트를 처리할수있도록 만들어 보자.

새로운 컨트롤러를 생성하여 exports로 함수를 내보내 get404Page나 get404라고 이름을 붙이고 app.js에서 미들웨어 함수를 잘라내어 우리가 만든 파일의 get404에 저장하자.

```jsx
exports.get404 = (req, res, next) => {
  res.status(404).render('404', { pageTitle: 'Page Not Found' });
};
```

⇒ 404를 반환하는 함수

컨트롤러 불러오기

```jsx
import errorController from './controllers/error';
```

# 모델 추가

루트 프로젝트에서 새 폴더 생성후 Models라고 이름붙이자

컨트롤러, 뷰, 모델로 이제 mvc패턴이 완성됨.

새 파일 product.js를 추가하자.

모델은 원하는대로 정의가 가능하다

ex) 구축자 함수 내보내기

```jsx
module.exports = class Product {
  constructor(t) {
    this.title = t;
  }
}
```

위와같이 클래스를 기반으로 객체를 생성함으로 new로 호출하게될 구축자로 title을 전달할 수 있게 됨.

제품 배열에 제품 생성 + 저장 + 불러오기

```jsx
module.exports = class Product {
  constructor(t) {
    this.title = t;
  }

  save() {
    getProductsFromFile(products => {
      products.push(this);
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

모델완성

# 모델을 통한 데이터 저장

1) 파일 시스템 작업이니 코어fs 모듈에서 fs를 import

2) 특별한 경로에서도 작동하는 경로 구축

```jsx
import path from 'path';
```

3) save에 경로생성

```jsx
save(){
	const p = path.join(
		path.dirname(process.mainModule.filename),
			'data',
			'products.json'
	);
	fs.readFile(p, (err, fileContnet)=>{
		let products = [];
		if(!err){
			products = JSON.stringify(products), (err) => {
				console.log(err);
		});
	});
}
```