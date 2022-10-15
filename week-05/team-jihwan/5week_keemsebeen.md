# section 8,9

## url에서 ID 추출

- 새로운 페이지를 표시하는 것이기 때문에 get 라우트를 사용한다.
- 라우팅할 path를 ‘/products/:productId’라고 설정하면 뒤에 : productId는 동적 파라미터를 담게되어 req.params.productId로 접근이 가능하다.

```jsx
router.get('/products:productId');
// 콜론은 (:) express에게 productId와 같은 라우트를 탐색하지 말라는 신호를 보낸다.
```

## POST 요청으로 데이터 전달하기.

- POST: 요청 본문을 통해 데이터를 전달할 수 있다. ( GET에서는 불가능. )
    - 데이터 게시에만 가능하다. 데이터를 포스팅할 때 URL에 무언가 더 추가할 필요가 없다. 정보를 POST 본문에 넣으면 되기 때문이다. (데이터를 읽어올 때는 사용 불가. )
- for문 또는 일반적인 조건문 안에 include가 포함되어 있는 경우 모든 product를 거쳐 실행하게 된다. 하지만 product는 해당 loop에서만 가능한 로컬변수이다.
    - 따라서 루프에 포함된 include에서 기본적으로 product를 받지 못한다.
    - 그러나 include에 입력하는 방법이 존재한다 include 함수에 객체를 입력하고 변수를 다시 추가해주는 두번째 인자를 추가하면된다.
    
    ```jsx
    <%- include('../includes/add-to-cart.ejs', {product: product}) %>
    ```
    

## 쿼리 매개 변수

- url에 ?를 추가하여 모든 url에 추가할 수 있으며 edit=true를 비롯한 = 표시로 분할한 키 값 쌍을 넣어주면 된다.
- & 표시로 분할해 줌으로써 다수의 쿼리 매개 변수를 입력할 수도 있다. 이것을 부차적 데이터라고 함.
- 도달하게 되는 라우터는 ? 표시까지의 부분을 통해 결정된다.
    
    ```jsx
    const editMode = req.query.edit;
    ```
    
- 컨트롤러에 존재하는 쿼리 매개변수를 확인 가능하다.
    - editMode가 요청으로 이동하여 설정되고 쿼리 객체가 있는가를 확인할 수 있다. 쿼리도 express.js에서 생성 및 관리를 함.
    - 또한 쿼리매개 변수에서 얻은 키에 접근을 시도하는 것만으로 데이터에 접근이 가능하다.
    - 값이 존재한다면 editMode는 true가 될 것이고, 존재하지 않는다면 false로 처리하게 된다.

[http://expressjs.com/en/4x/api.html#req](http://expressjs.com/en/4x/api.html#req)