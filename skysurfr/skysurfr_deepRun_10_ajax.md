# Ajax and JSON

## Ajax : Asynchronous JavaScript and XML

1. 자바스크립트를 사용하여
2. 브라우저가 서버에게
3. 비동기방식으로
4. 데이터를 요청하고
5. 서버가 응답한 데이터를 수신하여
6. 웹페이지를 동적으로 갱신

- Web API인 XMLHttpRequest 오브젝트를 기반으로 동작
- 웹페이지 변경에 필요한 데이터만 비동기 방식으로 전송받아 변경할 필요가 있는 부분만 한정적으로 렌더링

### Ajax의 장점

- 필요한 부분의 데이터만 서버에서 전송 = 불필요한 데이터 통신이 발생하지 않는다
- 변경할 필요가 없는 부분은 다시 렌더링하지 않는다 = 순간적으로 화면이 깜빡이는 현상이 발생하지 않는다
- 클라이언트 서버 비동기 방식 통신 = 서버에게 요청을 보낸 이후 블로킹(동기 방식에서 일어나는)이 생기지 않는다

## JSON : JavaScript Object Notation

클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷

```JSON
{
  "name" : "Min Woo",
  "age" : 24,
  "alive" : true,
  "hobby" : ["Cafe", "Coding"]
}
```

```JSON
[
  {"id" : "JavaScript", completed: true },
  {"id" : "TypeScript", completed: true },
  {"id" : "Python", completed: false }
]
```

### JSON.stringify(obj|array)

오브젝트/배열을 JSON 포맷의 문자열로 변환(직렬화 serializing)

### JSON.parse(json)

JSON 포맷의 문자열을 오브젝트/배열화 (역직렬화 deserializing)

## XMLHttpRequest

자바스크립트를 통해 HTTP 요청 전송

### HTTP 요청 전송

```JavaScript
const xhr = new XMLHttpRequest(); // xhr 오브젝트 생성

xhr.open("GET", "/users"); // HTTP 요청 초기화
xhr.setRequestHeader("content-type", "application/json"); // 헤더 MIME 타입 지정
xhr.send() // 요청 전송
```

```JavaScript
const xhr = new XMLHttpRequest();

xhr.open("POST", "/users"); // POST 방식
xhr.setRequestHeader("content-type", "application/json");
xhr.send(JSON.stringify({id:1,content:"HTML",complete:false}));
```

#### HTTP 요청 메서드

- GET : index/retrieve : 모든 특정 리소스 취득 - URL 쿼리 문자열로 전달
- POST : create : 리소스 생성(페이로드 있음) - 페이로드를 request body(헤더의 MIME Type에 맞춰)에 담아 전달
- PUT : replace : 리소스 전체 교체(페이로드 있음)
- PATCH : modify : 리소스 일부 수정(페이로드 있음)
- DELETE : delete : 모든/특정 리소스 삭제

[Payload](<https://en.wikipedia.org/wiki/Payload_(computing)>)

> In computing and telecommunications, the payload is the part of transmitted data that is the actual intended message. Headers and metadata are sent only to enable payload delivery.

#### MIME Type

- text : text/plain, text/html, text/css, text/javascript
- application : application/json, application/x-www-form-urlencode
- multipart : multipart/formed-data

### HTTP 응답 처리

이벤트 캐치는 Web API 라서 브라우저에서만 가능
readyStateChange 이벤트를 캐치해서 HTTP응답을 처리할 수 있음

```JavaScript
const xhr = new XMLHttpRequest();

xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");
xhr.send();
xhr.onreadystatechange = () => {
  if (xhr.readyState !== XMLHttpRequest.DON) return;
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
}
```

```JavaScript
const xhr = new XMLHttpRequest();

xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");
xhr.send();
xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
}
```
