# 섹션 3, 4

### 섹션 3

 **Core Mudules**

- http : 서버를 생성하거나 요청을 보냄 (즉, nodeJS가 다른 서버에 요청을 보내서 여러 서버거간에 소통 가능 ex. google map API에서 좌표 따오기)
- https : 모든 전송 데이터가 암호화되는 SSL 서버를 생성
- fs
- path : 파일 시스템에 있는 파일에 대한 경로를 구축하는 용도
- os : 운영체제 관련 도움을 줌

**서버 생성하기**

방법1 : 함수 생성 → function 함수이름(요청 인수, 응답인수){ }

`function rqListener(req, res) { }`

`http.createServer();`

방법2 : 익명함수 

`http.createServer(function(req, res) { });`

or

`http.createServer((req, res) ⇒ { });`

→ 이벤트 드리븐 아키텍처 (EDA) : 요청이 들어올 때 마다 익명 함수 실행됨

const server = http…~ → 변하지 않는 변수 선언으로 서버 저장해줌

server.listen(포트번호); → 스크립트를 바로 종료하지 않고 계속 실행되면서 듣도록 함

process.exit → 서버종료 (따로 하지 않으면 계속 작동함) or ctrl + c

![Untitled](%E1%84%89%E1%85%A6%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB%203,%204%20624ddd37a86f41fca954f2773a074d5a/Untitled.png)

**요청에 대한 정보에 접근 하는 법**

req.원하는 정보.. ← 아까 서버 만들면서 써준 인수 넣어줌 

 헤더 : 요청 및 응답에 추가된 메타 정보

res.write() ← 데이터를 기록할 수 있음

ex) `res.write(’<html>’);`

`res.write(’<head><title>My First Page</title></head>);`

 res.end() 이후에는 더이상 write를 넣으면 안됨. 응답을 끝낸 뒤에는 변경을 해선 안되기 떄문에.. 에러 발생 

**라우터 요청**

GET : 클릭하거나 URL을 입력하면 자동으로 전송됨

POST : 해당 양식을 생성함으로써 직접 설정해줘야 함 

if 문 안에서는 return res.end() → res.end() 후에는 다른 것들이 더이상 들어올 수 없지만 아예 res.end()를 넣지 않으면 위 함수에서 빠져나오지 않기 때문.

**데이터에 접근하기..** 

`req.data()` 이건 없음!

stream → 계속되는 프로세스. 노드가 많은 양의 요청을 한 청크씩 읽고 어느 시점에 다 읽게 되면 요청 전체를 읽기까지 기다리지 않고 각각의 청크를 다룰 수 있게 됨.

 : 간단한 요청의 경우 위와 같은 과정이 필요하지 않지만 node js는 요청의 크기를 모르기 때문에 모두 위와 같이 처리함. 

청크는 코드를 통해 제어가 되지 않음. 따라서 청크를 체계화 하기 위해  buffer를 사용함 

buffer : 여러개의 청크를 보유하고 파싱이 끝나기 전에 작업할 수 있게 함. (버스 정류장과 비슷한데… 버스는 항상 달리지만 승객이 버스를 이용하려면 멈추는 버스 정류장이 필요하다…….) 

`req.on()`을 통해 .. 데이터를 읽을 수 있다. 

ex) `const body = [ ] ;`  // 꼭 공란이어야 함.. 

`req.on(’data’, (chuck) ⇒ {`

`body.push(chunck);`

`});`

`req.on(’end’, () ⇒ {`

`const parsedBody = Buffer.concat(body).toString();`

`console.log(parsedBody);`

`});`

**응답 발송과 이벤트 리스너** 

응답 발송은 이벤트 리스너 실행이 끝났다는 의미가 아님. 응답이 발생한 후에도 계속 실행 됨.

node js 는 함수 안에 함수를 넣으면 안쪽 함수를 나중에 실행함 → 비동기식 

fs.writeFileSync() 을 사용할 경우 파일을 다 읽기 전까지 아랫 줄의 코드를 실행시키기 않음 → 큰 파일이 입력되면 서버가 멈춘 것 처럼 보일 것… 

