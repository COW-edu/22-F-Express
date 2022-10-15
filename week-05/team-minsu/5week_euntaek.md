5week




# 동적 라우팅


동적 라우팅이란, 라우트의 경로에 특정 값을 넣어 해당 페이지로 이동할 수 있게 하는 것.



# 동적 VS 정적 웹


웹을 서비스할때 정적으로 구성하여 서비스를 하던 동적으로 구성하여 서비스를 하던 2가지 선택지가 있다. 정적인 웹서비스는 Static으로 표현을 하며  이미지, css, js파일등 컨텐츠를 바로 전달해주는 것이다. ⇒ 웹페일지가 변경되거나 수정되지 않는 한 언제나 고정된 웹 페이지만 볼 수가 있다.

반대로

동적 웹서비스의 경우 Dynamic으로 표현되며 좀 더 프로그래밍적으로 접근하는 방식이다. 추가로 실시간으로 데이터 베이스와 연동이되어 이에대한 정보는 클라이언트에 제공하기도 하기때문에 웹 페이지를 열때 매번 새로운 내용을 표시해주고 변경도 가능하다.

동적으로 웹 서비스

(예시)

```jsx
app.get('/dynamic', function(req, res) {
    var output = `
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title></title>
      </head>
      <body>
        Hello Dynamic!
      </body>
    </html>`;
    res.send(output);
})
```

다음과 같이 코드를 작성하고 app.js를 실행하면 Hello Dynamic! 이라는 문자열이 출력된다. ⇒ 정적 프로그래밍과 차이점이 없어보이나 위 텍스트를 계속 반복하여 출력해야한다면 정적 웹에서는 출력하고 싶은 만큼의 문자열을 하나하나 코드로 작성해 주어야 한다.

그러나 동적의 경우는 반복문 처리로 쉽게 해결할 수 있다.

정적 프로그래밍이 동적 프로그램에 비해 단점만 있는것 같지만 정적 프로그래밍은 동적 프로그래밍과 다르게 정적으로 코드를 구성하면 서버를 재시작하지 않고 출력결과를 즉시 반영이 가능하다.

⇒동적 프로그래밍의 경우 수정사항이 있으면 서버를 리부팅 해야한다.



# 동적, 정적 각 장단점

### 각 장단점

**정적 라우팅 (Static Routing)**

장점 : 관리자의 의도대로 제어할 수 있다. 

경로 정보를 주고 받을 필요가 없어 효율이 높다.

단점: 경로를 설정하고 유지하는데 공수가 든다.

적절한 경로 구현을 위해 관리자의 이해도가 필요하다.

네트워크 규모가 커지면 힘들다.




**다이나믹 (Dynamic) 라우팅**

장점:관리자의 설정유지를 위한 작업이 적다.

경로의 특성에 대한 정보를 주고 받아 상황에 따라 적합한 경로를 선택 할 수 있다.

단점: CPU, 메모리, 링크 대역폭등 자원을 사용한다. 

여러 정보를 가지게 되어 데이터 전송 속도가 느려진다.

라우트 매개변수 입력방법

쿼리 매개변수 사용방법

동적 매개변수 추출하기




# 쿼리 매개변수 + 사용방법

http://ccccc.com/topic?id=1

각 의미하는것은 다음과 같다.

http: ⇒ hyper text transfer protocol

[ccccc.com](http://ccccc.com) ⇒ domain

topic ⇒ path

id=1 ⇒ query string




### query 값 가져오기

Express의 request객체에는 키-값의 쿼리 매개변수가 포함된 쿼리라는 속성이 포함되어 있다.

url내의 쿼리 스트링을 가져오려면

```jsx
app.get('/topic', function (req, res) {
  //http://a.com/topic?id=1&name=choi 에서
	res.send(req.query.id) // 1
  	res.send(req.query.name) // choi
})
```

다음과 같이 값을 가져올 수 있다.



### 쿼리 매개변수 사용방법

req.param()에 key값을 인자로 넣어서 사용하는 방법

```jsx
app.get('/topic', function (req, res) {
  //http://a.com/topic?id=1&name=choi 에서
	res.send(req.params('id')) // 1
  	res.send(req.params('name')) // choi
})
```

다음과 같이 ()안에 키값을 넣고 쿼리스트링으로 넘어온 이름을 받아올 수 있다.