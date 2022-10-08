# section 7

## Model View Controller (MVC)

- 관심사를 나누는 것 .  어떤 부분이 어떤 작업을 책임지고 있는지 확실하게 나누어서 사용하는 것이다. (다른 기능에 따라 코드를 논리적으로 나누는 것. )
- Models : 기본적으로 객체나 데이터를 나타내는 코드의 한 부분으로 데이터를 저장하거나 파일로부터 데이터를 주고 받는 등 데이터 관련 작업을 할 수 있도록 한다.
    - model은 클라이언트에게 노출되지 않는다.
- Views : 사용자가 보게 되는 화면을 책임진다. 애플리케이션 코드와 분리되어 있으며 뷰를 생성하기 위해 템플릿 엔진에 주입하는 데이터와 아주 약간 통합되어 있다.  (애플리케이션 논리와 상관이 없음. )
- Controllers : 작업할 모델과 렌더링할 뷰 사이의 연결점이다. controller가 model과 함께 데이터를 저장하거나 저장 프로세스를 유발한다. ( + view에서 가져온 데이터를 전달하는 경우도 controller가 돕는다.  )
    - 미들웨어 기능들 간 분할됨 . 일부 논리가 분리되어 다른 미들웨어 기능으로 이동한다.
    - Routes : 어떤 경로에 따른 http 메소드에 따라 어떤 controller 코드를 실행할지 정의한다.
    
    ![Untitled](section%207%201e4a53e12cf44f4899dfaa2122b449ff/Untitled.png)
    
- 코드를 구조화하는 특정 패턴을 따라야 한다.

## Controller

- 컨트롤러를 작업할 파일을 생성한 뒤, 해당 파일에서 작업을 진행한다.
- 컨트롤러 함수에 링크를 추가해야한다.
- 내보낼 함수를 exprots를 써서 내보내준다.

```jsx
const Product = require('../models/product')

exports.getAddProduct = (req, res, next) => {
	res.render('add-product',{
	    pageTitle : 'Add Product',
	    path: '/admin/add-product',
	    formsCSS: true,
	    productCSS: true,
	    activeAddProduct: true
	    });
};
//해당 코드는 controllers 폴더에 product로 저장되어있는 코드 중 일부이다. 
```

```jsx
const productsController = require('../controllers/product');

router.get('/add-product', productsController.getAddProduct);
// 컨트롤러를 작업한 파일을 가져온 뒤 사용할 컨트롤러명을 입력해준다. 
// express routes에 이 함수를 저장하라고 알려주는 역할을 한다. 
// 괄호가 쓰이지 않고 사용이 될 경우 실행이 되지 않는다. 참조만 전달
```

## Models

- 단일 entity를 만드는 것을 목표로 한다. (제품의 모습, 제품이 보유한 영역, 이미지나 제목의 보유 여부 등이 코어 데이터이다. )
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
     fs.readFile(p, (err, fileContent)=> {
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
          fs.readFile(p, (err, fileContent)=> {
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