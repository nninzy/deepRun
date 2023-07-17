# 46 Generator and async/await function

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
