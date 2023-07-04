## Set

배열의 특수버전:

값이 중복되지 않는 이터러블

<br>

### 메서드

```
new Set([iterable]) – set을 만든다. key: value 쌍으로 이루어진 객체는 value 값만 저장한다.

set.add(value) – value를 추가한다.

set.delete(value) – value를 삭제한다.

set.has(value) – value가 존재하는지 확인한다.

set.clear() – set을 비워준다.

set.size – 성분의 갯수를 반환한다.
```

<br>

### 이터러블 반환 메서드 및 프로퍼티

```
set.keys()  === set.values() – set의 값을 갖는 이터러블을 반환
set.entries() – [value, value] 쌍을 성분으로 갖는 이터러블을 반환
```

<br>

### 수학 집합과 관련된 메서드

기본적으로 존재하지 않는다.

<br>

## Map

객체의 특수버전:

key 타입에 제한이 없는 이터러블

```
myMap.set(true,true);   //Map(1) {true => true}


myMap.forEach((key,value)=> console.log(`key:${key} value:${value}`))

//key:true value:true
```

<br>

### 생성법

new 생성자를 통해 생성

### 생성자 함수의 인자

key, value를 포함하는 개별 배열

```
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);
```

<br>

### 메서드 및 프로퍼티

```
new Map() – Map 생성

map.set(key, value) – 데이터 저장

map.get(key) – key 와 묶인 데이터 호출

map.has(key) – 해당 key값을 갖는 데이터가 존재하는지 확인

map.delete(key) – key값에 해당하는 성분 삭제.

map.clear() – 성분 전부 삭제

map.size – 성분 갯수 출력
```

<br>

### 이터러블 반환 메서드

```
map.keys() – key 값들이 들어있는 이터러블 반환

map.values() – value 값들이 들어있는 이터러블 반환

map.entries() – [key, value] 값들이 들어있는 이터러블 반환
```

<br>

### 기존 객체(배열)과 비교 포인트

<br>

1. 성분 추가 방법
   key, value를 각각 set의 인자로 넣어서 성분을 추가

   - `add` 아니다.
   - `map.key = "value"` 아니다.

2. size를 통해 성분 갯수 확인

   - `length` 아니다.

3. get(key) 메서드를 통해 성분 획득

   - `map.key` 아니다.
   - `map[key]` 역시 아니다.

     <br>
