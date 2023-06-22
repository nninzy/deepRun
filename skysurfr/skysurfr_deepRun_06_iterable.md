# 이터러블 Iterable

## ES6 이터레이션 프로토콜 Iteration Protocol

순회 가능한 데이터 규약 Protocol을 정함

### Iterable Protocol, Iterator Protocol

- `Symbol.iterator`(Well-know symbol, JS 기본 제공) 메서드를 호출하면
- 이터레이터 오브젝트(일종의 탐색 포인터)를 반환하는 프로토콜
- 이터러블 프로토콜 대상
  - `for... of` 순환
  - `...` 스프레드 문법 적용
  - 배열 디스크럭처링 할당 array destructuring assignment
    - `const [a ,b] = [1, 2]` 이런거

### 빌트인 이터러블 Built-in Iterable

- Array
- String
- `new Map(iterable)`
  - **키 중복없는** 이터러블 오브젝트 생성
  - 길이는 `map.size` 로 확인
- `new Set(iterable)`
  - **중복되지 않는 유일값**을 가지는 수학적 집합 생성
  - 중복 없고, 순서에 의미 없고, 인덱스 접근이 불가
  - 길이는 `set.size` 로 확인
- [Typed Array](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/wqGlobal_Objects/TypedArray) : 특정 요소의 타입이 있는 배열 (ex: Uint8Array)
- arguments
- DOM Collection

### 유사 배열 오브젝트 Array-like Object 를 배열로 변환

``` /* JavaScript */
const arrLikeObj = {
  0: 1,
  1: 2,
  2: 3,
  length: 3
}

const arr = Array.from(arrLikeObj);
console.log(arr);
```
