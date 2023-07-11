# 43 Ajax

Asynchronous Javascript and XML

브라우저가 서버에게 비동기 방식으로 데이터를 요청.

수신받은 데이터를통해 웹페이지를 동적 갱신하는 프로그래밍 방식

XML: HTML 처럼 생긴 데이터 표현 언어

```
<dog>
    <name>식빵</name>
    <family>웰시코기<family>
    <age>1</age>
    <weight>2.14</weight>
</dog>
```

하지만 다들 JSON을 씁니다.

```
{
    "name": "식빵",
    "family": "웰시코기",
    "age": 1,
    "weight": 2.14
}
```

<br>

<div style="display: flex; justify-content:space-between;">
    <div style="margin: 5px; text-align:center;">
        <p>전통적인 웹페이지</p>
        <img 
        src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png"
        />
    </div>
    <div style="margin: 5px; text-align:center;">
        <p>AJAX</p>
        <img  
        src="https://poiemaweb.com/img/ajax-webpage-lifecycle.png"
        />
    </div> 
</div>

<h1>핵심 <br><br> - Page Reload => DOM manipulation 
    <br> 
 - RESPONSE: HTML, CSS, js => JSON</h1>

# JSON

Javascript Object Notation

클라이언트와 서버간 HTTP 통신을 위한 `텍스트 데이터` 포멧

JSON은 자바스크립트 객체리터럴과 유사한 `순수 텍스트`이다.

<h2> 통신으로 주고받는것은 `순수 텍스트`</h2>

## 유투브 복습

form은 순수 텍스트를 전송한다.

```pug
##pug 로그인
form(method="POST")
    input(name="username")
    input(name="password")
    input(type="submit", value="Login")
```

서버로 수신되는 순수 텍스트

```json
{
    body: {
        username: "name"
        password: "mypassword"
    }

}
```

컨트롤러는 req.body 객체에서 username, password를 꺼낸다.

```js
const handleLogin = (req, res) => {
  const { username, password } = req.body;
  // 이하 로그인 로직
};
```

<br>

순수 텍스트인데 어떻게 객체로 받는거지?

<br>

### 순수 텍스트를 객체로 바꿔주는 body-parser

query-string(순수텍스트) 를 익스프레스 내장 메서드로 변환해준다

```js
app.use(express.urlencoded({ extended: true })); // x-www-form-urlencoded
app.use(express.json()); //JSON

// 밑에 라우터 코드
```

## JSON 메서드

<h3>JSON.stringify</h3>

- 객체를 JSON 형식의 문자열로 변환한다

- 주로 브라우저에 데이터를 전송할때 사용

- 변환없이 전송시 `[Object]` 이런 텍스트가 전송된다

  <br>

<h3>JSON.parse</h3>

- JSON 데이터를 가진 문자열을 객체로 변환한다.

- 주로 수신된 데이터를 객체로 복원할때 사용.

- 수신후 복원을 안하면 객체모양의 `string`을 사용하게된다 `{ username: "young-jin"}`

<h3>toJSON()</h3>

- Date 객체의 데이터를 JSON 형식의 문자열로 변환하여 반환

- 접미사 Z로 식별되는 UTC 표준 시간대의 날짜를 ISO 8601 형식의 문자열로 반환

- 문자열은 언제나 24개나 27개의 문자로 구성

  ```
  YYYY-MM-DDTHH:mm:ss.sssZ

  ±YYYYYY-MM-DDTHH:mm:ss.sssZ
  ```

### Momentum 복습

<br>

```js
const saveTodo = function () {
  localStorage.setItem(TODOS_KEY, JSON.stringify(todoStorage));
};

const loadTodo = function () {
  todoStorage = {};
  const todoData = localStorage.getItem(TODOS_KEY);
  const loadedObj = JSON.parse(todoData);
  if (loadedObj !== null) {
    const keyArray = Object.keys(loadedObj);
    keyArray.forEach((key) => {
      const todo = loadedObj[key];
      todoStorage[key] = todo;
      paintTodo(todo, key);
    });
  }
};
```

# XMLHTTPRequest 객체로 fetch를 대체해보자.

---

# Fetch

```
Fetch Api는 프로토콜의 요청 및 응답(requests and responses)과 같은 부분에
대한 접근 및 조작 기능을 제공하는 자바스크립트 인터페이스입니다.

fetch() 메서드를 사용한다면 네트워크를 가로질러
비동기적으로 resources(자원, 정보)를 가져오는 쉽고 논리적인 방법을 제공합니다.

과거 이러한 기능은 XMLHttpRequest를 통해 수행되곤 했습니다.
하지만 Fetch가 더 나은 대안입니다... (중략)
```

---

# Fetch 코드

```
fetch(url)
    .then((response) => response.json())
    .then((data) => {
      city.innerText = data.name;
      weather.innerText = `${data.weather[0].main} / ${data.main.temp}`;
    });
```

---

# XMLHTTPRequset 코드

```
const req = new XMLHttpRequest(); // 1. 생성

req.onreadystatechange = () =>{                    // 2. 이벤트 리스너
  if (req.readyState === XMLHttpRequest.DONE) {
    const data = JSON.parse(req.responseText);

    city.innerText = data.name;
    weather.innerText = `${data.weather[0].main} / ${data.main.temp}`
  }
}

req.open("get", url); // 3. HTTP 통신 설정
req.send(); // 4. 전송요청
```

---

# open 과 send 메서드

- open: HTTP 통신 설정 초기화

```
req.open("get", url);
```

- send: 전송코드, 요청을 XMLHTTPRequest 객체에 반환

```
req.send(); // 4. 전송요청
```

    - 반환 응답은 responseText 속성에 저장.

---

# onreadystatechange ? readyState?

readyState: 통신상태를 나타내는 XMLHTTPRequest의 속성

```
0: "UNSENT" - XHR 오브젝트가 생성, open 메서드가 실행전

1: "OPENED" - open 메서드가 실행. 하지만 send메서드는 실행전

2: "HEADER_RECEIVED"  - send 메서드가 실행, 요청 전송.

3: "LOADING" -  응답이 도착중.

4: "DONE" - 전체 응답이 도착, 사용가능.
```

onreadystatechange: readystate의 변화를 감지하는 이벤트 리스너

---

```
const req = new XMLHttpRequest(); // 1. 생성

req.onreadystatechange = () =>{                    // 2. 이벤트 리스너
  if (req.readyState === XMLHttpRequest.DONE) {
    const data = JSON.parse(req.responseText);

    city.innerText = data.name;
    weather.innerText = `${data.weather[0].main} / ${data.main.temp}`
  }
}

req.open("get", url); // 3. HTTP 통신 설정
req.send(); // 4. 전송요청
```

---

# 왜 XMLHTTPRequest는 Fetch에 의해 대체되는가

### 1. 응답을 받아오는 방식의 차이

-XMLHTTPRequest : 객체내부에 responseText로 받아온다

  <br>
  
  -Fetch: Promise를 반환한다

### 2. Fetch는 no-cors 요청이 가능하다

- cors(cross origin request sharing) 서로다른 api의 자원을 공유하는것.

---

참고자료

https://developer.mozilla.org/ko/docs/Web/HTTP/CORS
https://web.dev/why-coop-coep/
http://www.tcpschool.com/json/json_use_js
