# Model View Controller(MVC)

<aside>
💡 사용자 인터페이스, 데이터 및 논리 제어를 구현하는 데 널리 사용되는 소프트웨어 디자인 패턴

</aside>

- 소프트웨어의 비즈니스 로직과 화면을 구분하는 데 중점을 두고 있다. → **관심사 분리는 더 나은 업무의 분리와 향상된 관리를 제공한다.**
    - 코드의 여러 부분들이 각기 다른 기능을 가져서 어떤 부분이 어떤 작업을 책임지고 있는 지 확실하게 알게 하는 것 → **유지보수에 용이**

### Model

- 기본적으로 객체나 데이터를 나타내는 코드의 한 부분
- 데이터를 저장하거나 파일을 주고받는 등의 데이터 관련 작업을 하도록 한다.

### Views

- html 자료에 올바른 내용을 렌더링해 사용자에게 보내는 역할을 한다.
- 논리가 너무 많이 들어가는 것은 좋지 않다.

### Controller

- 모델과 뷰 사이의 **연결점**
- 뷰 → 애플리케이션 논리와 상관 없음
모델 → 데이터를 가져오고 저장하는 역할
- 컨트롤러가 모델과 함께 데이터를 저장하거나 저장 프로세스를 유발한다.
- 뷰에서 가져온 데이터를 전달하는 과정을 돕는다.
    
     → **작업할 모델과 렌더링할 뷰를 정의하는 역할**
    
- 미들웨어 기능 간에 분할된다.

### Route

- 어떤 경로에 따른 http 메서드에 따라 어떤 컨트롤러 코드를 실행할지 정의한다.

### MVC 모델 1

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FI1UUx%2FbtqEZ0IhZ6O%2FqzvwssYAAkEltNqYd3Kpik%2Fimg.png)

- 웹 개발을 할 때 JSP가 View와 Controller의 역할을 모두 한다.
    - **JSP(Java Server Pages)**: HTML 코드에 java 코드를 넣어 동적인 웹페이지를 생성하는 웹어플리케이션 도구
- JSP에 java, HTML, css의 코드가 섞여있게 되어서 코드가 복잡해지고 어려워져 유지보수가 힘들어진다.
- 비교적 설계가 간단해 작은 프로젝트에 알맞다.

### MVC 모델 2

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbgbhsb%2FbtqE0BOnWqs%2FGhoRjVi90P1dYSBjRHSo91%2Fimg.png)

- 모델 1에서 유지보수가 힘들다는 단점을 보완하기 위해 나온 모델
- JSP가 View의 역할을 하고 Servlet이 Controller 역할을 한다.
    - **Servlet:** 자바 코드 안에 HTML코드가 클래스의 형태로 들어가는 방식
    - Servlet이 웹브라우저의 요청과 웹 어플리케이션의 전체적인 **흐름을 제어**
- 초기 설계 단계에 비용이 많이 들어 개발 시간이 오래걸린다.

### MVC 패턴의 한계

- 대규모의 프로그램인 경우 다수의 View와 Model이 Controller를 통해 연결되기 때문에 Controller가 불필요하게 커지게 된다.
- 이러한 문제점을 보완한 MVC에 기반을 둔 다른 패턴들
    - MVVM(모델-뷰-뷰모델)
    - MVP(모델-뷰-프리젠터)
    - MVW(모델-뷰-왓에버)