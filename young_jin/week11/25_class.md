# 25. Class

## 객체지향 프로그래밍: Object-Oriented Programming, OOP

<br>

프로그램을 상태와 행동을 가진 객체들의 상호작용으로 보는것

- 객체지향의 기본은 class가 아니다.

        단지 class가 유명할 뿐이다.

<br>

### 추상화/상속/다형성/캡슐화

<br>

객체를 사용하여 위의 4가지의 특징을 만족하는
프로그램을 작성하는것이 객체지향

<br>

## 객체지형의 4가지 특징

<br>

### 1. 추상화(Abstration)

나타내고 싶은것의 `상태와 행동`을 적절하게 `속성과 메서드`로 표현한것.

<br>

노마드 스터디 학생의 추상화

| 상태 | 행동               |
| ---- | ------------------ |
| 수료 | 일일 스프린트 작성 |
| 속성 | 메서드             |

```js
const student = {
  certificated: ["python web-scrapper", "banilla-js"],
  writeSprint: function (...rest) {
    rest.forEach((item, index) => {
      console.log(`${index}: ${item}`);
    });
  },
};
```

<br>

```s
student.certificated
(2) ['python web-scrapper', 'banilla-js']

student.writeSprint("과제확인", "강의수강 및 과제", "조별활동 참여");

0: 과제확인
1: 강의수강 및 과제
2: 조별활동 참여
```

<br>

### 2. 상속(Inheritance)

<br>

기존의 객체를 재활용하여 새로운 객체를 작성

<br>

```js
function User(name, ...certificated) {
  this.name = name;
  this.certificated = certificated;
  this.writeSprint = function (...rest) {
    rest.forEach((item, index) => {
      console.log(`${index}: ${item}`);
    });
  };
}

const student = new User("burger-king", "wetube-clone", "kokoa-clone");
```

<br>

사용: student 인스턴스 객체는 User의 속성과 메서드를 가지고있다.

```s
student.name //'burger-king'

student.certificated //(2) ['wetube-clone', 'kokoa-clone']

student.writeSprint("식사 주문", "취식","디저트 주문", "섭취");

0: 식사 주문
1: 취식
2: 디저트 주문
3: 섭취
```

<br>

### 프로토 타입을 통한 상태 및 메서드 상속

<br>

인스턴스는 `__proto__`

생성자 함수는 `prototype`으로 각각의 프로토 타입에 접근가능하다.

```js
student.__proto__;

// {constructor: ƒ}
// constructor : ƒ User(name, ...certificated)
// [[Prototype]]: Object

User.prototype;

// {constructor: ƒ}
// constructor: ƒ User(name, ...certificated)
// [[Prototype]]: Object
```

프로토타입 체인
![](https://poiemaweb.com/img/extension_prototype.png)

<br>

### 3. 다형성(polymorphism)

<br>

- 정적 다형성: 메서드 오버로딩

         하나의 메서드가 여러 타입의 매개변수를 가질 수 있다.

<br>

- 동적 다형성: 메서드 오버라이딩

        하나의 이름이지만, 기능을 덮어써서 다른 기능을 갖는다.

<br>

### 4. 캡슐화(Encapsulation)

<br>

서로 연관있는 속성과 기능들을 묶어 캡슐로 만든후,

외부 접근으로 부터 보호하는것

<br>

- 데이터 보호(data protection): 내부 동작을 통해서만 변경가능

- 데이터 은닉(data hiding): 필요한 부분만 노출

<br>

자바스크립트는 클로저로 가능하다.

```js
const bodyFunction = () => {
  let closureOnly = "클로저로만 접근가능";

  return function closure(addition) {
    console.log(`${closureOnly} + ${addition}`);
    closureOnly += addition;
  };
};

const savedClosure = bodyFunction();

savedClosure("진짜루?");
//클로저로만 접근가능 + 진짜루?

savedClosure("넵");
// 클로저로만 접근가능진짜루? + 넵

savedClosure("확인하시죠");
//클로저로만 접근가능진짜루?넵 + 확인하시죠
```

<br>

### 클래스가 도입된 이유

<br>

class 를 기반으로 한 객체지향 프로그래밍이 득세

<br>

자바스크립트 진영에

다른 언어 사용자가 쉽게 합류할 수 있게 하기위함

## 클래스 사용법

<br>

### 1. 선언하는법

`class` 키워드를 이용한다.

속성은 constructor 내부에 작성한다.
메서드는 외부에 작성한다.

```js
class 클래스이름 {
  // 속성은 반드시 constructor 내부에
  constructor(name) {
    this.name = name;
  }

  hello() {
    console.log("hello");
  }
}
```

### 2. 인스턴스 생성법

기존의 생성자 함수와 똑같다

```js
const a = new 클래스이름(초기화값);
```

- 클래스도 함수고, 기존 prototype 패턴을 사용한다.

```js
class ThisIsFunction {}

typeof ThisIsFunction;
//'function'
```

### 3. 클래스 상속

<br>

DRY 원칙: don't repeat yourself에 아주 적합하다.

<br>

### 4. super

<br>

부모 클래스를 참조, 부모 클래스를 호출할때 사용한다.

- constructor 내부에서 메서드로 쓰임

```js
class Parent {
  constructor(ancestor) {
    this.ancestor = ancestor + " from africa";
  }
}

class Child extends Parent {
  constructor(ancestor, name) {
    super(ancestor);
    this.name = name;
  }

  sayHi() {
    console.log(`my ancestor ${super.ancestor}, and my name is ${this.name}`);
  }
}

const writer = new Child("kunta-kinte", "alex-haley");
```

<br>

### 5. static method

<br>

클래스 내부에 선언하는 메서드, 인스턴스가 아니라 클래스를 통해 호출한다.

```js
class Parent {
  constructor(lastName) {
    this.lastName = lastName;
  }

  static staticMethod() {
    return "staticMethod";
  }

  prototypeMethod() {
    return this.lastName;
  }
}

//클래스로 호출
Parent.staticMethod();
// "staticMethod"

//인스턴스로 호출불가
const parent = new Parent("Kim");
parent.staticMethod();
// Uncaught TypeError: foo.staticMethod is not a function
```

### 객체지향의 사실과 오해

1. 클래스중심으로 생각하지 말고 객체중심으로 생각해라.

- 클래스는 그냥 타입을 구현하는 도구

2. `객체는 현실상태의 추상화` 는 잊어라.

- 그냥 이해하기 쉬운 비유이다. 실제로는 전혀다르다.

3. 구현이아니라 인터페이스에 집중해라.

- 객체가 어떤 기능을 수행할지를 설계하고, 구현은 나중에 해라

- 어떤 기능을 갖고, 어떤 상태를 갖는지를 먼저 다 정하고나면, 구현은 순식간에 진행된다.

- 도메인 주도 개발

4. 객체의 자율적인 책임이 중요하다.

- 자율적이다: 정의가 추상적이다. 내부로직은 자유롭다.
