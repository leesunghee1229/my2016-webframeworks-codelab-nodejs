ExpressJS
=================

## 익스프레스를 사용하는 이유

익스프레스는 노드로 만든 패키지의 일종인데요 웹 서버를 만들기위한 것이라고 볼수 있습니다. 자세한 사용법은 [http://expressjs.com/](http://expressjs.com/)에 잘 나와있습니다. 그런데 질문. API 서버를 만드는데 왜 익스프레스를 사용하려는 것일까요?

우리가 만들 API 서버에 대해 다시 생각해 봅시다. 서버는 클라이언트의 어떠한 요청이 있을 경우 서버에서 자원을 처리한뒤 결과를 다시 클라이언트로 보내줍니다. 클라이언트는 서버에게 요청할 때 API라는 것을 통해서 요청할수 있는데 어떤 일정한 규칙이 있어야 합니다. 한국사람이 중국 타오바오에 전화해서 한국말로 지껄일수는 없기 때문이죠.

클라이언트와 서버는 HTTP라는 규칙을 이용해서 서로 통신하게 됩니다. 웹에서도 이 HTTP를 이용해 페이지를 주고 받습니다. 익스프레스가 웹 프레임웍이긴 하지만 똑같은 HTTP 기반의 서비스를 개발하는 것이기 때문에 API 서버에서도 사용하는 것입니다.


## 익스프레스 설치

익스프레스도 일종의 노드 패키지이기 때문에 `npm`으로 설치할 수 있습니다. 아래 명령어를 실행하여 프로젝트에 익스프레스 패키지를 추가해 봅시다.

```
npm install express --save
```

익스프레스를 설치하면 패키지 파일의 `dependencies`에 "express" 문자열이 추가 되었습니다. 그리고` node_modules` 폴더가 생성되었을 겁니다. `npm`을 통해 설치한 노드 패키지들은 이 폴더에 저장되고 노드는 이 폴더를 찾아서 모듈을 로딩합니다.


## 익스프레스 Hello world로 변경하기

기존의 `app.js`를 익스프레스 패키지를 이용해서 만들어 봅시다. [익스프레스 홈페이지](http://expressjs.com/en/starter/hello-world.html)에 샘플코드가 있습니다. 우리는 이미 `const`와 `arrow function` 등의 ES6 문법을 익혔으니 홈페이지에서 제공하는 코드를 살짝 변경해 보도록 하지요.

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!\n');
 });

app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```

서버를 구동하고 curl로 요청하면 "Hello world" 문자열이 터미널에 찍힐 것입니다.

```
npm start
Example app listening on port 3000!
```

```
curl -X GET '127.0.0.1:3000'
Hello World!
```


## 익스프레스의 주요 개념

익스프레스는 크게 네 가지 부분으로 이해하면 됩니다.

* Application
* Request
* Response
* Routing


## Application

불러온 익스프레스 객체에는 하나의 함수가 할당되는데 그 함수를 실행하면 익스프레스 객체가 생성됩니다. 익스프레스 클래스를 이용해 익스프레스 객체를 만든다고 생각하면 됩니다. 이것을 익스프레스 어플리케이션(Application)이라고 하는데 우리 코드에서는 `app` 상수에 할당했습니다.

```javascript
const express = require('express');
const app = express();
```

익스프레스 인스턴스가 하나의 서버역할을 하는데 크게 보면 서버를 세팅하고 서버를 구동하는 역할을 합니다.

서버를 세팅하는 것은 서버에 필요한 기능을 추가한다고 볼 수 있는데 익스프레스에서 서버의 기능은 미들웨어 형태로 존재합니다. 그리고 이 미들웨어에서 익스프레스 인스턴스의 `use()` 함수로 추가할 수 있습니다. 예를 들어 서버에서 정적파일(static file)를 호스팅할 때는 다음과 같이 정적파일설정을 위한 미들웨어를 추가할 수 있습니다.

```javascript
app.use(express.static('public'));
```

앞으로 API 서버 기능을 확장하면서 필요한 미들웨어를 찾아서 추가할 것입니다. 아직 헬로 월드 코드에서는 미들웨어를 사용하지 않았습니다

어플리케이션의 또 하나의 기능은 서버를 구동하는 역할인데요 아래 코드가 그 역할을 수행합니다.

```javascript
app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```

익스프레스 인스턴스의 `listen()` 함수를 이용해 서버가 클라이언트의 요청 대기 상태로 들어갔습니다. 첫번째 파라매터 `3000`이 대기할 포트 번호입니다. 두번째 파라매터는 함수인데 `listen()` 이 완료되면 실행되는 콜백함수입니다. 이 콜백함수가 호출되면 서버 구동이 완료되었다고 판단할 수 있습니다. 그리고 서버가 구동되었다는 메세지를 터미널에 출력합니다.

어플리케이션은 라우팅 기능도 수행합니다. 서버는 나름대로 자원을 관리하는 몇가지 기능을 가지고 있을 것입니다. 만약 클라이언트로부터 어떤 요청이 있을때 서버는 가지고 있는 기능 중에 이에 적절한 것을 찾아서 응답해 줘야하는데 이 두가지를 연결해 주는 것을 라우팅이라고 합니다.

라우팅: 클라이언트 요청과 서버의 로직을 연결하는것

우리는 '/' 요청에 대해 다음과 같이 라우팅 로직을 설정하였습니다.

```javascript
app.get('/', (req, res) => {
  res.send('Hello World!\n');
 });
```

`app.get()` 함수를 이용해 요청 메소드가 GET 이라는 것을 설정합니다. 그 첫번째 파라매터로 경로를 설정했구요. 그리고 이러한 요청이 들어왔을 경우 두번째 파라매터인 콜백 함수가 동작하도록 만들었습니다. 콜백함수는 `req`, `res` 두개 파라매터를 받는데요 이 부분은 다음 설명에서 이어집니다.


## Request

콜백함수에서 전달해 주는 1번째 파라매터 `req`는 익스프레스 요청(Reqeust) 객체라고 합니다. 요청 객체는 말 그대로 서버로 요청한 클라이언트의 정보에 대해 담고 있습니다. 하나의 객체 형태로 되어 있는데 키와 함수들로 구성되어 있습니다. 우리가 사용하는 키는 아래의 것들입니다.

* `req.params`: url 파라매터 정보를 조회
* `req.query`: 쿼리 문자열을 조회
* `req.body`: 요청 바디를 조회


## Response

콜백함수에서 전달해 주는 2번째 파라매터 `res`는 익스프레스 응답 객체(Response)라고 합니다. 응답 객체는 요청한 클라이언트에게 응답하기 위한 함수들로 구성된 객체입니다. 우리는 아래 함수들을 사용할 거에요.

* res.send()
* res.json()
* res.status()


## Router

어플리케이션을 이용해 라우팅 로직을 만들 수 있지만 익스프레스에는 별도로 `Router()` 클래스를 제공합니다. 라우터 클래스를 이용하면 라우팅 로직을 좀더 구조적으로 만들 수 있습니다. 자세한 사용법은 이후에 계속 설명하겠습니다.

```
git checkout express
```