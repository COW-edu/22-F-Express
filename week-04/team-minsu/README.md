# TEAM 민수 - 4주차 RBF

# 아키텍처 패턴

## 아키텍처 패턴(Architecture Pattern)이란?

- `소프트웨어의 구조를 패턴화 한 것을 의미합니다.`
- 주어진 문맥 안에서 소프트웨어 아키텍처의 공통적인 발생 문제에 대한 재사용 가능한 해결책을 의미합니다.
- 아키텍처 패턴은 소프트웨어 디자인 패턴과 비슷하지만 더 넓은 범위에 속합니다.ex) MVC Pattern, MVP Pattern, MVVM Pattern,...

## 디자인 패턴(Design Pattern)이란?

- `객체 지향 프로그래밍을 설계할 때 자주 발생하는 문제에 대해서 피하기 위해 사용되는 패턴을 의미합니다.`
- 디자인 패턴은 건축으로 치면 공법에 해당하는 것으로 소프트웨어의 개발 방법을 공식화한 것입니다.
- 소수의 뛰어난 엔지니어가 해결한 문제를 다수의 엔지니어들이 처리할 수 있도록 한 규칙이면서 구현자들 간의 커뮤니케이션의 효율성을 높이는 기법입니다.ex) 생성, 구조, 행위 패턴, …

## 아키텍처 패턴의 종류 이해하기

- 유명한 아키텍처 패턴으로 `MVC, MVP, MVVM` 패턴이 있습니다. 이에 대해서 각각의 아키텍처 패턴에 대한 이해가 필요합니다.

## 📌 MVC-MVP-MVVM 패턴 비교 및 정리

### 1.MVC(Model-View-Controller) 패턴

- `MVC는 Model-View-Controller로 애플리케이션을 세 가지의 계층으로 구분한 방법론을 의미합니다.`
- 사용자가 처음 페이지를 출력하는 경우 Controller로 요청이 발생하고 Model에서 데이터를 가져와서 그 정보를 바탕으로 시각적 표현인 View를 그려주는 아키텍처 패턴을 의미합니다.

#### 1-1. 구조 계층

- Model : 애플리케이션에서 데이터를 저장하고 처리하는 계층을 의미합니다.
- View : 애플리케이션에서 사용자가 직접 보는 화면(UI)을 담당하는 계층을 의미합니다.
- Controller : 뷰와 모델간의 관계를 설정하는 계층이며 해당 부분에서 애플리케이션의 로직을 담당하는 계층을 의미합니다. (View에서 UI를 갱신하고 Model에서는 데이터를 업데이트 하는 구조입니다.)

#### 1-2. 동작 순서

💡  MVC 아키텍처 패턴의 흐름

전체 흐름

```
사용자의 Action → Controller → Model → Controller → View → Controller → View → 사용자 화면
```

상세 흐름

```
1. 사용자의 Action을 Controller에서 받습니다. (사용자 Action → Controller)
2. Controller에서는 이를 확인하고 Model을 업데이트 수행합니다.(Controller → Model)
3. 수정된 값을 Controller로 반환합니다.(Controller ← Model)
4. Controller에서는 View의 수정합니다.(View ← Controller)
5. 사용자에게 변경된 화면을 반환합니다.(사용자 Action ← View)
```

> 참고 - MVC에서 View가 업데이트 되는 방법
>
> - View가 Model을 이용하여 직접 업데이트 하는 방법
> - Model에서 View에게 Notify 하여 업데이트 하는 방법
> - View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법.

#### 1-3. 특징

- Controller는 여러개의 View를 선택할 수 있는 1:n 구조입니다.
- Controller는 View를 선택할 뿐 직접 업데이트 하지 않습니다. (View는 Controller를 알지 못합니다.)

#### 1-4. 장점

- MVC 패턴의 장점은 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순합니다. 단순하다 보니 보편적으로 많이 사용되는 디자인패턴입니다.

#### 1-5. 단점

- MVC 패턴의 단점은 View와 Model 사이의 의존성이 높다는 것입니다. View와 Model의 높은 의존성은 어플리케이션이 커질 수록 복잡하지고 유지보수가 어렵게 만들 수 있습니다.

<br><br><br>

### 2.MVP(Model-View-Presenter) 패턴

- MVP 패턴은 Model + View + Presenter를 합친 용어입니다. Model과 View는 MVC 패턴과 동일하고, Controller 대신 Presenter가 존재합니다.

#### 2-1. 구조 계층

- Model : 애플리케이션에서 데이터를 저장하고 처리하는 계층을 의미합니다.
- View : 애플리케이션에서 사용자가 직접 보는 화면(UI)을 담당하는 계층을 의미합니다.
- Presenter : - 해당 계층에서는 뷰와 모델을 완전히 분리하고 서로간의 의존성을 없앴습니다.
  (View - Presenter / Model - Presenter 구조 - 인터페이스로 구성되어 있습니다.) Presenter는 Model을 참조하고 있다가 Update가 발생시 View를 업데이트를 합니다.
  또한 Presenter는 이벤트 발생시 View를 참조해서 Model의 데이터를 업데이트를 합니다.

#### 2-2. 동작 순서

💡  MVP 아키텍처 패턴의 흐름

전체 흐름

```
- 전체 Flow - 사용자의 Action → View → Presenter → Model → Presenter → View → 사용자
```

상세 흐름

