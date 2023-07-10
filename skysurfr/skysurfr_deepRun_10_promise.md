# Promise

ECMAScript 표준 빌트인 오브젝트
[Callback Hell](http://callbackhell.com/) 탈출을 위해 느긋하게 Promise

## [Promise 생성자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise) : new Promise((resolve, reject) => { resolve(resultValue); reject(failureReason) })

> Promise 생성자는 주로 프로미스를 지원하지 않는 함수를 감쌀 때 사용합니다.

```JavaScript
const promise1 new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});
promise1.then((value) => {
  console.log(value)
});
console.log(promise1);
```

### Promise의 세가지 상태

- pending : 프로미스 생성 직후
- fulfilled : 비동기 수행 완료, 성공해서 resolve() 함수 호출
- rejected : 비동기 수행 완료, 실패해서 reject() 함수 호출

### doPromiseFunc().then(cb(fulfilledResult), cb(rejectedError))

- 첫번째 콜백은 성공처리 콜백함수
- 두번째 콜백은 에러처리 콜백함수
- 리턴은 Promise를 할 수도, 다른거 하면 암묵적으로 성공시 resolve(), 실패시 reject() 리턴

### doPromiseFunc().catch(cb(rejectedError))

then(undefined, onRejected)와 동일하게 작동

### doPromiseFunc().finally(cb)

성공 실패 여부와 상관없이 무조건 한번 호출된다

```JavaScript
let isLoading = true;

fetch(myRequest)
  .then((res) => {
    const contentType = res.headers.get("content-type");
    if(contentType && contentType.includes("application/json"")) {
      return res.json();
    }
    throw new TypeError("This is not JSON");
  })
  .then(json => { console.log(JSON.stringify(json)) }) // 성공시
  .catch(err => { console.log(err) }) // 에러시
  .finally(() => { isLoading = false; }); // 무조건 한번 실행
```

언제나 Promise를 리턴하므로 프로미스 체이닝 Promise Chaining 형식으로 연속적으로 사용할 수 있다

`fn().then().then().then().catch()`

async/await 가 더 최신이고 좋은데, 프로미스 기반이므로 프로미스 알아둘 필요있다

### Promise.resolve(value) / Promise.reject(error)

```JavaScript
const resolvedPromise = Promise.resolve([1,2,3]);
resolvedPromise.then(res => console.log(res));
console.log(resolvedPromise);
```

```JavaScript
const rejectedPromise = Promise.reject(new Error("Error"));
rejectedPromise.catch(err => console.error(err));
console.log(rejectedPromise);
```

### Promise.all\(\[reqIterable1(),reqIterable2(),reqIterable3()\]\)

이터러블로 된 여러 인자를 병렬로 처리한다

결과는 가장 늦게 끝나는거 기준으로 받아 처리한다

```JavaScript
const prm1 = Promise.resolve(3);
const prm2 = 42;
const prm3 = new Promise((res, rej) => {
  setTimeout(() =>res("foo"), 100)
});
Promise.all([prm1,prm2,prm3]).then((val) => {
  console.log(val);
});
```

에러나면 젤 먼저 나는거 캐치해서 리턴

```JavaScript
const prm1 = Promise.resolve(3);
const prm2 = 42;
const prm3 = new Promise((res, rej) => {
  setTimeout(() => res("foo"), 100)
});
const prm4 = new Promise((res, rej) => {
  setTimeout(() => rej("error"), 2000)
});
const prm5 = new Promise((res, rej) => {
  setTimeout(() => rej("error2"), 1000)
});
Promise.all([prm1,prm2,prm3,prm4,prm5])
  .then((val) => {
    console.log(val);
  })
  .catch((err) => {
    console.error(err);
  });
```

### Promise.race\(\[reqIterable1(),reqIterable2(),reqIterable3()\]\)

레이스해서 1등한 아이만 리턴한다
에러나면 all과 동일

```JavaScript
const prm1 = Promise.resolve(3);
const prm2 = 42;
const prm3 = new Promise((res, rej) => {
  setTimeout(() =>res("foo"), 100)
});
Promise.race([prm1,prm2,prm3]).then((val) => {
  console.log(val);
});
```

### Promise.allSettled\(\[reqIterable1(),reqIterable2(),reqIterable3()\]\)

all하고 같은데 처리가 모두 끝나면 모든 결과를 배열로 반영

```JavaScript
const prm1 = Promise.resolve(3);
const prm2 = 42;
const prm3 = new Promise((res, rej) => {
  setTimeout(() => res("foo"), 100)
});
const prm4 = new Promise((res, rej) => {
  setTimeout(() => rej("error"), 2000)
});
const prm5 = new Promise((res, rej) => {
  setTimeout(() => rej("error2"), 1000)
});
Promise.allSettled([prm1,prm2,prm3,prm4,prm5])
  .then((val) => {
    console.log(val);
  })
```

## MicroTask Queue / Job Queue

- 마이크로태스크 큐는 태스크 큐와 별도의 큐
- 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다
- Promise의 후속 처리 메서드의 콜백 함수가 여기에 일시 저장
- 그외의 비동기 함수의 콜백 함수나 이벤트 함수는 태스크 큐에 저장

## fetch(url [, options])

- HTTP 요청 전송 기능을 제공하는
- 클라이언트 사이드 Web API
- XMLHttpRequest 보다 사용법이 간단하고
- Promise 지원
  - 네트워크에러나 CORS 에러 이외에는 에러 reject를 안하니 response.ok 를 명시적으로 확인해 에러 리턴하는 것이 좋다
  - [Axios](https://axios-http.com/docs/example) 는 http 에러 reject 한다

```JavaScript
const url = "https://jsonplaceholder.typicode.com/posts/1";

fetch(url)
  .then(response => {
    if (!response.ok) {
      throw new Error(response.statusText);
    }
    return response.json();
  })
  .then(json => {console.log("JSON :", json)})
  .catch(error => {console.error("Error :", error)});
```

```JavaScript
const url = "https://jsonplaceholder.typicode.com/"

const data = fetch(url, {
  method: "POST",
  headers: {"content-type": "application/json"},
  body: JSON.stringify({
    id: 1,
    title: "JavaScript",
    completed: true
  }),
})
  .then(response => console.log(response);
```

[Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) + async/await 조합이 더 좋음
