---
marp: true
---

# [모딥다04] 생성자 함수에 의한 오브젝트 생성

constructor
>One who constructs or makes; specifically, a builder.

---
![bg left:30%](assets/images/kevin-engelke-crPBQpJjz50-unsplash.jpg)

## 오브젝트 생성자 함수 Object Constructor

오브젝트 리터럴(수식같은거)로 오브젝트를 만드는 방법 외에
오브젝트(틀-구조)를 인스턴스 instance(내용물)로 생성하는 방법

> 빈 객체 복사해서 내용물 넣는 거

특성상 필요한 경우 아니면 많이 쓰지는 않을거 같음
(RegExp, Date 등)

사진: [Unsplash](https://unsplash.com/ko/%EC%82%AC%EC%A7%84/crPBQpJjz50?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)의[Kevin Engelke](https://unsplash.com/@twopixelsdesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

---

### 오브젝트 리터럴과 비교

- 오브젝트 리터럴 : 한번에 하나의 오브젝트만 생성, 수식을 쓰면 바로 생성되기 때문에 직관적
- 오브젝트 생성자 함수 방식 : 구조가 동일하지만 내용물이 다른 여러개의 오브젝트 생성에 유리

---

### Where is ```this```?

자기 참조 변수 ```this```

- 함수 방식 호출 : 전역 오브젝트 Global Object
  - 데이터 프로퍼티이기 때문에,
    - 자기 안에서는 참조할 놈이 없다 &rarr; 그래서 global이 ```this```
- 메서드 방식 호출 : 메서드를 호출한 오브젝트(부른놈);
  - 접근자 프로퍼티이기 떄문에,
    - 접근한 오브젝트가 ```this```(점 앞에 있는 그것)
- 생성자 방식 호출 :
  - 생성할 인스턴스 = 아직 만들어지지 않은 뭔가 채워질 오브젝트
  - 상식적으로 생성자 자체가 ```this```가 되면 대혼돈이 생기지 않을까

---

#### 번외편 Event의 ```this```

DOM Event 에서 함수형의  ```this```는 Event의 타깃 DOM이 global 역할을 한다
 : click event라면 click 한 엘리먼트 Element가 ```this```
 : 이것은 이벤트 버블링과 캡처링```event.stopPropagation()``` 등과도 관련이 있지만,

[velog : 이벤트 핸들러 내부의 ```this```](https://velog.io/@ursr0706/이벤트-핸들러-내부의-this-2gyujqm2) 글 참조
[버블링과 캡처링](https://ko.javascript.info/bubbling-and-capturing) 글 참조

---

![bg contain](assets/images/c77f4246-2f86-443c-85b1-221bc5a12f20-1238709634.jpg)

---

### 생성자 함수의 인스턴스 생성 과정

1. (생성자 그 자체의 함수를 복사한) 빈 객체=인스턴스 암묵적으로 생성
2. ```this```에 Binding 된 인스턴스 한줄씩 읽으며 초기화
3. 인스턴스의 ```this```가 암묵적으로 return

---

#### 예제

```/* JavaScript */
function SquarePants(centimeter) {
  // 1. (생성자 그 자체의 함수를 복사한) 빈 객체=인스턴스 암묵적으로 생성

  // 2. this에 Binding 된 인스턴스 한줄씩 읽으며 초기화
  this.centimeter = centimeter;
  this.getPants = () => {
    return this.centimeter * this.centimeter
  }

  인스턴스의 this가 암묵적으로 return
}

const spongeBob = new SquarePants(5);
console.log(spongeBob); // Square {centimeter: 5, getPants: ƒ}
console.log(spongeBob.getPants()); // 25

```

---

### 생성자 함수랑 그냥 함수랑 똑같이 생겼는데

function 쓰고, ```.```(dot) 없이 호출할 수 있는 형식이면

&rarr; 함수를 호출할 수도 있고 callable,
&rarr; new로 생성할 수도 있다 constructor

그러니까 실수할 수 있다는...(파스칼 케이스 잘 쓰면 되지만...)

```/* JavaScript */
function foo() {}

foo(); // 함수 오브젝트 내부 메서드 [[Call]] 호출 call

new foo(); // 생성자 오브젝트 내부 메서드  [[Constructor]] 생성 constructor

```

---

#### ```new``` 못씀 두가지

이거랑 화살표 함수는 new 못쓴다

```/* function */
const bar = {
  x: function() { return 10 }
}
new bar.x();
// x {} 리턴 (이게 뭐지?)

const foobar = {
  z() {}
}
new foobar.z();
// type error

```

---

#### ```new.target```

메타 프로퍼티
- 생성자 함수로 호출하면 함수 자신 반환
- 아니면 undefined

if문으로 판단해서 생성자로 리턴 재귀 호출하면 그냥 써도 된다

---

##### 예제
```/* function */
function SquarePants(cm) {
  if(!new.target) {
    return new SquarePants(cm);
  }
  this.cm = cm;
  this.getPants = () => {
    return this.cm * this.cm;
  }
}

const spongeBob = SquarePants(5);
console.log(spongeBob.getPants());

```

---

### 빌트인 생성자 함수

Object, String, Number, Boolean, Function, Array, Data, RegExp, Promise
new 랑 같이 쓰면 객체 반환하지만

String, Number, Boolean은
각 원시 타입 Raw Type 반환한다
