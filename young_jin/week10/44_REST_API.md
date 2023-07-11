# REST API

## 1. URI는 정보의 자원을 표현해야 한다.

<br>

- 리소스명은 동사보다는 명사를 사용한다.

- URI는 자원을 표현하는데 중점을 두어야 한다.

- 행위에 대한 표현이 들어가서는 안된다.

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

## 2. 자원에 대한 행위는 HTTP Method로 표현한다.

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```

| Method | Action         | 역할                    | 페이로드 |
| ------ | -------------- | ----------------------- | -------- |
| GET    | index/retrieve | 모든/특정 리소스를 조회 | x        |
| POST   | create         | 리소스를 생성           | ○        |
| PUT    | replace        | 리소스의 전체를 교체    | ○        |
| PATCH  | modify         | 리소스의 일부를 수정    | ○        |
| DELETE | delete         | 모든/특정 리소스를 삭제 | x        |

- 페이로드: 요청과 함께 보내는 데이터

# REST?

Rest API의 역사 및 정의

### Definition

Representational State Transfer

표현 가능한 상태 전송

- 수신자는 별도의 배경지식 없이, 전송된 내용만으로 해독 할 수있어야한다.

- State

  - 변수, 값을 상태라고 하는데 객체지향의 영향
  - 잘 와닿지 않으면 체내 수분 비율, 머리카락 갯수 등등을 생각해보자

### REST 의 목표

```
    a way of providing interoperability between
    computer systems on the internet
```

컴퓨터 시스템의 상호 운용성을 제공하는 방법.

### 요청만으로 완결성이 있어야한다.

- 추가적인 사전 지식을 요구하면 안된다.

- 사람에게 요청을 보낼때
  - 사람이 추가 질의로 요청 handle 가능

```

1. 담배 줘

- 품목이 불분명함, 의사가 불분명함 - 구입? 갈취?

2. 에쎄

- 품목이 불분명함

```

컴퓨터에게 요청을 보낼 때

```

요청: 결제
품목: 담배 에쎄 수 니코틴 0.5 mg
수량: 2
결제수단: 신용 카드 일시불

```

## 역사: RestAPI의 등장배경

<br>

### Web 전송에 사용하는 표준 도구의 등장

Q: 어떻게 인터넷에서 정보를 잘 공유할까?

A: 정보를 하이퍼 텍스트로 연결하자

    - 텍스트에 링크를 포함하는 텍스트

표현형식: HTML
식별자: URI
전송방법: HTTP

| 구성 요소       | 내용                    | 표현 방법             |
| --------------- | ----------------------- | --------------------- |
| Resource        | 자원                    | HTTP URI              |
| Verb            | 자원에 대한 행위        | HTTP Method           |
| Representations | 자원에 대한 행위의 내용 | HTTP Message Pay Load |

<br>

### 표준 도구 사용의 표준 등장

<br>

Q. HTTP를 사용한 통신로직을 유지보수가 쉽게 코딩할 수 있을까요?

A. 우리끼리 스타일가이드를 정합시다

- HTTP OBJECT MODEL
  - REST로 명명

https://www.ics.uci.edu/~fielding/pubs/dissertation/top.html

<br>

## REST의 요구사항

1. client-server

클라이언트와 서버로 구성

2. stateless

연결은 불연속적, 연결상태 유지 X

![](https://blog.kakaocdn.net/dn/UVeDC/btrmmckE5KW/2q2D4fL9k0v1lD8P488iRk/img.png)

3.  cache

```

클라이언트가 response를 캐싱 할 수 있는지 여부를 표시

//HTTP Header에 cache-control 헤더를 이용

```

4. uniform interface

```
통합 인터페이스 사용

리소스: URI

리소스 조작 방식: HTTP 요청

HTTP 요청의 자기설명: 수신자는 요청의 정보만으로 모든것을 알 수 있다.

hypermedia as the engine of application state (HATEOAS)

-하이퍼미디어는 앱의 상태를 표현 및 조작하는 도구.
```

5. Layerd System

```

시스템이 분리
클라이언트 / 서버 서로 연결되어 통신한다는 사실만 알고
그 중간과정은 은닉한다.

```

6. code-on-demand

```
서버가 코드를 제공하면, 클라이언트에서 실행할 수 있다.
우리가 만든 거의 모든 자바스크립트 코드
```

### Rest를 달성하면 좋은점

<br>

Rest의 목표

```

발신자의 요청 및 전송되는 값이 무엇인지
배경지식이 없어도 수신자가 알아서 해독 할 수 있다.

