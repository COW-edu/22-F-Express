# Section 9

## 115. 모듈 소개

제품 ID와 같은 것을 url 일부로 입력해보자. url을 통해 데이터를 제출하거나 전송하는 방법과 요청 본문을 대신하는 방법을 알아보자. 또한, 라우트 매개변수 입력 방법과 쿼리 매개변수 사용 밥법도 배워보자. 이는 Express 라우터가 지원하는 중요한 기능으로, 모델을 강화할 수 있다.

## 118. 경로에 제품 ID 추가

1. Details이라는 버튼 만들기. 경로는 /products/infomation
<a href=”/products/<%= [product.id](http://product.id) %>” class=”btn”>Details</a>
2. ID 추가하자.
this.id = Math.random().toString();
3. data 폴더로 가서, 무작위 숫자를 넣어 ID 추가하자.
”id“: “12345”
4. 실행해보면, url에 id가 나타난(할당된) 것을 볼 수 있다. 

## 119. 동적 매개변수 추출하기

1. routes > shop.js 에서 새로운 라우트 추가하자.
router.get(’/products/:productId’);

콜론(:)은 Express에게 productId와 같은 라우트를 탐색하지 말라는 신호이다.
참고로, 더 구체적인 라우트를 뒤에 두면, 동적 세그먼트 취급을 받아 실행이 되지 않는다. 구체적일 수록 앞 줄에 작성하자.
2. controllers > shop.js에 상품 하나를 불러오는 새 컨트롤러를 추가해보자.

```jsx
exports.getProduct = (req, res, next) ⇒ {
	const prodId = req.params.productId;
	console.log(prodId);
	res.redirect('/');
}
```

1. routes > shop.js로 돌아가 함수 연결하기
router.get(’/products/:productId’, shopController.getProduct);

## 120. 상품 상세 데이터 로드하기

1. models > product.js에서 제품 하나 불러오자.

```jsx
static findById(id, cb) {
	getProductsFromFile(products => { //파일 전체를 읽어야 하므로, 모든 제품 가져오기
		const product = product.find(p => p,id === id); //제품마다 ID가 있으므로, 원하는 product를 찾아 임시 변수에 저장
		cb(product); //참(원하는 product를 찾았음)이라면, product를 회신하여 상수에 저장되었으니, 콜백 실행
	});
}
```

1. controller > shop.js로 가서 불러오자.
Product.findById(prodId, product ⇒ {
    console.log(product);
}

## 122. POST 요청으로 데이터 전달하기

1. post에 id 정보 넣기
<input type=”hidden” name=”productId” value=”<%= [product.id](http://product.id) %>”>
2. routes > shop.js에서 post 요청을 받는 새로운 라우트 추가
routes.post(’/cart’);
3. controllers > shop.js 에 새로운 컨트롤러 함수 추가

```jsx
exports.postCart = (req, res, next) ⇒ {
	const prodId = req.body,productId;  //데이터 추출
	console.log(prodId);
	res.redirect('/cart'); //이제 get 라우트를 불러오고 Cart 페이지가 렌더링 된다.
};
```

1. routes > shop.js에서 controller 액션을 post 라우트와 연결
routes.post(’/cart’, shopController.getCart);
2. Add to Cart 버튼 마다 폼이 중복되므로, includes에 넣자.
includes에 add-to-cart.ejs 파일을 생성하여 폼 붙여넣기
이제, shop > index.ejs의 Add to Cart 버튼의 폼이 있던 자리에 includes 포함시키자.
<%- include(’../includes/add-to-cart.ejs’) %>
3. 하지만, product는 해당 루프 안에서만 사용 가능한 로컬변수이다. 루프에 포함된 include에서는 이 변수를 받지 못한다. include에 입력하기 위해서는, 객체를 입력하고 변수를 다시 추가해 주는 두 번째 인수를 추가하면 된다. 이제 product를 include에서 사용할 수 있다.
<%- include(’../includes/add-to-cart.ejs’, {product: product} ) %>

## 123. 장바구니 모델 추가하기

models에 cart.js 파일 생성하여 장바구니 모델 작성해보자.

//주석 참고

```jsx
const fs = require('fs');
const path = require('path'); //경로를 잘 구축할 수 있도록 경로 도우미 불러오기

const p = path.join(
  path.dirname(process.mainModule.filename),
  'data',
  'cart.json'
);

module.exports = class Cart {
  static addProduct(id, productPrice) { //추가하기 원하는 제품의 ID, 장바구니의 가격 productPrice
    // Fetch the previous cart
    fs.readFile(p, (err, fileContent) => { //cart.json파일을 읽어서 오류 또는 콘텐츠를 회신하는 콜백
      let cart = { products: [], totalPrice: 0 }; //빈 배열의 products, 전체 수량
      if (!err) { //에러가 없다면, 기존 카트 존재.
        cart = JSON.parse(fileContent);
      }
      // Analyze the cart => Find existing product //제품이 이미 존재하는 지 확인
      const existingProductIndex = cart.products.findIndex( //기존의 product에서 existingProduct가 어디 index에 있었는지 알아내기
        prod => prod.id === id
      );
      const existingProduct = cart.products[existingProductIndex];
      let updatedProduct;
      // Add new product/ increase quantity
      if (existingProduct) { //기존 제품이 이미 있다면
        updatedProduct = { ...existingProduct }; //새로운 제품 생성. 차세대 JavaScript 객체 스프레드 연산자를 사용.
        updatedProduct.qty = updatedProduct.qty + 1; //수량 늘이기
        cart.products = [...cart.products]; //기존 배열 복사
        cart.products[existingProductIndex] = updatedProduct; //existingProductIndex 덮어 쓰기, updatedProduct로 교체
      } else { //신규 제품이라면
        updatedProduct = { id: id, qty: 1 }; //id 작성, 수량은 1
        cart.products = [...cart.products, updatedProduct]; //기존 cart.products가 있는 배열 얻기, 새로운 product 추가
      }
      cart.totalPrice = cart.totalPrice + +productPrice; //장바구니의 가격 갱신
      fs.writeFile(p, JSON.stringify(cart), err => { //구현
        console.log(err);
      });
    });
  }
};
```

## 124. 쿼리 매개변수 사용

실제 ID를 가져와 URL에 반영하여 상품에 대한 데이터로 형식 채우기와 Save 버튼을 누르면 기존의 것 편집하는 것을 해보자.

1. Admin 라우틀 가서 새로운 라우트 추가하기
router.get(’/edit-product/:productId’, adminController.getEditProduct)
2. controllers > admin.js > exports.getEditProduct 에서 쿼리 객체가 있는 지 확인하는 코드 작성 
const editMode = req.query.edit;
editing: editMode
3. editMode가 아니(false)라면, 리다이렉트
if (!editMode) { 
    return res.redirect(’/’);
}

## 125. 데이터로 제품 편집 페이지 채우기

제품 데이터 불러오기
const prodId = req.params.prodictId

## 126. 편집 페이지에 연결하기

1. 라우트를 통해 edit-product 페이지에 도달하기 위해 admin의 products 페이지에는, 추가된 제품과 제품의 ID를 포함해야 한다.
<%= [product.id](http://product.id) %>
2. 쿼리 매개변수도 첨부해주어야 한다.
<%= [product.id](http://product.id) %>?edit=true
3. Update Product를 클릭했을 때도 뭔가 하도록 해보자.
admin.js에서 라우트 등록
router.post(’/edit-product’);
4. controllers에도 작업
exports.postEditProduct = (req, res, next) ⇒ { }

## 127. 제품 데이터 편집하기

새로운 제품을 구축하고, 기존의 것을 이 제품으로 대체

1. constructor에 새 제품 생성 ID 작성
2. save에 ID가 존재하는지 확인

```jsx
save() {
    getProductsFromFile(products => { //모든 제품 확보
      if (this.id) { //ID가 있는 경우, 기존의 것을 업데이트
        const existingProductIndex = products.findIndex( //편집하고 싶은 제품의 인덱스 찾기
          prod => prod.id === this.id
        );
        const updatedProducts = [...products]; //모든 제품 요소 배열 생성
        updatedProducts[existingProductIndex] = this; //this(편집하고 싶은 새로 생성된 제품)으로 교체 
        fs.writeFile(p, JSON.stringify(updatedProducts), err => { //저장하고 해당 정보를 파일에 입력
          console.log(err);
        });
      } else { //null(ID가 없는 경우)이면 새로운 제품 생성
        this.id = Math.random().toString();
        products.push(this);
        fs.writeFile(p, JSON.stringify(products), err => {
          console.log(err);
        });
      }
    });
  }
```

1. controllers > postAddProduct 의 첫번째 인수를 null로 저장하자.
왜냐하면 생성자에 this를 추가 인수로 방금 추가했기 때문이다. 이 부분이 null이어야 확인에 실패하여, 새로운 제품 생성 모드로 진입하기 때문이다.
2. views > edit-product.ejs에서 기존 제품 ID를 저장하는 숨겨진 코드 작성
3. controlles > admin.js > postEditProduct 메서드에서 제품에 대한 정보 가져오고, 새로운 제품 인스턴스 생성하고 save 호출해보자.
사용자가 입력한 폼의 내용을 전부 상수로 받아 요청 바디에 엑세스하자.
컨트롤러로 응답을 전달하자.
4. admin.js 라우트로 가서 post 라우트에 새로 추가한 postEditProduct 활동을 등록하자.

## 128. 제품 삭제 기능 추가

제품 삭제와 Cart 뷰에서 장바구니에 담은 제품을 보거나 삭제해보자.

1. delete-product 라우트 추가
2. hidden 입력에 ejs 템플릿 구문으로 [product.id](http://product.id) 값, name은 productId 추가
3. controllers에서 exports.postDelete로 제품 삭제 액션 추가, productId 정보 얻기
4. 라우트에 액션 연결
5. product.js에 delete 메서드 추가
6. 모든 제품 호출
7. 제품의 인덱스 찾는 것 보다 빠른 방법인, filter 메서드를 사용해보자.
삭제하려는 ID와 다른 ID를 가진 모든 요소를 유지해서 파일에 새로운 배열로 쓰이겠끔 한다.
prod.id ! == id 라 작성해야한다. 왜냐하면, ID가 다르면 true 값이 반환되면서 해당 요소들은 유지되고 찾고자 하는 하나의 제품만 false 값이 반환되며서 새로운 배열에 유지되지 않기 때문이다.
8. 이미 삭제한 제품을 제외하고 업데이트된 제품을 저장하기 위해 fs.writeFile의 해당 경로에 대해 updateProducts를 JSON 형식으로 추가하자.
9. 콜백 함수를 통해 오류가 있는지 확인하자. 오류가 나지 않는다면, 장바구니에서 해당 제품 제거하는 코드를 작성해나가보자. 

## 129. 장바구니 항목 삭제

1. models > cart.js에서 새로운 정적 메서드 deleteProduct 추가하자. 삭제하고자 하는 제품의 id와 price 작성하자.
2. readFile장바구니 정보 받기
    1. if 오류가 나면 그냥 return; 삭제할 제품이 없다는 뜻이니 그냥 두면 된다.
    2. 기존 장바구니의 모든 속성을 새로운 객체로 받기
    3. 삭제하려는 제품의 ID와 일치하는 ID 제품 찾기 
    4. 제품 수량을 찾아 확인하기
    5. updatedCart.products에 filter를 입력해 모든 제품 중에 반환 값이 true인 요소들만 유지하자.
    제거하는 제품 하나만 배고 모든 제품이 반환되도록 [prod.id](http://prod.id) ! == id
    6. 장바구니의 총 가격 연산하기
    7. writeFile 작성
3. products 모델에서 장바구니 모델 불러오기
4. ID로 제거하기 전 제품 정보 얻고, 삭제하기
5. controller의 admin.js에서 postDeleteProduct에 prodId를 넘기고 리다이렉트

## 130. 장바구니 페이지에 장바구니 항목 표시하기

cart.ejs에서 장바구니 항목을 보자.

1. cart.js에서 모든 제품 정보를 콜백 함수를 사용하여 제공하자
2. shop.js > getCart에 콜백 함수를 전달하자. 뷰도 렌더링하자.
3. 장바구니 제품 필터링하자.
for로 모든 제품 루핑하여 특정 제품이 장바구니에 있는지 ID로 확인하자.
4. 배열을 만들어, 찾은 제품을 넣자
객체에 product를 가진 productData 필드와 qty 필드도 추가하자. 
5. 이제 cartProducts를 반환하기 위해 뷰로 보내자.
6. cart.ejs 파일에 가서 if문으로 products 보여주자.
else에는 장바구니 제품이 없는 경우 랜더링할 페이지 작성

## 131. 장바구니 항목 삭제

1. form 형태의 button 생성
2. cart-delete-item 라우터 작성
3. shop.js에 getCartDeleteProduct 작성

## 132. 제품 삭제 버그 수정

장바구니에서 해당 제품이 있는지 확인하는 과정이 없어서 버그가 발생했었다.
if (!product)를 검사하여 true 값이 반환되면 제품이 없다는 뜻으로 그냥 return으로 마무리하면 된다.