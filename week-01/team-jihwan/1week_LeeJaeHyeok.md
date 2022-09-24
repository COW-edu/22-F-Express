# 1주차

# Section 3: 기본 개념 이해

## 노드란?

Chrome V8 Javascript로 엔진으로 빌드된 Javascript 런타임

## 웹 작동 방식

Client → Request → Server(database, node…) → Response → Client

응답과 요청에는 Header 라는 메타 데이터가 있음

(내용이 무엇인지 설명해줌 일종의 명세서)

HTTP(HyperTextTransferProtocol) : 요청과 응답의 통신 규약

HTTPS(HyperTextTransferProtocolSecure): 보안성이 강화된 http

## Node.js의 라이프사이클 및 이벤트 루프

## 이벤트 루프란?

```jsx
function oneMore() {
	console.log('one more');
}
function run() {
	console.log('run run');
	setTimeout() => {
		console.log('wow');
}, 0)
new Promise((resolve => {
	resolve('hi');
})
	.then(console.log);
	oneMore();
}

setTimeout(run, 5000);

//출력
//run run
//one more
//hi
//wow
```

1. oneMore과 run이 선언됨(메모리 상에 올라감)
2. 호출 스택에 annonymous 등록
3. 호출스택에 setTimeout(run, 5000);
4. setTimeout(run, 5000) 실행되면서 백그라운드로 옮겨감
5. 백그라운드 상태: 타이머(run, 5초)
6. annonymous 끝남(파일이 다 실행됐기때문)
7. 백그라운드에서 5초 후 테스크 큐로 run 함수 이동
8. **호출스택이 비어있는데 이 상태일때만 테스크 큐에서 이동 가능**
9. 이벤트 루프에 의해서 run 함수가 호출스택으로 이동
10. console.log(’run run’) 호출 스택에 들어왔다가 콘솔창에 출력
11. setTimeout(익명함수, 0초) 호출스택에 추가
12. 백그라운드에서 타이머(익명, 0)
13. Promise 실행하는데 내부가 동기이기 때문에 resolve(’hi’)가 호출 스택에 올라감
14. then을 만나서 비동기가 됨 백그라운드에 then console.log(hi)
15. oneMore가 실행되면서 console.log
16. 콘솔창에 one more 출력
17. 호출 스택에서 oneMore, run 나감
18. 백그라운드에 있던 타이머가 테스크큐로 익명함수 이동, then 이 끝나서 console.log(’hi’)도 테스크 큐로 이동
19. **Promise가 우선순위가 높으므로 익명함수보다 먼저 호출 스택으로 이동하여 실행됨 (nextTick도 있음)**
20. 익명함수 실행 콘솔창에 wow 출력됨

**이벤트루프 실행 플로우**

**호출스택 → 백그라운드 → 테스크 큐 → 호출스택**

## **요청 리디렉션**

