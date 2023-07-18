# class : 새로운 오브젝트 생성 매커니즘

## class 특징 (vs 기존 프로토타입 방식)

- new 없이 호출 불가
- 상속 지원 키워드 extends, super
- 암묵적 strict mode
- 샐수 없다 enumerable : false (for ... in, Object.keys 사용 불가)
- 클래스는 반드시 정의한 뒤에 사용한다

## class는 일급 first-class 오브젝트 == 함수다

- 무명 리터럴 생성 가능(런타임으로 생성 가능)
- 변수, 오브젝트, 배열에 저장 가능
- 함수의 매개변수에게 전달 가능
- 함수의 반환값으로 사용 가능

## class의 세가지 메서드

### constructor

특수한 메서드로 고유한 이름에 클래스 당 단 한개만 존재 가능
생략하면 빈 constructor가 암묵적으로 생성
리턴이 없으면 암묵적으로 this 리턴 (return 문은 안쓰는게 좋다)

```JavaScript
class Rectangle {
  // 인스턴스 생성, 초기화 특수 메서드
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  get area() {
    return this.calcArea();
  }

  // 암것도 안쓰면 프로토타입 메서드가 된다
  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10); // 인스턴스 생성

console.log(square.area);
```

### 프로토타입 메서드

클래스 내부에서 그냥 만든 메서드()는 프로토타입 메서드가 된다

### 정적 메서드

class 내에서 static 이라고 써주고 정의한 메서드
안에서만 쓰라고 만들어준건가

```JavaScript
class Person {
  constructor(name) {
    this.name = name;
  }
  static sayHi() {
    console.log("hi");
  }
}

Person.sayHi(); // 인스턴스를 생성하지 않아도 호출 가능

const me = new Person("Bruce");
me.sayHi(); // 인스턴스를 생성하고 호출은 안된다
```

### 정적 메서드 vs 프로토타입 메서드

프로토타입 체인에서 정적 메서드는 없다

정적 메서드 : 클래스 호출 vs. 프로토타입 메서드 : 인스턴스 호출
정적 메서드 : 인스턴스 프로퍼티 참조 X vs. 프로토타입 메서드 : 인스턴스 프로퍼티 참조 O

this를 사용하지 않으면 정적 메서드가 좋다

오브젝트 본체에서 바로 쓰는 그런게 정적 메서드
Math.max() 같은게 빌트인 정적 메서드

## 프로퍼티

### 인스턴스 프로퍼티

인스턴스용 프로퍼티는 constructor 내부에서 정의한다
외뷔 접근 제한 없이 public 하다

```JavaScript
class Person {
  constructor (name) {
    this.name = name;
  }
}
```

#### 인스턴스 프라이빗 필드 프로퍼티

이름 앞에 \#붙이면 된다
constructor 앞에 미리 선언해야함

```JavaScript
class Person {
  #name

  constructor() {
    this.#name = "Kim"
  }
}
```

### 접근자 프로퍼티

- 전에 했던 그... 접근자 프로퍼티
- 프로토타입 메서드이고,
- 다른 데이터 프로퍼티의 값을 읽거나 getter /
- 값을 저장하는 setter 로 구성

get(getter)은 반드시 무언가를 return 해야 한다
set(setter)은 반드시 매개변수 인자(하나)가 있어야 한다

```JavaScript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const me = new Person("Bruce", "Lee");

console.log(me.fullName); // getter 호출

me.fullName = "Lionel Messi"; // setter 호출

console.log(me.fullName); // getter 호출
```

### 퍼블릭 클래스 필드 Public Class Fields

클래스가 생성하는 인스턴스의 프로퍼티

```JavaScript
class Person {
  name = "Lee";
}

const me = new Person();
console.log(me);
```

### 프라이빗 클래스 필드 Private Class Fields

이름 앞에 \# 붙이고 쓰는거

- 클래스 내부에서만 쓰이고
- 자식 클래스 내부 X, 인스턴스 접근 X

### 스태틱 클래스 필드 Static Class Fields

스태틱 메서드와 같이 static을 앞에 붙인다

- 클래스에서 사용 가능하고
- 인스턴스로는 사용 불가

Math.PI 같은거

## 상속에 의한 클래스 확장

클래스를 상속받아 확장 extends 하여 정의
기존것을 그대로 사용하고 추가로 할것만 더해준다

여기서 상위클래스를 슈퍼클래스라고 하고 확장된 클래스를 서브클래스라고도 한다

```JavaScript
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return "eat" }
  move() { return "move }
}

class Bird extends Animal {
  fly() {return "fly" }
}

class HomoSapience extends Animal {
  // ?
}
```

### super 키워드

super를 호출하면 슈퍼클래스의 constructor를 호출한다

```JavaScript
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class Subclass extends Base {
  // 아무것도 안하면 암묵적으로 아래의 constructor가 정의
  // constructor(...args) { super(...args) }
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}
```

메서드도 가능

```JavaScript
class Base {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `Hi, ${this.name}`
  }
}

class Drink extends Base {
  sayHi() {
    return `${super.sayHi()}, Would like something to drink?.`
  }
}

const sub = new Drink("Lee");
console.log(sub.sayHi());
```

```JavaScript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

class ColorRange extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  toString() {
    return super.toString() + `, color = ${this.color}`
  }
}

const newShape = new ColorRange(2, 4, "red");
console.log(newShape);
console.log(newShape.getArea());
console.log(newShape.toString());
```

표준 빌트인 오브젝트도 확장할 수 있다
