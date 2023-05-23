---
marp: true
---

# [모딥다03] 전역변수의 문제점

Global Variables

---

## 변수의 생명주기 Life Cycle

- 전역변수 Global Variables = 앱의 생명주기
- 지역변수 Local Variables 는...

---

### 지역변수 Local Variables 의 영역

지역 변수는 선언된 위치
>Lexical = 선언한 거기 = 명명한 장소 = 화살표 &rarr; 그 자체가 생긴 곳

에서 환경이 끝날때까지 메모리에 남아있다

---

#### 지역변수 호이스팅 Hoisting

스코프 (Scope 범위) 내의 가장 위에서 선언된다
var는 선언과 동시에 할당
let, const는 선언만 하고 할당은 선언된 위치에서

---

#### 지역변수 생명이 끝나는 시점

1. 스코프가 끝나고
2. 누구도 참조하지 않을 때

가비지 컬렉터 Garbage Collector 가 메모리 영역을 정리한다
= 화살표를 치우고 사용할 수 있는 영역으로 바꾼다 (임자가 없는 땅이 된다)

---

### 전역오브젝트 Global Object

코드가 실행되기 이전에 가장 먼저 생성하는 특수 오브젝트

- 브라우저에서는 window
- NodeJS 등 서버 환경에서는 global

JavaScript의 오브젝트는 모두 이들 아래에 있다
(전역오브젝트를 가르키는 식별자는 ES11부터 globalThis로 통일됨)

---

#### 전역오브젝트의 자식들

- 표준 빌트인 Built-in 오브젝트 : Object, String, Number, Function, Array...
- 환경에 따른 호스트 Host 오브젝트 : Web API, NodeJs 호스트 API
- 전역변수 가장 바깥의 함수 function 스코프 밖의 var로 선언한 오브젝트

---
이 console.log의 결과는?
```
var a = 1;
{
    var b = 2;
    var c = 4;
    a = 3;
}

function test() {
    b = 1;
    a = 1;
}
test();
console.log(a, b);
{
    c = 5;
}
console.log(c);
```

---

```
var a = 1;
{
    var b = 2;
    var c = 4;
    a = 3;
}
function test() {
    b = 1;
    a = 1;
}
test();
console.log(a, b); // 1, 1
{
    c = 5;
}
console.log(c); // 5
```

---

#### 전역변수의 문제점 (1)

- 어디서든 사용(참조, 할당) 가능
  - = 암묵적 결합 implicit coupling 을 허용
  - 의도하지 않게 상태변경이 가능
    - 오류 발생 확률이 높다
  - 네임스페이스 Namespace 의 오염
    - 파일로 분리되어 있어도 전역 스코프를 공유하기 때문에,<br> 예상하지 못한 결과를 가져올 수 있다

---

#### 전역변수의 문제점 (2)

- 길어질수록 가독성이 나빠진다
- 긴 생명주기
  - 메모리를 그만큼 잡아먹고 있다
- 스코프 체인 Scope Chain 의 종점에 있다
  - 변수 검색시 가장 마지막에 있다 = 검색 속도가 느리다

---

#### 전역변수 사용 안하기 캠페인(1) : 변수의 스코프는 좁을수록 좋다

- 즉시 실행 함수 ([요즘에 많이 안쓰는 이유](https://www.craft.do/s/vBiJNvVbRFGisY))
  - 코드 전체를 실행 함수 ```( function(){} )()```로 감싸면 정의와 동시에 호출

---

#### 전역변수 사용 안하기 캠페인(2) : 변수의 스코프는 좁을수록 좋다

- 네임스페이스 오브젝트
  - 네임스페이스를 담당할 오브젝트를 생성하고 사용할 변수를 프로퍼티 Property로 추가

```
const NAMESPACE = {};

NAMESPACE.name = "blueskyto";

NAMESPACE.person = {
  name: "future";
  address: "Tokyo";
}

console.log(NAMESPACE.name);
console.log(NAMESPACE.person.name);
```

---

#### 전역변수 사용 안하기 캠페인(3)

- 모듈패턴 Module Pattern : var로 하는거니까 몰라도 됨
- ES6 Module : 이거 쓰세요

---

#### 전역변수 사용 안하기 캠페인(3-1)


예제 1 : HTML에서 쓰기 (확장자 ```.mjs``` 권장)
```
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

예제 2: 우리가 아는 그것
```
import express from "express";

export default rootRouter
```