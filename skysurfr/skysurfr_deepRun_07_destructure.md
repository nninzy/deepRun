# Destructuring Assignment 디스트럭처링 할당

[Destructuring Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

> The destructuring assignment syntax is a JavaScript expression that makes it possible to _unpack values_ from arrays, or properties from objects, into _distinct variables_.

구조 분해 할당이라고도 한다. 구조(이터러블, 오브젝트)를 destroy 해서 변수에 개별 매칭해서 할당하는 문법

```/* JavaScript */
const [value, setValue] = React.useState(); // React.useState는 item 2개 배열을 리턴
```

## 배열 디스트럭처링 할당

- 할당 기준은 인덱스이고 순서대로 할당한다
- 변수의 개수와 이터러블 개수가 안맞아도 상관없다
  - 예1 : `const [ a, b, c ] = [ 1, 2 ]; // a = 1, b = 2, c = undefined`
  - 예2 : `const [ a, b ] = [ 1, 2, 3 ]; // a = 1, b = 2`
- 변수에 기본값 설정할 수 있다
  - 예 : `const [ a = 1, b, c = 3 ] = [ 1, 2 ]; // a = 1, b = 2, c = 3`
- Rest Parameter 사용 가능하다
  - 예 : `const [x, ...y] = [1, 2, 3]; console.log(x, y);`

## 오브젝트 디스트럭처링 할당

프로퍼티 키로 변수에 할당

```/* JavaScript */
export const watch = (req, res) => {
  const { params : { id } } = req; // req는 프로퍼티 키가 있다
  console.log(id);
}
```

- 값을 할당하지 않으면 에러가 발생한다
- 여러 프로퍼티 중, 원하는 필요한 것만 할당할 수 있다
- 중첩도 사용할 수 있다
- 오브젝트의 키와 다른 변수명으로 받을때는 값을 설정하면 된다
  - 예 : `const userInfo = { firstName : "Kildong", lastName : "Hong" };const { firstName : fn, lastName : ln } = userInfo; // fn = "Kildong", ln = "Hong`
- 기본값을 설정할 수 있다
  - 예 : `const { age = 17, name } = { name: "Kim" };`
- [Rest Property](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#rest_property) 다른 Rest와 유사하게 오브젝트에서도 사용할 수 있다
  - 예 : `const { a, ...others } = { a: 1, b: 2, c: 3 }; // a = 1, b = 2, c = 3`
