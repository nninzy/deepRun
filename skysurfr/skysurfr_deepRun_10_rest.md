# REST API

REpresentational State Transfer

HTTP의 장점을 최대한 활용할 수 있는 아키텍처

REST의 기본 원칙을 성실히 지킨 서비스 디자인은 RESTful 이라고 표현

## REST API 구성

- Resource 자원 : 자원 : URI(엔드포인트)
- Verb 행위 : 자원에 대한 행위(HTTP 요청 메서드)
- Representations 표현 : 자원에 대한 행위에 대한 구체적인 표현(페이로드)

REST는 자체 표현 구조 self descriptiveness로 REST API 만으로 HTTP 요청을 이해할 수 있다

## REST API 설계 원칙

**URI는 리소스를 표현** 하는데 집중하고 **행위에 대한 정의는 HTTP 요청 메서드**를 통해 하는 것

1. URI는 리소스를 표현해야 한다
   1. 리소스를 표현하는데 중점
   2. 동사보다는 명사를 사용한다
2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다
   1. 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법
   2. GET/POST/PUT/PATCH/DELETE 를 사용하여 CRUD를 구현

## Fetch

### GET

```JavaScript
const url = "https://jsonplaceholder.typicode.com/todos/1";

const data = fetch(url, {
  method: "GET",
  headers: {"content-type": "application/json"}
})
  .then(response => response.json())
  .then(json => JSON.stringify(json))
```

### POST

```JavaScript
const url = "https://jsonplaceholder.typicode.com/"

const data = fetch(url, {
  method: "POST",
  headers: {"content-type": "application/json"},
  body: JSON.stringify({
    id: 1,
    title: "JavaScript",
    completed: true
  }),
})
  .then(response => console.log(response);
```
