# generator / Async / Await

## generator 함수란?

남의 코드를 보는데 곱하기도 아닌데 함수에 \* 가 붙어있다면?

generator 함수는 잠시 나갔다가 돌아올 수 있는 함수
(비동기가 가능할지도?)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function) (그 다음 항목)

`// console.log(gen.next().value);` 의 주석을 풀면?

```JavaScript
function* generator(i) {
  yield i;
  yield i + 10;
}
const gen = generator(10);
console.log(gen.next().value); // 출력: 10
// 딴짓 딴짓....
// console.log(gen.next().value); // 출력: ??
```

### generator 사용법

- 선언 : `function *` 함수와 이름 사이 `*` 넣기
  - 화살표 함수와 생성자 함수에서는 사용할 수 없다
- `yield` 제네레이터 함수 안에서 중지 또는 재개하는데 사용

### generator는 이터레이터 오브젝트

- 제네레이터 함수는 함수 코드 블록을 실행하는 것이 아니라 제네레이터 오브젝트를 생성, 리턴
- 제네레이터 오브젝트는 이터러블이면서 이터레이터이다

#### next() 는 다음 제네레이터 동작으로 넘김

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)

```JavaScript
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator(); // "generator { }"

console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
```

#### return() 는 제네레이터의 리턴역할

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator/return](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator/return)

```JavaScript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

const g = gen();

g.next();        // { value: 1, done: false }
g.return('foo'); // { value: "foo", done: true }
g.next();        // { value: undefined, done: true }
```

#### throw() 는 에러 처리

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw)

```JavaScript
function* gen() {
  while (true) {
    try {
      yield 42;
    } catch (e) {
      console.log('Error caught!');
    }
  }
}
const g = gen();
g.next();
// { value: 42, done: false }
g.throw(new Error('Something went wrong'));
// "Error caught!"
// { value: 42, done: false }
```

### 제너레이터 활용

#### 예제

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function) (그 다음 항목)

간단한 증가 제네레이터 함수

```JavaScript
function* idMaker(){
  var index = 0;
  while(index < 3)
    yield index++;
}

let gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // undefined
// ...
```

yield 다음에 함수 실행에도 제네레이터 붙일 수 있다
(누적 아니다)

```JavaScript
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i){
  yield i;
  yield* anotherGenerator(i);
  yield i + 10;
}

let gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

인자값 넘기기

```JavaScript
function* logGenerator() {
  console.log(yield);
  console.log(yield);
  console.log(yield);
}

let gen = logGenerator();

// the first call of next executes from the start of the function
// until the first yield statement
gen.next();
gen.next('pretzel'); // pretzel
gen.next('california'); // california
gen.next('mayonnaise'); // mayonnaise
```

---

## Async/Await

async/await : promise 비동기 처리를 동기처럼 구현
기다려주고 리턴값이 필요한 경우에(만) 이렇게 처리 -> 그럴 필요 없을땐 그냥 비동기 처리 하면 됨

async/await 함수는 항상 promise를 반환할 수 있기 때문에 아래와 같이 2중 await 처리도 가능

[https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/userController.js](https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/userController.js)

```JavaScript
export const finishKakaoLogin = async (req, res) => {
  //...

  const tokenRequest = await (
    await fetch(accessUrl, {
      method: "POST",
      headers: {
        "Content-type": "application/x-www-form-urlencoded;charset=utf-8"
      }
    })
  ).json();

  //...
}
```

### await 없는 [async function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)은?

> async 함수의 본문은 0개 이상의 await 문으로 분할된 것으로 생각할 수 있습니다. 첫번째 await 문을 포함하는 최상위 코드는 동기적으로 실행됩니다. 따라서 await 문이 없는 async 함수는 동기적으로 실행됩니다. 하지만 await 문이 있다면 async 함수는 항상 비동기적으로 완료됩니다.

=> 제네레이터 함수의 yield 와 비슷?

이 함수는

```JavaScript
const foo = async () => {
  return 1
}
```

요것과 동일

```JavaScript
const foo = () => {
    return Promise.resolve(1)
}
```

그리고

이 함수는

```JavaScript
const foo = async () => {
  await 1
}
```

이것과 동일

```JavaScript
const foo = () => {
    return Promise.resolve(1).then(() => undefined)
}
```

### 에러는 try/catch 문으로 처리

[https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/videoController.js](https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/videoController.js)

```JavaScript
export const removeComment = async (req, res) => {
  //...
  try {
    relatedVideo.comments = relatedVideo.comments.filter(item => String(comment._id) !== String(item));
    commentUser.comments = commentUser.comments.filter(item => String(comment._id) !== String(item));
    await Comment.findByIdAndDelete(id);
    await relatedVideo.save();
    await commentUser.save();
    return res.sendStatus(201);
  } catch (error) {
    console.log(error);
    return res.sendStatus(400);
  }
//...
}
```