```
1. 사용자의 Action을 View에서 받습니다. (사용자 Action → View)
2. View에서는 Presenter로 요청을 합니다. (View → Presenter)
3. Presenter에서는 Model로 데이터를 요청합니다(Presenter → Model)
4. Model은 Presenter로 데이터를 전달합니다.(Model → Presenter)
5. Presenter는 View에게 데이터를 전달합니다.(Presenter → View)
6. View에서 사용자로 화면을 보여줍니다.(View → 사용자)
```

#### 2-3. 특징

- Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 합니다.
- Presenter와 View는 1:1 관계입니다.

#### 2-4. 장점

- MVP 패턴의 장점은 View와 Model의 의존성이 없다는 것입니다. MVP 패턴은 MVC 패턴의 단점이었던 View와 Model의 의존성을 해결하였습니다. (Presenter를 통해서만 데이터를 전달 받기 때문에..)

#### 2-5. 단점

- MVC 패턴의 단점인 View와 Model 사이의 의존성은 해결되었지만, View와 Presenter 사이의 의존성이 높은 가지게 되는 단점이 있습니다. 어플리케이션이 복잡해 질 수록 View와 Presenter 사이의 의존성이 강해지는 단점이 있습니다.

<br><br><br>

### 3. MVVM(Model-ViewModel-Model) 패턴

- MVVM은 Model-ViewModel-Model로 애플리케이션을 세 가지의 계층으로 구분한 방법론을 의미합니다.
- View는 ViewModel을 알고 있지만, ViewModel은 View를 알지 못합니다.
- 해당 모델에서는 ViewModel은 Model을 알고 있지만, Model은 ViewModel을 알지 못합니다.

#### 3-1. 구조

- Model : 애플리케이션에서 데이터를 저장하고 처리하는 계층을 의미합니다.
- View : 애플리케이션에서 사용자가 직접 보는 화면(UI)을 담당하는 계층을 의미합니다.
- View Model : 애플리케이션에서 View와 Model 사이에 존재하며 서로간의 중재를 하는 역할을 수행합니다.
  - View - ViewModel : 사용자와의 뷰의 상호작용(클릭, 키보드 등작 등)을 수신하여 이에 대한 처리를 View와 ViewModel을 연결하고 있는 데이터 바인딩을 통해 서로간을 연결합니다.
  - Model - ViewModel: 사용자의 데이터의 변경이 발생하는 경우 데이터를 가져오거나 갱신 한 뒤 View에게 전달하여 사용자에게 전달하는 역할을 수행합니다.

#### 3-2. 동작 순서

💡  MVVM 아키텍처 패턴의 흐름

전체 흐름

```
- 전체 흐름 - 사용자 Action → View → ViewModel → Model → ViewModel → View → 사용자
```

상세 흐름

```
1. 사용자가 입력한 값이 View를 통해 들어옵니다. (사용자 → View)
2. View에 입력값이 들어오면 ViewModel로 입력 값을 전달합니다. (View → ViewModel)
3. 전달받은 ViewModel은 Model에게 데이터 요청을 보냅니다. (ViewModel → Model)
4. Model은 ViewModel에게 요청받은 데이터를 Response 합니다(ViewModel ← Model)
5. ViewModel은 그 값을 처리하여 내부에 저장합니다.
6. View는 ViewModel과의 ‘데이터 바인딩’을 통해 화면상에 표출합니다.(View ↔︎ ViewModel)
```

#### 3-3. 특징

- MVVM 패턴은 Command 패턴과 Data Binding 두 가지 패턴을 사용하여 구현되었습니다.
- Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앴습니다.
- View Model과 View는 1:n 관계입니다.

#### 3-4. 장점

- MVVM 패턴은 View와 Model 사이의 의존성이 없습니다. 또한 Command 패턴과 Data Binding을 사용하여 View와 View Model 사이의 의존성 또한 없앤 디자인패턴입니다. 각각의 부분은 독립적이기 때문에 모듈화 하여 개발할 수 있습니다.

#### 2-5. 단점

- MVVM 패턴의 단점은 View Model의 설계가 쉽지 않다는 점입니다.

<br><br>

## 최종 아키텍처 패턴 간의 비교

| 패턴      | 장점                                                                                                                                                                    | 단점                                                                                                                                                                                |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MVC 패턴  | 단순하고 직관적으로 구조를 파악하기가 가능합니다. 기능별로 코드를 분리하여 하나의 디렉토리에 코드를 모으는 것을 방지하여 가독성과 코드 재 사용성이 좋습니다.            | 핵심 비즈니스 로직이 컨트롤러에 집중되어 있어서 코드 유지 관리에 어려워집니다. View와 Model의 의존성으로 어플리케이션이 커질수록 복잡도가 높아집니다                                |
| MVP 패턴  | 애플리케이션의 모델, 뷰, 프리젠터 레이어가 분리되어 있어 코드 유지 및 테스트가 용이합니다.                                                                              | View와 Presenter가 1:1 관계이기에 서로간의 의존성이 높아집니다.                                                                                                                     |
| MVVM 패턴 | View와 Model 사이의 의존성이 없다 Command 패턴 혹은 데이터 바인딩을 사용하여 View와 ViewModel 사이의 의존성이 없다. 프로젝트 파일을 유지 관리하고 쉽게 변경 할 수 있다. | View와 Model을 설계하는데 쉽지 않다. 이 디자인 아키텍처 패턴은 소규모 프로젝트에는 적합하지 않습니다. 데이터 바인딩 로직이 너무 복잡하면 애플리케이션의 디버깅이 어려워 질 수 있음. |