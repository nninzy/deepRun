# 새로운 JavaScript

## 화살표 함수Arrow Function

- 함수 표현식이다
- 줄일 건 다 줄일 수 있다 (직관적이고 편하다)

``` /* function */
// 소괄호 안에 매개변수(들)을 선언 할 수 있다
const arrow = (x, y) => { };

// 매개변수가 하나면 소괄호 생략 가능하다
const arrow2 = x => { };

// 매개변수가 없으면 소괄호 생략 못한다
const arrow3 = () => { };

// 안에 문장이 한줄이고 리턴을 한다면 중괄호 생략 가능하다 (암묵적인 리턴)
const arrow4 = x => x ** 2
// 이거는 이렇게도 쓸 수 있다. 중괄호 쓰면 암묵적 리턴 안한다
const arrow5 = x => { return x ** 2 }

// 오브젝트 그 자체를 리턴하면 소괄호로 감싸줘야 한다. 빼면 그냥 함수 몸체
const arrow6 = (id, content) => ({id, content});
```

### 일반함수와 화살표 함수의 차이

- new 못쓴다(non-constructor), 그래서 프로토타입도 없다
- 인자 argument가 없으면 상위 스코프 참조한다
- 매개변수를 중복해서 쓰면 에러난다(당연한거 같은데 function은 이게 된다는 이야기-strict mode 제외-)

``` /* function */
const arrow6 = (a, a) => { return a + a } // 에러난다
```

- 함수 자체의 this가(this binding) 없고, 가장 가까운 상위함수의 화살표 함수 아닌 함수의 this를 참조
  - 이걸 lexical this 라고 한다. 정의된 위치로 this가 결정되는거라
  - 상위에 뭐가 없으면 전역 오브젝트가 this가 된다
- 무조건 상위 스코프니까 프로퍼티에 넣고 쓰는 화살표함수 자제요

``` /* JavaScript */
const counter = {
  num: 1,
  increase: () => ++ this.num // this가 같은 레벨의 num이 아니라 그 위
};
console.log(counter.increase()); // NaN
```

## 오브젝트 메서드 축약 표현

``` /*JavaScript*/
//이거를
const ai = {
  name: "bard",
  sayHello: function() {
    console.log(`Hello, ${this.name}`);
  }
}

//이렇게 줄일 수 있다
const ai2 = {
  name: "chatGpt",
  sayHello() {
    console.log(`Hello, ${this.name}`);
  }
}

ai.sayHello();
ai2.sayHello();

```

## Rest 파라미터 Rest Parameter (가변 인자)

`...`으로 매개변수 argument로 전달받은 인수 전체를 배열로 전달받는다.
인자의 형식 신경 안쓰고 다 받는다는...

``` /* JavaScript */
const sum = (...args) => {
  // args는 배열 된다
  return args.reduce((pre, cure) => pre + cure, 0);
}

console.log(sum(1,2,3,4,5));
console.log(sum(1,2,3,4,5,6,7,8,9,10));
```

### Rest 파라미터 특성

- () 안에서 하나만 선언 가능
- 기본값 선언은 안된다
- 다른 인자랑 같이 써도 되고, 순서는 마지막이어야 한다, 순차적으로 인식한다

``` /* JavaScript */
const bar = (param1, param2, ...rest) => {
  console.log(param1);
  console.log(param2);
  console.log(rest);
}
bar(1,2,3,4,5,6,7); // 3이후부터 배열로 받는다
```

## 매개변수 기본값 선언

- 매개변수에 기본값 줄 수 있다 (TypeScript로 가는 길?)
- 매개변수가 없거나, undefined 일 경우에만 기본값 유효

``` /* JavaScript */
const sayTheName = (name = "홍길동") => {
  console.log(`Say the name ${name}`);
}

sayTheName();
sayTheName(undefined);
sayTheName(null);
sayTheName("John Doe");
```

- 매개변수가 없는 건 length 프로퍼티, arguments 오브젝트에 영향을 주지 않는다

``` /* JavaScript */
function sum(x, y = 0) {
  console.log(arguments); // 인자 출력
}
console.log(sum.length); // 길이는 1
sum(1); // 1밖에 안나옴
sum(1,2); // 1, 2 다 나옴
```
