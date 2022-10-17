# TEAM 보겸 - 5주차 RBF
- 전체 rbf
    
    # Section 9
    
    ### 정적 웹이란?
    
    움직이지 않는, 언제 접속을 해도 같은 리소스를 주는 웹 사이트를 칭하는 말로 이미 파일들을 클라이언트 브라우저에 전달되어 서버에서 접속시 매번 가공해서 제공하는 것이 아닌 html css javascript 코드들이 그대로 전달되는 것
    
    ex) 소개 페이지, 댓글 없는 블로그글
    
    → 서비스가 한정적이지만 요청에 대한 파일만 전송하면 되기에 서버간 통신이 거의 없고 속도가 빠르다는 장점이 있다
    
    ### 동적 웹이란?
    
    서버에서 접속시 매번 가공해서 제공하는 웹사이트로 데이터베이스에서 값을 읽어 접속할때마다 최신 정보를 주는것
    
    [단순히 최신 정보를 주는 것은 정적 웹 사이트도 가능하다 그때마다 리소스를 업데이트 해주면 되니까. 하지만 동적이다라는 말에 초점을 맞춰서 생각해보자. 동적 웹 사이트는 데이터베이스에 존재하는 데이터에 의존하여 화면이 변화된다. 그러면 클라이언트가 데이터 베이스의 데이터를 생성, 수정, 삭제가 가능하고 그 점이 동적 웹의 가장 큰 장점이다.]
    
    ex) sns
    
    → 사용자에게 웹 페이지를 전달하기 전에 처리하는 작업이 필요하기에 상대적으로 느리지만 다양한 서비스를 제공하고 관리가 쉬움
    
    [https://velog.io/@pluviabc1/정적-웹과-동적-웹은-무엇인가요-얄팍한-코딩-사전](https://velog.io/@pluviabc1/%EC%A0%95%EC%A0%81-%EC%9B%B9%EA%B3%BC-%EB%8F%99%EC%A0%81-%EC%9B%B9%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-%EC%96%84%ED%8C%8D%ED%95%9C-%EC%BD%94%EB%94%A9-%EC%82%AC%EC%A0%84)
    
    ## 118. 경로에 제품 ID 추가 
    118과 같은 번호는 udmey 강의 번호입니다.
    
    1. Details이라는 버튼 만들기
    2. ID 추가
    
    ## 119. 동적 매개변수 추출하기
    
    1. 새로운 라우트 추가
    2. 상품 불러오는 controller 추가
    3. 라우트에 함수 연결하기
    
    ## 120. 상품 상세 데이터 로드하기
    
    1. models에서 제품 불러오기
        
        ```jsx
        static findById(id, cb) {
        	getProductsFromFile(products => { //파일 전체를 읽어야 하므로, 모든 제품 가져오기
        		const product = product.find(p => p,id === id); //제품마다 ID가 있으므로, 원하는 product를 찾아 임시 변수에 저장
        		cb(product); //참(원하는 product를 찾았음)이라면, product를 회신하여 상수에 저장되었으니, 콜백 실행
        	});
        }
        ```
        
    2. controller에서 불러오기
    
    ## 122. POST 요청으로 데이터 전달하기
    
    1. post에 id 정보 넣기
    2. post 요청 받는 새로운 라우트 추가
    3. 새로운 controller 추가
    
    ```jsx
    exports.postCart = (req, res, next) ⇒ {
    	const prodId = req.body,productId;  //데이터 추출
    	console.log(prodId);
    	res.redirect('/cart'); //이제 get 라우트를 불러오고 Cart 페이지가 렌더링 된다.
    };
    ```
    
    1. controller 액션을 post 라우트와 연결
    2. Add to Cart 버튼 마다 폼이 중복되므로, includes에 넣자.
    includes에 add-to-cart.ejs 파일을 생성하여 폼 붙여넣기
    이제, shop > index.ejs의 Add to Cart 버튼의 폼이 있던 자리에 includes 포함시키자.
    <%- include(’../includes/add-to-cart.ejs’) %>
    3. 하지만, product는 해당 루프 안에서만 사용 가능한 로컬변수이다. 루프에 포함된 include에서는 이 변수를 받지 못한다. include에 입력하기 위해서는, 객체를 입력하고 변수를 다시 추가해 주는 두 번째 인수를 추가하면 된다. 이제 product를 include에서 사용할 수 있다.
    <%- include(’../includes/add-to-cart.ejs’, {product: product} ) %>
    
    ## 123. 장바구니 모델 추가하기
    
    ```jsx
    const fs = require('fs'); //es6로 작성하려면, import fs from 'fs'
    const path = require('path'); //경로를 잘 구축할 수 있도록 경로 도우미 불러오기 //es6로 작성하려면, import path from 'path'
    
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
    
    1. 새로운 라우트 추가
    2. 쿼리 객체가 있는지 확인
    3. false라면, 리다이렉트
    
    # 쿼리스트링과 url파라미터
    
    **URL 파라미터 즉 Path Variale은 특정 인덱스에 대한 조회를 할 때 사용한다 ( 특정 객체를 조회할 때 사용)**
    
    예를 들어 id를 조회할 때 사용
    
    ```jsx
    router.get("/products/:productId", shopController.getProduct);
    ```
    
    ex) /products/:1 은 productId가 1인 product를 조회
    
    **Query String은 특정 값으로 필터링 ( 특정 값을 가진 객체를 조회)**
    
    예를 들어 /movies로 요청을 하면 모든 영화를 보여줌 하지만 쿼리 스트링을 사용해서 /movies?moviesName=”노트북”으로 요청을 하면 특정 값으로 필터링이 가능하다
    
    ex) 아이디가 2번인 유저를 조회한다면 path variable을 사용
    
    Get /user/:userid
    
    하지만 나이가 21살이고 이름이 가현인 유저를 조회한다면 query string을 사용해서 GET /user?userName=gahyun&?age=21
    
    - req.params는 (url 파라미터) 특정 인덱스를 조회할때 사용하는 요청
    - req.query는  특정 값을 필터링할 때 사용하는 요청
    - req.body는 json등의 전체 바디 데이터를 담을 때 사용
    
    ### req.params
    
    라우터의 매개변수
    
    명명되 경로 “매개변수”에 매핑된 속성을 포함하는 객체이다. 예를 들어 /student/:id 경로가 있는 경우 "id" 속성을 req.params.id로 사용할 수 있다. 이 객체의 기본값은 {}이다.
    
    ```jsx
    router.get('/:id/:name',(req,res,next)=>{
    console.log(req.params);
    });
    ```
    
    만약 이런 코드의 예시가 있다면 req.params를 출력했을 때 실제 요청온 url이 요청온 url : [www.example.com/public/100/jun](http://www.example.com/public/100/jun)라면  {id : ‘100’, name’jun’} 이 출력될것이다. 
    
    req객체에 parameter라는 프로퍼티가 있고 그 프로퍼티의 ‘id’라는 프로퍼티로 접근해 요청을 보낼 수 있는 것이다. 
    
    ### req.query
    
    경로의 각 쿼리 문자열 매개변수에 대한 속성이 포함된 개체다 (주로 get 요청에 대한 처리)
    
    → 주로 검색,정렬 , 필터링 , 페이지 매김 등에 사용한다
    
    예를 들어 [www.example.com/post/1/jun?title=hello](http://www.example.com/post/1/jun?title=hello)! 이면, title=hello! 부분을 객체로 매개변수의 값을 가져온다
    
    ```jsx
    // 요청온 url : www.example.com/public/100/jun?title=hello!
    app.use(express.urlencoded({ extended: false })); // uri 방식 폼 요청 들어오면 파싱
    
    router.get('/:id/:name', (req, res, next) => {
    
      //../100/jun 부분이 담기게 된다.
      console.log(req.params) // { id: '100', name: 'jun' }
      
      // title=hello! 부분이 담기게 된다.
      console.log(req.query) // { title : 'hello!' }
    });
    ```
    
    - express에서 쿼리 스트링을 인식하기 위한 request 메서드로 쿼리스트링을 객체 형태로 변환시켜준다
    
    예시 2)
    
    ```jsx
    // from(Front)
    
    fetch('http://localhost:5000/rests?filter[type]=hotel&page=1&limit=15'
    
    // to(Back)
    
    console.log(req.query)
    {
      filter: { type: "hotel" },
      page: 1,
      limit: 15,
    }
    ```
    
    이 예시처럼 req.query의 키벨류는 string 타입이기에 숫자의 경우 Number을 통해 number 타입으로 변환이 필요하다.
    
    ## 126. 편집 페이지 연결하기
    
    1. 추가된 제품과 제품의 ID 포함시키기
    <%= [product.id](http://product.id) %>
    2. 쿼리 매개변수도 첨부하기
    <%= [product.id](http://product.id) %>?edit=true
    3. Update Product 하기 위한 라우트 등록
    4. controllers 작성
    
    ## 127. 제품 데이터 편집하기
    
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
    null 확인이 실패하면, 새로운 제품 생성 모드로 진입
    2. views에는 기존 제품 ID를 저장하는 숨겨진 코드 작성
    3. controller에 제품 정보 가져오고, 새로운 제품 인스턴스 생성하고 save 호출
    사용자 입력 폼을 상수로 받아 요청 바디에 엑세스
    컨트롤러로 응답
    4. admin라우터에 post 활동 등록
    
    ## 128. 제품 삭제 기능 추가
    
    1. delete-product 라우트 추가
    2. id, name 을 productId에 추가
    3. controller에서 productId 정보 얻기
    4. 라우트 액션 연결
    5. delete 메서드 추가
    6. 모든 제품 호출
    7. filter로 제품 찾기
    8. 삭제 제품 제외하고 저장
    9. 콜백 함수를 통해 오류 확인
    오류가 나지 않았다면, 제품 제거
    
    ## 129. 장바구니 항목 삭제
    
    1. deleteProduct 추가, 삭제할 id, price 작성
    2. readFile 장바구니 정보 받기
        1. if 오류, return ; 삭제할 제품이 없다는 뜻
        2. 기존 장바구니 속성 새로운 객체로 받기
        3. 삭제하려는 제품 ID와 일치하는 제품 ID 찾기
        4. 제품 수량 찾아 확인하기
        5. filter로 반환 값이 true인 요소 유지
        제거하는 제품 빼고 다른 제품은 반환되도록 [prod.id](http://prod.id) ! == id
        6. 장바구니 총 가격 연산
        7. writeFile 작성
    3. products 모델에서 장바구니 모델 불러오기
    4. ID로 제거하기 전 제품 정보 얻고, 삭제하기
    5. controller의 admin.js에서 postDeleteProduct에 prodId를 넘기고 리다이렉트
    
    ## 130. 장바구니 페이지에 장바구니 항목 표시하기
    
    1. 콜백 함수로 cart.js에서 모든 제품 받기
    2. 콜백 함수 전달, 뷰 렌더링
    3. 장바구니 제품 필터링
    4. 배열을 만드러, 찾은 제품 넣기
    5. 반환을 위해 뷰로 보내기
    6. products 보여주기
    
    ## 131. 장바구니 항목 삭제
    
    1. form 형태의 button 생성
    2. cart-delete-item 라우터 작성
    3. shop.js에 getCartDeleteProduct 작성
    
    ## 132. 제품 삭제 버그 수정
    
    장바구니에서 해당 제품이 있는지 확인하는 과정이 없어서 버그가 발생했었다.
    if (!product)를 검사하여 true 값이 반환되면 제품이 없다는 뜻으로 그냥 return으로 마무리하면 된다.