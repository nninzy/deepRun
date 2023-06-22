# Set & Map

Set은 배열에 대응, Map은 오브젝트에 대응하면서 중복이 없게 만든 것

## Set

_중복되지 않은_ 유일한 **값**들의 집합 = 수학적 집합 (합집합, 교집합, 차집합, 여집합)

### 생성은 `new Set(iterableValues)`

- 중복되지 않는 값을 가진 이터러블 오브젝트 생성
- 중복되는 값은 _삭제_
- 값에서 NaN과 NaN은 같다고 평가, -0과 +0도 같다고 평가
- 중복되지 않는 값이 중요하기 때문에 인덱스가 없다
- 이터러블이다 = 추가된 순서대로 순회한다. 순서에 의미는 없지만 이터러블과의 호환성을 위해

### 메서드, 프로퍼티

- iterable.size 로 개수 확인 가능(값을 바꿀 수는 없고 get만 할수만 있다)
- iterable.add(value) 로 새로운 요소를 추가하고, 그 요소를 포함한 오브젝트를 리턴한다
  - 중복은 무시한다
  - 연속 점찍어 추가 가능하다
- iterable.has(value) 특정 값이 존재하는지 true/false 리턴
- iterable.delete(value) 특정 값을 지우고 성공 여부 true/false 리턴
  - 연속으로 점찍어 호출 불가(오브젝트를 리턴하는 것이 아니라서)
- iterable.clear() 모든 요소 삭제 undefined 리턴
- 인덱스가 없기때문에 forEach의 콜백 첫번째 두번째 인자인 currentValue, index 모두 같은 값이다
  - `const newSet = new Set([1, 2, 2, 3, 4, 5, 7, "a", "c", "c"]);newSet.forEach((currentValue, index, arr)=> { console.log(currentValue, index, arr) });`

## Map

*유일한 키와 값의 쌍pair*으로 이루어진 컬렉션, **오브젝트와 유사**하지만 다른 점이 있다

### 생성은 `new Map()`

- 이터러블 오브젝트를 생성
- 추가 순서를 기억한다
- 중복되는 키(이름)는 _덮어쓴다_
- 키에서 NaN과 NaN은 같다고 평가, -0과 +0도 같다고 평가
- 중복 배열 리터럴처럼 생겼다
- _모든 값을 키로 사용_ 가능(오브젝트는 문자열, 심볼만 가능)

### 메서드 프로퍼티

- iterable.size 로 개수 확인(값을 바꿀 수는 없고 get만 할수만 있다)
- iterable.set("key", "value") 로 키,값 쌍을 추가하고 추가를 포함한 오브젝트를 리턴한다
  - 중복은 덮어쓴다
  - 연속 점찍어 추가 가능하다
- iterable.get("key") 부르면 값을 리턴한다
- iterable.has("key") 특정 키가 있는지 확인하고 true/false 리턴
- iterable.delete("key") 특정 키, 값 쌍을 지우고 성공 여부 true/false 리턴
  - 연속으로 점찍어 호출 불가(오브젝트를 리턴하는 것이 아니라서)
- iterable.clear() 모든 요소 삭제 undefined 리턴
- 인덱스가 없기때문에 forEach의 콜백 첫번째는 value 두번째는 key 값이다
  - `const newMap = new Map();newMap.set("a", 10);newMap.set("b", 20);newMap.forEach((value, key, obj)=> { console.log(value, key, obj) });`

---

#### 여기서 이터레이터 한번 더 본다

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols)

이터러블 프로토콜 (오브젝트에 대한 것)

> _iterable protocol_ 은 JavaScript 객체들이, 예를 들어 for..of 구조에서 어떠한 **value 들이 loop 되는 것과 같은 iteration 동작을 정의하거나 사용자 정의하는 것을 허용**합니다. 다른 type 들(Object 와 같은)이 그렇지 않은 반면에, 어떤 built-in type 들은 Array 또는 Map 과 같은 default iteration 동작으로 built-in iterables 입니다.

이터레이터 프로토콜 (순환시키는 것들에 대한 것)

> iterator protocol 은 value( finite 또는 infinite) 들의 sequence 를 만드는 표준 방법을 정의합니다.
> 객체가 next() 메소드를 가지고 있고, 규칙에 따라 구현되었다면 그 객체는 iterator이다:

---

### 이터러블이면서 이터레이터인 오브젝트를 반환하는 메서드

- iterable.keys() 키를 값으로하는 이터러블이면서 이터레이터인 오브젝트를 리턴
- iterable.values() 값으로 구성된 이터러블이면서 이터레이터인 오브젝트를 리턴
- iterable.entries() 키와 값으로 구성된 이터러블이면서 이터레이터인 오브젝트를 리턴
