# 46 Generator and async/await function

## 제너레이터 함수

`funtion*`

함수 코드블록의 실행을 여러개로 쪼갤수 있는 함수.

- 소분 되어있는 함수

### 쓰임새

- 이터러블 생성

- 비동기처리

- redux-saga에서 제너레이터 문법을 사용

### 형태 소개

기본적으로 함수 선언자 뒤에 `*` 을 붙이고, return 대신 `yeild`

```js
function* mygenerator() {
  //1
  yield 1;
  //2
  yield 2;
  //3
  yield 1;
}
const generator = mygenerator();
console.log(generator); // [object Generator]
```

함수의 출력값 : jenerator 객체

```s
mygenerator {<suspended>}
[[GeneratorLocation]]:  VM 50:1
[[Prototype]]: Generator
[[GeneratorState]]: "suspended"
[[GeneratorFunction]]: ƒ* mygenerator()
[[GeneratorReceiver]]: Window
[[Scopes]]: Scopes[3]
```

함수 내부 코드블록을 실행하려면
제너레이터 오브젝트의 `next()` 메서드를 호출.

반환값은 `value, done` 값을 담은 객체이다.

`value`: yeild에 전달된 값.
`done`: 제너레이터의 내부 코드 완료 여부를 알려주는 값.

```
generator.next()
{value: 1, done: false}
generator.next()
{value: 2, done: false}
generator.next()
{value: 1, done: false}
generator.next()
{value: undefined, done: true}
console.log(generator)
proxyConsoleLog.js:12 mygenerator {<closed>}
```

제너레이터는 이터러블이므로, for of 문을 사용하면

`next().value`를 반복 호출하는것과 같은 결과를 얻는다.

- 마지막값은 `done: false` 때문에 무시되므로 `return`을 배제하고 `yeild`로 끝낼것

### generator 중첩 함수

generator 내부에 generator 함수를 호출하는 로직을 짜면,
함수의 중간값을 저장하는 변수를 따로 선언할 필요가 없다.

### generator 내부로 값 저장하기

yeild는 `return` 과 비슷한 역할을 하지만,
값을 내부로 전달 할 수 도 있음

```js
function* myGenerator() {

  const data = yeild "hello";
 }

const generatorObj = myGenerator();

generatorObj.next("kimchi love");

```

출처: https://ko.javascript.info/generators

## async /await

### async

함수를 비동기 함수로 만들어준다.

함수의 파라미터 입력부 `()` 앞에 쓰인다.

```js
function async() {}
const myFunction = function async() {};
const arrowFunction = async () => {};
```

<br>

반환값을 `promise`로 바꾸어준다.

```js
const a = async () => {
  return "return is string";
};
```

`string`을 반환하는 위의 함수를 호출해보자

```s
a();

출력값
Promise {<fulfilled>: 'return is string'}
[[Prototype]]: Promise
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: "return is string"

```

프로미스이므로, `then()` 메서드를 사용해 값을 꺼낼 수 있다

```js
a().then((item) => console.log(item));

// print: return is string
```

### await

`async` 함수 내부에서 사용되는 키워드이다.

`await`의 오른쪽 작업이 완료될때까지, 코드의 흐름이 멈춘다.

```js
async () => {
  const response = await getResponseFromOuterSpace();
  // await 우측 함수 실행이 완료될때까지, 실행이 지연된다.
  const { something, interesting } = response;
};
```

=> 비동기코드의 진행을 동기적으로 바꾸어준다

`async` 함수 내부에서만 사용되므로

전역에선 사용할 수 없다.

