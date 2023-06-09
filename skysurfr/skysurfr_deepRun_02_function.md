# [02주차] [skysurfr - ch 9~12 Object로 확장하기](https://www.craft.do/s/zf8XHX6L0kaswZ)

## Object (Property 속성)의 개념

> Key는 From, 화살표의 시작

- 책의 목차
- 상가 1층의 호수 안내도
- 부동산 계약서

> Value는 To, 화살표의 대상 그 자체, 그중에는 변하지 않는 실체 Raw Data도 있다

- 실제 페이지 내용
- 실제 상가 위치
- 실제 건물의 위치

---

> Object는 From에서 To 까지, 두가지를 합친 것

- 책 한권 : 목차Index와 책 내용
- 상가 전체
- 계약서와 집을 합친 것

---

### Object의 가능성

#### Key가 가르키는 것이 Value가 아니라 Object 라면?

Key가 오브젝트를 가르키고 또 그안에서 Key가 오브젝트를 가르키고 또 그안에서 Key가 오브젝트를….

#### Key가 Key를 가르키면?

Key → Key 일까?

Key → Value니까 결국

Key,Key → Value인걸까

(두 키가 같은 Value를 가르킨다)

#### `=`  표시로 Value가 바뀌는 거 같지만 사실은 ::key가 가르키는 방향이 바뀌는 것::

값이 바뀐다고 하면 이런 것들이 있을 수 있다

- 대상이 숫자, 글자, true/false 같은 쪼갤 수 없는 단위 - 메모리에 새로 생성해서 그것을 가르킨다
- 가르키는 대상이 Object - 가르키는 오브젝트가 바뀐다
- Key를 가르킨다 - 대상 Key가 같은 것을 보게 된다

#### 그래서 이게 무엇을 하는건데?

**::Value를 Return::**

실제 내용이 있는 메모리에 있는 가치있는 ::데이터들을 어떻게 꺼내서 전달:: 하느냐의 문제

메모리는 사실 집안 정리처럼 잘 정리되어 있는 것이 아니라 섞여있어서 꼬리표(Key)를 단서로 해서 물건(Raw 값) 기능을 잘 복사해서 화면에도 뿌려주고 다른곳에도 전달하고 하는 것

---

### Object의 약점

#### 같은 것을 참조하는 Object의 훼손

서로 다른 두개가 같은 객체를 가르킨다는 건 한쪽에서 바꾸면 다른쪽도 바뀐다는 이야기

객체의 불변성 - 신용을 지킬 수 없단 이야기

#### 얕은 복사

= 화살표만 복사

#### 깊은 복사

내용하고 화살표도 다 분리해서 서로 상관없게

서로 독립성을 지키도록

---

### function의 function

#### 입력 Argument `( )` 이 있고

::변수하고 매개변수 또는 인자 Argument하고 햇갈리지 말자::. 이것은 이름을 맘대로 지을 수 있다

- 입력이 없거나 ()
- 입력이 하나 이상이거나 ( a, b, c ) : 3개 이상 쓰지 말자
- 배열로 받거나 Object로 받거나
- 통으로 다 받거나 (…args)

#### function `{ }` 그 자체

- 계산
- 논리식
- 반복문
- 조건문
- 아니면 그안에 또 function이 있거나
- 변수가 있거나…

등등등이 있는…

#### return : 내보내고 끝낸다

- return을 안쓰고 외부의 처리로 끝낼수 있고
  - DOM Event 발생
  - 다른 function을 실행
  - 외부의 것을 컨트롤
  - 심지어 ::인수(입력받는 것)의 function을 실행할 수도 있고 callback::
- return하면서 어떤 값을 전달할 수도 있고
- 그냥 끝내려고 return 할 수도 있고

> Key가 가르키는게 function?  그것이 Method

---

### function을 어떻게 쓸까

#### function을 function 취급하면 (함수 선언문)

지가 맨 위로 가서 hoisting 먼저 선언되고 함수 역할을 한다

#### function을 변수 취급하면 (함수 표현식)

변수에 할당하니 변수의 특성을 가진다 let, const으로 hoisting 되지만 선언된 장소 전까지 undefined 취급 받다가 선언된 장소에서 function이 객체로 역할을 한다

#### new Function으로 만드는거

instance(어떤 특정 형식을 복사해서 메모리에 오브젝트를 올리는 방식)로 만드는건데, 일반적인 함수와는 다르게 동작한다.

그래서 안쓰는 걸 권장

#### `=>`::화살표 함수::

간편한 익명함수 방식

이름을 변수에 맏기거나 아예 안쓰는 함수

인수가 함수에 의해 적용된다는 걸 직관적으로 보여주고 간단하다

this, argument 등의 동작이 조금 다르다고 한다

이게 요즘 대세

#### function 실행 간단해요

::이름 옆에 `()`:: 만 붙이면 됨, 안붙이면? 그냥 그거 내용

심지어는 function 감싼다음에 `()` 해도 실행됨 - 즉시 실행 함수

#### 내가 나를 부른다

재귀 호출 recursive call 이라고 하는데

그 안에서 자기 자신을 부르면 어떻게 될까? 무한 반복 하겠지

#### 내 안에 나 있다

중첩 함수 Nested Function - 안에서 보조역할을 한다

- 범위 Scope : 어디까지 영향을 줄까
- 클로저 Closure : 다 끝난 함수도 밖에서 안을 부르는 구조면 돌아간다

#### 그 유명한 callback function

::인자(매개 변수)를 function으로 써먹는:: 기발한 방법

외부의 입력을 function으로 받아버리니 그걸 언제 쓰는지 활용하기가 자유로워 진다

호출 횟수, 타이밍 등을 결정할 수 있다

---