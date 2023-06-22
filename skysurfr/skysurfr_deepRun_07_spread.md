# Spread Syntax 스프레드 문법

[Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

> The spread (...) syntax allows an _iterable_, such as an array or string, to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected. In an object literal, the spread syntax **enumerates the properties of an object** and **adds the key-value pairs to the object being created**.

## iterable 대상들

- Array
- String
- Set
- Map
- DOM (NodeList, HTMLCollection)
- Arguments

## 사용처 : 쉼표로 구분하는 값의 목록

- 함수 호출시 인자들
  - 예: `Math.max(...arr)` 인자중에 최대값을 넣는거라 배열을 그냥 넣을 수 없음
- 배열 리터럴
  - 예1: 배열 안에 배열을 풀어서 추가할때 `arr = [...[1, 2], ...[3, 4]]`
  - 예2: 배열 옅은 복사 Swallow Copy `const origin = [1, 2]; const copy = [...origin];`
- 오브젝트 리터럴 프로퍼티
  - 예 : `const objClone = { ...obj }`

## 함수 인자의 Rest Parameters와 차이점

- 문법은 비슷해보이나
- Rest Parameters는 함수 만들때 쓰고, 인자들을 배열로 묶어줌 =/= 반대개념
  - 예 : `const sum = (...args) => args.reduce((pre,cur) => pre + cur, 0);console.log(sum(1,2,3,4,5));`
- Spread Syntax는 이터러블 요소들을 개별값 목록으로 나눈다
