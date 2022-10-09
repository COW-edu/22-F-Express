# TEAM 보겸 - 4주차 RBF

## 아키텍처 패턴이란?

- 소프트웨어 구조를 패턴화 한 것을 의미한다

앱에서는 아키텍처 패턴으로 MVC, MVP , MVVM 패턴이 있다

## 디자인 패턴이란?

- 객체 지향 프로그래밍을 설계할 때 자주 발생하는 문제에 대해 피하기 위해서 사용되는 패턴을 의미한다.

→ 아키텍처 패턴은 소프트웨어 구조 자체를 패턴화 한 것이고 디자인 패턴은 소프트웨어 구조 내에서 특정 문제를 피하기 위해 사용되는 패턴이므로 디자인 패턴 상위에 아키텍처 패턴이 존재한다

### MVC 패턴

<aside>
💡 MVC 패턴이란?

Model - View - Controller로 애플리케이션을 세 가지 계층으로 구분한 방법론을 의미한다

</aside>

**MVC 패턴 흐름**

1. 모든 입력들은 Controller로 전달된다
2. controller는 입력에 해당하는 Model을 업데이트 한다
3. 업데이트 결과에 따라 View를 선택한다 (Controller는 View를 선택 관리 가능 1:n의 관계로)
4. Controller는 View를 선택만 할 뿐 직접 업데이트 하지 않는다
5. view를 업데이트 하기 위해서는 아래와 같은 방법이 있다

- View가 Model을 직접 이용하여 업데이트
- Model에서 View를 Notify하여 업데이트
- View가 Pollingㅎ여 Model의 변화를 감지해서 업데이트

위 흐름을 보면 알 수 있듯이 View를 업데이트 하기 위해서는 M-V사이 의존성이 존재해서 애플리케이션이 커질수록 복잡도가 높아질 수 있다.

하지만 단순하고 직관적으로 구조를 파악하기가 가능하다.

### 1. 모델: 데이터와 비즈니스 로직 관리

모델은 앱이 포함해야할 데이터 정의.

데이터 변화 → 뷰, 컨트롤러에 알림

뷰는 최신의 결과를 보여줌.

컨트롤러는 모델 변화에 따라 명령도 변화시킬 수 있음.

**규칙**

- 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야함
- 뷰, 컨트롤러에 대해 어떤 정보도 알지 말아야 함
- 변경이 일어나면, 변경을 알려줄 처리 방법을 구현해야함.

### 2. 뷰: 레이아웃, 화면 처리

MVC에서 모델은 여러개의 뷰를 가질 수 있음

앱의 데이터를 보여주는 방식 정의

표시할 데이터를 모델로부터 받음

**규칙**

- 모델이 가지고 있는 정보를 따로 저장하면 안됨.
- 다른 구성요소를 몰라야 함.
- 변결될 때 통지할 처리 방법 구현.

### 3. 컨트롤러: 명령을 모델과 뷰 부분으로 라우팅.

MVC의 뷰는 여러개의 컨트롤러를 가지고 있음.

앱에서 사용자의 입력에 대한 응답으로 모델과 뷰를 업데이트 하는 로직 포함.

모델에 명령을 보내서 모델의 상태를 변경하거나 뷰에 명령을 보내 모델의 표시 방법을 바꿀 수 있음.

**규칙**

- 모델이나 뷰에 대해 알고 있어야 함
- 모델이나 뷰의 변경을 모니터링 해야함

### MVC 패턴을 사용하는 이유

유지 보수의 편리성.

기존에 유지보수 시 높아지던 기능간의 결합도 → 사소한 변경도 다른 비즈니스 로직에 영향 → 버그 유발 가능

UI 시스템의 핵심 컴포넌트를 세가지로 나누고 자신의 역할만 수행하고 넘겨주는 방식으로 시스템 결합도를 낮춤.

유지보수 시에도 특정 컴포넌트만 수정 → 쉬운 시스템 변경 가능.

### 한계