따라서 file.writeFile(’message.txt’, message, err ⇒ {…})을 사용하는 것이 더 나음. → 전체 운영을 막지 않음.

**이벤트 루프**

node js 를 계속 실행하도록 만드는 루프로 모든 콜백을 처리함. 새로운 반복이 시작될 때 마다 실행해야하는 타이머 콜백(setTimeout…)이 있는지 확인.  타이머가 끝난 콜백을 실행함 .. 이후로 실행되는 콜백의 순서는,,,

타이머 → I/O관련 및 다른 이벤트 → setlmmediate 와 닫힌 이벤트 → 남아있는 이벤트 핸들러가 있는지 확인 (refs == 0 / 추후에 처리할 작업이 늘어 날 때마다 refs 숫자가 늘어남. 서버에서 listen을 통해 들어오는 요청을 드는 이벤트들은 기본적으로 끝나지 않기 때문에 refs는 항상 1 이상일 것)→ 프로그램 종료

### 섹션 4

**NPM 스크립트** 

npm init를 통해 package.jason 파일 생성.

package  파일 안의 script 에 스크립트를 추가해 줄 수 있음. (입력할 때는 npm run ~~ / start는 제외) 

ex) “start”: “node app.js” 를 넣으면 npm start 로  node app,js가 살행됨.. 

**제 3자 패키지** 

- 개발 패키지 : 개발 도중 도움이 됨
- 프로덕션 의존성 : 서버가 돌아가면서 앱이 도움이 됨

ex) 저장하면 서버가 자동 재시작 하고 싶음

`npm install nodemon —save-dev`

nodemon은 서버 이름 

save는 프로덕션 의존성으로 설치 (실제로 코드안에서 사용하고 자겁하는 패키지)

dev는 단순히 개발 도중 사용하는 것임을 나타냄

g는 머신 전체에 설치함으로써 어디에서든지 사용할 수 있게 함. 

**전역 기능 vs 코어 모듈 vs 제3자 모듈**

- **전역 기능:** const나 function 같은 키워드 및 process 등의 전역 객체
- **코어 Node.js 모듈:** 파일 시스템 모듈 ("fs"), 경로 모듈 ("path"), Http 모듈 ("http") 등
- **제3자 모듈:** npm install을 통해 어떤 종류의 기능도 설치 가능

**전역 기능**은 **항상 사용 가능**하며, 사용하기 위해 파일에 임포트 할 필요가 없음.

코어 Node.js 모듈은 설치하지 않아도 되기 때문에 `npm install`이 필요하지 않지만, 관련된 기능을 사용하려면 임포트 해야 함.

ex)

`const fs = require('fs');`

“fs” 모듈에서 내보낸 `fs`객체를 사용할 수 있게 됩니다.

프로젝트 폴더에`npm install`을 실행해 제3자 모듈을 설치하고 임포트 합니다.

추후 강의에서 다룰 예정이라 지금 당장 이해하실 필요는 없지만, 그 예로

Terminal 또는 명령 프롬프트에서는
 `npm install --save express-session`

app.js 같은 코드 파일에서는
`const sessions = require('express-session');`

**전역 및 지역 npm 패키지**

강의에서 프로젝트에 `nodemon`을 local dependency로 설정함

local dependency의 장점은 프로젝트를 저장하는 **node_modules 폴더가 없어도** 공유가 가능하다는 것. 그 후에 프로젝트에 `npm install`을 실행해서 node_modules 폴더를 재생성. 이렇게 하면 소스 코드만 공유할 수 있어, 공유 프로젝트의 크기가 엄청나게 줄어듬

첨부된 코드 스니펫 역시 같은 방법으로 공유했기 때문에, 추출된 패키지에 `npm install`을 실행해야 코드를 실행할 수 있음.

Terminal이나 명령줄에는 local dependency가 아니라 글로벌 패키지를 사용하기 때문에, `nodemon app.js`가 작동하지 않음.

**오류 및 디버거 → 익숙한 그것..** 

- syntax : 오타
- runtime : 실행하려고 할 때 멈춤
- logical : 실행은 하는데 원하는 것 처럼 안됨