```

- `REST`를 달성하면 뭐가 좋은가?

  - 클라이언트와 서버의 독립적 진화

  - 서버나 클라이언트한쪽의 로직이 바뀌어도 다른쪽도 바꿔줄 필요는 없어야 한다.

## REST의 성공사례와 실패사례

<br>

### 웹 생태계와 앱 생태계

<br>

### 웹은 잘 지킨다.

    - 페이지 변경해도 브라우저 업데이트 필요 X

    - 브라우저 업데이트해도 기존 웹페이지 변경 X

    - HTTP 명세가 바뀌어도 웹은 문제없다.

    - HTML 명세가 변경되어도 웹은 문제없다.

### 앱은 잘 못지킨다.

    - 최신버전으로 업데이트하지않으면 실행할 수 없습니다.

- 리액트 네이티브에서는 어느정도 달성가능

<br>

### REST의 영향

<br>

- HTTP에 지속적으로 영향

  - HTTP Request에 HOST 헤더 추가
    - 요청에대한 설명

  <br>

- 여러가지 웹 엔지니어링 개념에 영향

  - URI의 리소스개념의 추상화: 웹페이지 주소 -> 요청받는 자원

    - URI의 하위 개념으로 URL, URN, URC 등등이 분화

    - 웹페이지 주소는 이제 URL이라고 부른다.

### 우리의 REST API가 true REST가 아닌이유

<br>

현재 rest api라고 하는것들은

4번 `uniform interface`의 제약조건을 잘 지키지못한다

1. identification of resources

   - URI를 통해 접근하는 자원을 식별할 수 있어야한다.

<br>

2. manipulation of resources through representations

   - 자원 조작 표현이 들어가있어야한다.

<br>

3. self-descriptive message

   - 스스로의 의미를 설명해야 한다.

     - HOST 주소 / Content-Type

<br>

4. hypermedia as the engine of application state (HATEOAS)

- 애플리케이션의 상태가 Hyperlink를 통해 전이되어야한다.

  - 웹페이지 이동은 하이퍼링크를 통해서 해야한다는 뜻

<br>

## 이유 self-descriptive message

### 범인은 AJAX

1999년 발표된 AJAX는 2005년에 사용가능성이 처음 재기

-> 이후 점차 사용이 늘어남

-> Rest는 2000년생

<br>

before Ajax

- 서버로부터 받은 데이터로 수정한 HTML 클라이언트에게 재전송

with Ajax

- JSON으로 특정데이터만 클라이언트에게 전송

<br>

self-descriptive message 의 결여

<br>

|            | 웹   | HTTP API |
| ---------- | ---- | -------- |
| Protocol   | HTTP | HTTP     |
| Media Type | HTML | JSON     |

<br>

HTML: 수신자가 이해가능

```html
<!--HTML -->

<a href="/logs"></a>

<!--<a> 클릭시 `/logs` 자원에게 get 요청 발생하겠군 -->

<div class="article">
  <p id="title">The second Article</p>
  <pre> blah... </pre>
</div>

<!--글을 추가하는 태그군-->
```

JSON: 프론트 / 백엔드가 사전에 조율해야함

```json
// JSON

{
  "title": "The second Article",
  "contents": "blah"
}

// 이게뭔데...
```

<br>

### RESTful 하게 JSON 전송하기

<br>

JSON 데이터도 `self-descriptive message` 를 첨부할 수 있다.

1. Media type을 IANA에 등록, 타입정보를 전송

- https://www.iana.org/

2. JSON에 해당 데이터타입을 설명하는 문서 하이퍼링크 첨부

```json
{
  "data": {
    "title": "The second Article",
    "contents": "blah",
    "self": "http://localhost:3000/api/article/2", // 현재 api 주소
    "profile": "http://localhost:3000/docs#query-article" // 해당 api의 문서
  }
}
```

<br>

## 엄밀한 REST가 중요한가?

<br>

NO! 다음의 경우에는 REST가 오히려 시간낭비

<br>

1. 시스템 전체를 통제 가능

2. 서버와 클라이언트의 독립적 진화에 관심없음

### 시스템 전체를 통제가능

<br>

- 내가 api, 라우터 , 브라우저 다설계해서, 딱히 다른 인력과 소통할 필요없다.

<br>

### 서버와 클라이언트의 독립적 진화에 무관심

<br>

- 서버 로직이 조금 바뀜 기존 어플 실행 금지, 사용자 한테 업데이트 강요하면 된다.

<br>

참고자료: 그런 REST로 괜찮은가? https://deview.kr/2017/schedule/212