대규모 프로그램의 경우 다수의 뷰와 모델이 컨트롤러에 연결 → 컨트롤러가 불필요하게 커지는 현상 발생

복잡한 화면을 구성하는 경우도 동일한 현상 발생.

‘Massive-View-Controller’ 라고 함.

### MVP 패턴

<aside>
💡 MVP 패턴이란?

MVP는 Model - View - Presenter로 애플리케이션을 세가지 계층으로 구분한 방법론

</aside>

**MVP 패턴 흐름**

1. 모든 입력들은 View로 전달
2. Presenter은 입력에 해당하는 Model을 업데이트
3. Model 업데이트 결과를 기반으로 View를 업데이트
4. Presenter는 해당 View를 참조하고 있다 (1:1 관계)
5. Presenter은 view와 Model 인스턴스를 가지고 model view 사이의 매개체 역할을 한다

MVC 패턴과 달리 M-V 사이 의존성이 없다 하지만 앱이 커지거나 복잡해질수록 V-P 간의 의존성이 강해지는 문제점이 발생한다

### MVVM 패턴

<aside>
💡 MVVM 패턴이란?

MVVM은 Model-View-ViewModel로 애플리케이션을 세가지 계층으로 구분한 방법론을 의미한다

</aside>

1. 모든 입력들은 View로 전달한다
2. ViewModel은 입력에 해당하는 Presentation Logic을 처리하여 View에 데이터를 전달한다

→ 이때 Presentation Logic은 실제 눈에 보이는 GUI 화면을 구성하는 코드를 뜻함

1. ViewModel은 View를 참조하지 않기에 독립적이다 (1:1 관계이다)
2. 따라서 View는 자신이 이용할 VIewModel을 선택해 바인딩하여 업데이트를 받게 된다
3. Model이 변경되면 해당하는 ViewModel을 이용하는 View가 자동으로 업데이트
4. ViewModel은 View를 나타내주기 위한 Model이자 View의 Presentation Logic을 처리한다

mvp와 마찬가지로 M-V 사이 의존성이 없고 v-vm이 1:1 관계가 아닌 독립적이기에 이 둘 사이 의존성도 없다 하지만 ViewModel을 설계하는데 쉽지 않다는 단점이 있다

[https://brunch.co.kr/@oemilk/113](https://brunch.co.kr/@oemilk/113)

[https://adjh54.tistory.com/60](https://adjh54.tistory.com/60)

## 모델 추가

1. models 폴더와 js 파일 생성
2. 제품의 형태를 정의하는 클래스 생성
   products 배열 생성

```jsx
const products = [];

module.exports = class Product {
  //제품의 형태를 정의하기 위한 구축자 함수를 생성하자.
  constructor(t) {
    //제품에 대한 제목 수신, 컨트롤러 내부에서 생성
    this.tilte = t;
  }
  save() {
    //제품 배열에 제품 생성(저장)하기
    products.push(this);
  }
  static fetchAll() {
    //배열로부터 모든 제품 회수하기
    //static을 사용하면, 예시된 객체가 아니라, 클래스 자체에 직접 호출하게 된다.
    return products;
  }
};
```

1. controller에 있는 products 배열과 배열 관련 논리 삭제
2. 클래스 임포트
3. 클래스 설계도를 토대로 새로운 객체 생성
4. save를 호출하여 저장
5. fetchAll로 모든 제품 가져오기

## 모델을 통해 파일에 데이터 저장하기

배열이 아닌 파일에 저장해보자.

1. 배열을 삭제하고, 코어 fs 모듈에서 fs 임포트
2. Path 모듈을 사용해 모든 운영체제에서 작동하는 경로 구축
3. save를 호출할 때 파일에 저장
4. 기존 배열 가져오기

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

1. 데이터를 저장소에서 가져오기

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

## 모델을 통해 파일에서 데이터 가져오기

1. 콜백 인수(cb) 포함시키자.
2. controllers 파일에 fetchAll 함수 내부로 cb 전달하자.
