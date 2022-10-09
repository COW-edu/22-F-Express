# MVC (Model View Controller)
- 코드의 각 부분들이 각기 다른 기능을 가진 것과, 어떤 부분이 어떤 작업을 책임지고 있는지 파악해 분리한다
- 사용자 인터페이스, 데이터 및 제어 로직을 구현하기 위해 일반적으로 사용되는 소프트웨어 설계 패턴
- 소프트웨어의 business logic과 디스플레이의 분리를 중요시
    - business logic : 컴퓨터 프로그램에서 규칙에 따라 데이터를 생성 표시 저장 변경하는 부분 / 데이터처리를 수행하는 응용프로그램의 일부
- Seperation of Concerns
    - 보다 나은 분업과 유지보수를 도움
- Software Architectural Patterns중 하나
  - MVC는 의존성 문제를 가지고 있다(model없으면 view가 안됨)
## Model
- 데이터와 busniess logic을 관리한다
    - 데이터를 저장하거나 파일로부터 데이터를 주고받는다
- app에 포함해야 하는 데이터를 정의한다
- 데이터의 상태가 변경되면 model은 view 및 controller에 알린다
    - view에는 최신의 결과를 보여준다
    - controller에는 model의 변화에 따른 적용 가능한 명령을 추가 제거 수정 한다
- 객체나 데이터를 나타내는 코드의 한 부분
    - 어떤 동작을 수행하는 코드를 말한다
    - 표시형식에 의존하지 않아 보여지는 부분에는 신경쓰지 않아도 된다 (디스플레이와 분리)
## View
- layout 및 display를 처리한다
- app의 데이터를 표시하는 방법을 정의한다
- model로부터 표시할 데이터를 수신한다
    - 보여줄 값(model)을 controller로부터 받아와 사용자에게 보여준다
- application code와 분리되어 있다
    - view를 생성하기 위해 templete engine에 들어가는 데이터와 약간만 통합되어 있다
    - application code : 실행 프로그램 코드
## Controller
- model와 view에 명령을 routing한다
- middleware Function에 분할된다
    - 일부 logic이 분리되어 다른 middleware function으로 이동한다
- 사용자의 입력에 따라 model 및 view를 업데이트 한다
    1. 입력이 controller로 전송됨
    2. controller는 model을 조작하여 업데이트 한다
    3. 업데이트 된 데이터를 view로 전송한다
- controller는 view에서 가져오는 데이터를 전달하는 경우에도 돕는다
    - 예) 항목 순서를 최저에서 최고 가격으로 변경하는 경우
        - model을 업데이트할 필요없이 controller가 직접 처리한다
- model과 view사이의 연결점이다
- model의 mutator 함수를 호출해 상태를 바꾼다
    - mutator
        - model의 상태를 변경하는 함수
- in-between logic
    - 데이터와 교류하고
    - view를 반환한다
## Routes & Controller
- Routes는 어떤 경로에 따른 http 메서드에 따라 어떤 Controller 코드를 실행할지 정의함