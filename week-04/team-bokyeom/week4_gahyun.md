# Week4

# MVC 패턴이란?

MVC란 Model-View-Controller의 약자로 애플리케이션을 세가지 역할로 구분한 개발 방법론이다.

- View

클라이언트와 상호작용이 일어나는 것을 의미한다. 화면에 보여주기 위한 역할을 하는 것이다. User Interface가 바로 View 레이어 있는 코드로 핸들링 된다. React, Angular, Vuew와 같은 라이브러리 또는 프레임워크로 개발하는 앱을 생각하면 된다. 모바일 ios 안드로이드 앱도 View를 담당한다고 할 수 있다

- Controller

유저의 요청을 처리해서 응답하는 부분이라고 할 수 있다 뷰에서 일어나는 액션이나 이벤트에 대한 값을 받고 가공해 모델에게 전달하고 모델로부터 받은 값을 다시 뷰에게 전달한다

- Model

서비스에 필요한 모든 데이터는 모델에서 정의된다. 오로지 Model 레이어에 정의된 데이터베이스 schema를 통해서만 데이터베이스에 접근해 CRUD 로직을 처리할 수 있다.

### MVC Pattern 의 장점

세개의 레이어 역할이 나뉘어졌기 때문에 동시다발적인 개발이 가능하다. 

각각의 레이어 , 그리고 그 레이어 속에 속한 각각의 모듈을 테스트하기 좋다.

### Node js Project Layering

Node.js를 통한 BackEnd 애플리케이션은 큰 서비스의 관점으로 봤을 때 Controller, Model의 레이어를 담당하게 된다

하지만 오로지 이 두개의 레이어로만 로직을 분리하기에는 코드의 복잡성과 레이어 하나가 담당하는 비중이 너무 커지기에 더 확장해 프로젝트 코드를 레이어링 해야한다

라우터는 어떤 경로에 따른 http 메서드에 따라 어떤 컨트롤러 코드를 실행할지 정의한다. 

[https://velog.io/@jinukix/Node.js-MVC-모듈화](https://velog.io/@jinukix/Node.js-MVC-%EB%AA%A8%EB%93%88%ED%99%94)[https://velog.io/@ice-ame/node.js-MVC-디자인-패턴-데이터베이스-연결](https://velog.io/@ice-ame/node.js-MVC-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%97%B0%EA%B2%B0)

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

위 흐름을 보면 알  수 있듯이 View를 업데이트 하기 위해서는 M-V사이 의존성이 존재해서 애플리케이션이 커질수록 복잡도가 높아질 수 있다.

하지만 단순하고 직관적으로 구조를 파악하기가 가능하다. 

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
4. ViewModel은  View를 나타내주기 위한 Model이자 View의 Presentation Logic을 처리한다

mvp와 마찬가지로 M-V 사이 의존성이 없고 v-vm이 1:1 관계가 아닌 독립적이기에 이 둘 사이 의존성도 없다 하지만 ViewModel을 설계하는데 쉽지 않다는 단점이 있다

[https://brunch.co.kr/@oemilk/113](https://brunch.co.kr/@oemilk/113)

[https://adjh54.tistory.com/60](https://adjh54.tistory.com/60)