![Untitled](https://i.esdrop.com/d/f/jx3AtGlfQP/UXNU5VKVs2.png)

이렇게 요청 리디렉션을 할 시 얻게되는 것

1. http 상태코드 : 302
2. message.txt 파일에 DUMMY 가 작성됨
3. 헤더가 변경되어 input에 데이터가 입력되면 다시 원상태로 깜빡 거리고 돌아감 === ‘리디렉션’

## Streams & Buffer

Nodejs는 요청이 얼마나 큰고 복잡한지 모르기 때문에 데이터를 **비동기**적으로 처리

![Untitled](https://i.esdrop.com/d/f/jx3AtGlfQP/rBqzsEnt7N.png)

이를 위해 Buffer를 사용함(여러 request들을 지니고 비동기적 처리를 도움)

## 이벤트 기반 코드 실행의 이해

Nodejs는 보편적으로 함수안에 함수가 있을경우 바깥 함수를 먼저 실행합니다.

리스너 부분(드래그 한 부분)

![Untitled](https://i.esdrop.com/d/f/jx3AtGlfQP/nvKr6yiHDY.png)

req.on 콜백함수를 통해 비동기적 처리.

## 블로킹 및 논블로킹 코드

예시)

논블로킹 코드 fs.writeFile : 동기처리 x

블로킹 코드 fs.writeFileSync : 동기(Sync) 처리하여 파일이 작성되기 전까지 다른 코드 실행을 멈춤

⇒ 대용량 프로그램 실행시 문제 발생

## Node.js 백그라운드 확인

자바스크립트는 싱글스레드 언어

스레드: 운영 체제에서의 프로세스

이벤트 루프: 모든 콜백을 순서에 따라 처리함

이벤트 루프가 콜백을 처리하는 순서

1. Timer (setTimeout, setInterval)
2. Pending 콜백(입력과 출력 혹은 네트워크 연산(블로킹 연산)이 끝난 콜백들을 처리하는데 너무 많다면 처리하다가 두고 루프를 이어가고 다음번에 돌아와서 계속 처리)
3. Poll : 새로운 입출력 콜백을 찾고 빨리 실행하도록 함 미루지 않음. 타이머가 다 된 콜백도 확인하여 Timer로 가서 반복을 이어가지 않고 다시 돌아갈 수 있음
4. Check: setImmediate 콜백이 다른 열린 콜백이 다 처리되고 실행됨(setTimeout보다 빠름)
5. Close 콜백: 닫힌 콜백들을 처리

처리할 이벤트가 없으면 종료

## Node 모듈 시스템 사용

라우터 분리(index역할 app.js, 분리된 라우터 routes.js)

app.js 에서 ES6 문법을 이용해서 routes를 가져옴

```jsx
const routes = require(’./routes’);
```

이때, 절대경로를 쓰지말고 상대경로를 사용하여야 함(같은 디렉토리 > ./)

routes에서 전역 객체를 통한 export

```jsx
module.exports = requestHandler;
//exports = requestHandler;

// requestHandler function 작성 생략
```

(node.js가 module을 생략 할 수 있게해줌 === nodejs의 단축키)

# Section 4: 개선된 개발 워크플로우 및 디버깅

## NPM 스크립트의 이해

npm은 node package manager 로 node.js의 패키지를 관리할 수 있는 도구입니다.

== 저는 기능이 업그레이드 된 yarn을 사용하여 정리하여 보겠습니다==

노드 프로젝트 시작 명령어: yarn init

yarn init시 name, version, description, entry point(진입점인데 npm은 app.js이고 yarn은 index.js라는 차이점이 있었습니다.), repository url, author, license, private 입력을 하면

버전을 관리 할 수 있는 package.json 파일 생성

package.json

- 설치한 패키지의 버전 관리를 위한 버전 관리 기록용 파일

scripts 키워드의 “start” 는 특별히 npm run start 이렇게 실행하지 않고

바로 npm start로 실행 할 수 있음.

⇒ npm start로 애플리케이션을 ‘쉽게’ 시작 할 수 있음!(=== yarn start)

## 제 3자 패키지 설치하기

```jsx
npm install 패키지명

//노드몬 설치
npm install nodemon --save-dev

//yarn 노드몬 예시
yarn add -D nodemon
```

ex) 자동 재시작 매커니즘: nodemon(변경사항이 생길 때 마다 서버를 껏다 키는 번거로움을 없애줌)

노드몬은 개발단계에서만 필요하지 실제 서버에 설치할 필요가 없음

npm install nodemon —save : 프로덕션 의존성으로 설치

npm install nodemon —save-dev : 단순히 개발단계에서 사용하는 것임을 나타냄

([npmjs.com](http://npmjs.com) 에서 다양한 패키지 명을 찾을 수 있음)

+모듈을 삭제하고 싶을때)

```jsx
npm uninstall 패키지명
```

모듈을 install 시

node_modules 폴더와 package-lock.json 파일 생성되고

package.json이 업데이트됨(버전 관리)

node_modules: 모듈들을 모아 놓은 폴더로 삭제해도 npm install로 다시 설치하면 됨

package-lock.json: 오늘 설치한 그 버전들만 저장하므로 다른 사람들과 버전 공유가 가능

npm install : package.json에 명세된 모든 패키지를 찾아 설치함

## 전역 및 로컬 npm 패키지

```jsx
"scripts": {
	.
	.
	"start": "nodemon app.js",
	.
	.
},
```

위와 같이 작성 후 터미널에서 npm start 시 로컬에 노드몬이 설치됐으므로 정상적으로 작동합니다

반면, nodemon app.js 시 머신 전체에 설치된게 아니므로 “not found” 에러가 발생합니다

(nodemon app.js시 전역에서 nodemon이 설치됐는지 검색하므로)

⇒ 터미널에서는 글로벌 패키지를 사용함을 알 수 있다(npm install -g nodemon 실행 시 머신에 설치)

## 오류의 종류(Type of error)

1. 구문 오류(Syntax Error): 해결하기 쉬움, 문법 오류 → IDE가 오류난 줄을 알려줌
2. 런타임 에러(Runtime Error): 프로그램을 실행할 때 발생하는 에러
3. 논리적 오류(Logical Error): 오류 메세지가 뜨지 않고 가장 해결하기 어려움
   1. 중단점을 찍어서 데이터를 확인

::중단점을 활용한 console 확인

### 과제: NVM 이란 무엇이고 설치해보자!

NVM은 node.js를 설치하는 툴

Node Version Manager로써 버전을 관리한다

- **가상환경에서 다양한 버전의 node.js를 쉽게 설치하고 사용가능**
- 최신의 node.js를 사용 할 수 있다(리눅스 운영체제 패키지 매니저는 구버전이 설치되는 경우가 많음)
- 여러 node.js 버전 간 전환이 쉽다

<aside>
💡 !설치순서: nvm → Node.js → npm

</aside>

nvm 설치 사이트

[https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

ex) 14버전 node.js 설치

→nvm install 14

![Untitled](https://i.esdrop.com/d/f/jx3AtGlfQP/LUXaDkYOGF.png)
