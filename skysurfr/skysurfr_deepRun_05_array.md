# 배열 Array

## 정의

[Array Objects - ECMA](https://tc39.es/ecma262/multipage/indexed-collections.html#sec-array-objects)
>Arrays are **exotic objects** that give special treatment to a **certain class of property names**. See 10.4.2 for a definition of this special treatment.

배열은 사실 이상한 오브젝트다, 특별한 종류의 키(0 이상의 정수를 가진 Index)를 가진.

[Array - MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)
배열은 리스트와 비슷한 객체로서 **순회**와 **변형** 작업을 수행하는 메서드를 갖습니다. JavaScript 배열은 **길이도, 각 요소의 자료형도 고정되어 있지 않습니다**.

배열은 유연한 리스트를 위한 오브젝트로, 순회와 변형이 가능하다.

## 배열이 가지고 있는 것

- Index : 0 부터 시작하는 일종의 키Key, 각 요소Value 에 자동으로 **순서**대로 붙는다
- length : 총 **길이(갯수) 값**을 알 수 있다
- 순차적 : 순회하며 처리하는 반복문(for문 등) 특화(= 사용시*역순도 가능*)
- `[ , ]` : 대괄호와 쉼표로 구분

### 일반적인 배열 : 밀집배열 dense array

메모리 공간에 빈틈없이 동일한 크기로 연속적으로 나열

- 인덱스(보통 0번 부터, 번호만 사용)로 임의의 요소에 효율적으로 빠르게 접근 Random Access 가능 O(1)
- 정렬되지 않은 배열은 시간이 더 걸릴 수 있다 O(n)
- 요소의 삽입 삭제시 연속 유지를 위해 다른 배열 요소를 이동시켜야 한다

### 자바스크립트 배열 : 희소 배열 sparse array 의 특성을 가짐

연속적으로 있지도 않고 동일한 메모리도 가지지 않은 분산된 배열

- 오브젝트 : 자바스크립트에서 배열이란 타입은 없다, 배열은 오브젝트이다
- 자바스트립트의 배열은 일반적인 배열을 흉내낸 특수 오브젝트 (해시 테이블로 구현한 오브젝트)
- 배열 요소의 접근은 일반적인 배열보다 느리지만
- 요소의 삽입 삭제는 일반적인 배열보다 빠르다

- 배열 번호 중간이 비어있는 empty 배열을 문법적으로 허용 `[ , 2, , 4].length === 2`
  - 가능하지만, 배열의 특성을 이용에 제약이 있어 안쓰는게 좋다
  - 순서를 건너뛴 인덱스, 없는 인덱스를 지정하면 요소를 동적으로 추가 가능
  - 오브젝트이기 때문에 배열의 특정 요소를 삭제할 수도 있다 `delete arr[1]` -> 희소배열 방지를 위해 `splice` 메서드 사용

## ES6의 새 기능 : 기존 JS의 배열의 약점을 보완, 개선

### Array.of()

- 기존 new Array로, 숫자로 인수 하나 받았을때 길이 10인 undefined 배열을 생성하고, 두개 이상이면 두개의 요소를 가진 배열을 받는 이상한 규칙에서 탈피
- 각 인자가 요소인(숫자여도 상관없이) 배열을 생성하는 메서드 `Array.of(5)`, `Array.of(1,6,3)`

### Array.from()

- 유사 배열 오브젝트 array-like object(길이값과 요소가 있는 오브젝트) 를 배열로 return `Array.from({ length: 2, 0: "a", 1: "b"})`
  - 유사 배열 오브젝트 : 인덱스가 있고, length 프로퍼티가 있는 오브젝트
- 이터러블 오브젝트 iterable object 를 배열로 return `Array.from("Hello")`
  - 순환 가능한 오브젝트 : `Set`, `Map`, `String`, DOM Object

## 배열의 메서드들

### 인덱스 찾기

- `Array.isArray(arr)` 배열 맞냐?
- `arr.indexOf("tomato")` 특정 배열요소를 찾아서 인덱스 return

#### 새로운 방법 (ES7) include

- `arr.include("banana", 2)` 첫번째 인자를 검색해서 true/false return하고, 두번째 인자로 검색 시작 위치 지정, NaN도 찾아낼 수 있다

### 배열의 마지막 요소 넣고 빼기 : push/pop

- 원본을 변경한다
- 넣는 것은 length를 리턴, 빼는 건 빼는 값을 리턴
- push와 pop이면 스택(Stack, 접시 쌓기, LIFO) 구현

### 배열의 첫번째 요소 넣고 빼기 : unshift/shift

- push, pop의 앞쪽 버전
- push와 shift이면 큐(Queue, 선입선출, FIFO) 구현

### 배열 합치기 : concat

- 인자값으로 받은 값이나 배열을 합쳐서 새 배열을 리턴한다
- 원본을 변경하지 않음
- push/unshift 를 대체할 수 있다

#### 새로운 방법 (ES6) ... 스프레드 문법

- 문법으로 이터러블(순환가능한) 배열 요소들을 전달한다 `[...arr, 3]`, `[...[1, 2], ...[3, 4]]`
- 일관적이고 직관적이다
- 원본을 복사하여 사용하여 원본을 훼손하지 않는다

### 배열 중간에 요소 제거, 추가 : splice

- `splice(start, deleteCount, items)`
  - start : 시작 인덱스
  - delete : 자를 갯수, 0(삽입시)이면 자르지 않고 item 요소를 삽입한다
  - item : (옵션) 자른 곳에 아이템을 삽입한다
- 원본을 직접 변경, 훼손한다
- filter가 이것과 유사하지만, 중복요소 모두 제거한다

#### 초초최신 ES14(ECMA2023) : toSpliced

- `toSpliced()`
- splice와 인자 역할은 동일
- 원본을 훼손하지않고 변경 값만 return

### 특정 범위를 복사, 반환 : slice

- `slice(start, end)`
  - start : 시작 인덱스
  - end : 끝 인덱스
- 원본을 훼손하지 않고 인수로 지정한 범위를 return
- 인수를 안쓰면 얕은 복사 shallow copy를 한다

### 배열을 텍스트로 붙여버린다 : join

- `arr.join(", ")`
- 인수로 받은 문자로 배열을 붙여서 텍스트로 Return
- 기본값은 `,`

### 배열을 뒤집는다 : reverse

- `arr.reverse()`
- 원본 순서를 뒤집고(훼손하고) 그 배열을 Return

#### 초초최신 ES14(ECMA2023) : toReversed

- `arr.toReversed()`
- 원본을 변경하지 않고 순서를 뒤집은 배열을 Return

### ES10(ECMA2019) : flat

- `[1,[,2,[3,[4]]]].flat(2)`
- 배열안의 배열을 인수 숫자 만큼의 단계씩 합친다
- 인수가 Infinity 이면 중첩 배열 모두를 합친다

#### ES10(ECMA2019) : flatMap = flat + map

- flat 과 map을 동시에
- flat은 1단계 평탄화만 한다

``` /* JavaScript */
let arr1 = ["it's Sunny in", "", "California"];

arr1.map(x=>x.split(" "));
// [["it's","Sunny","in"],[""],["California"]]

arr1.flatMap(x => x.split(" "));
// ["it's","Sunny","in","","California"]
```

## 배열 고차 함수 High-Order Function

- 함수를 인수로 전달 받거나, 함수를 반환하는 함수
- 함수를 인수로 쓸때는 왠만하면 화살표 함수로 써라 (this 바인딩을 안하니 `.` 앞에꺼가 this 됨)
- 가변 데이터를 피하고 불변성 지향의 함수형 프로그래밍 기반
  - 함수형 프로그래밍은 조건문, 반복문을 제거하고 변수사용을 억제하여 상태변경을 피하려는 프로그래밍 패러다임
    - 순수 함수를 통해 부수 효과 억제 -> 오류를 피하고 안정성을 높인다

### 요소들을 정렬 : sort()

- 기본 오름차순
- 배열로 받는거 가능
- 유니코드도 정렬 가능
- 숫자는 유니코드 문자로 변경했다가 변환해서 `[10,2].sort()` 하면 제대로 안나온다. 그래서 다음의 방법을 사용
- 인수 안에 함수 넣어서 정렬 가능
  - 반환값이 0보다 작으면 첫번째 인수를 앞으로, 0보다 크면 두번째 인수를 앞으로, (0은 브라우저 엔진 맘대로)
    - `[5,6,7,1,4,23].sort((a,b)=>a-b)` 오름차순
    - `[5,6,7,1,4,23].sort((a,b)=>b-a)` 내림차순
- 기존 배열을 변경한다 -> 함수형과 안맞는 요소 그래서 등장한게

#### 초초최신 초초최신 ES14(ECMA2023) : toSorted

- sort() 와 하는 일은 동일
- 원본을 훼손하지 않고 변경값을 return

### for문 대신 써라 : forEach

- 인자로 콜백함수를 받아 반복 호출후 안에서 처리한다
- 인자 순서는 value, key, 배열 자신(this)
- `[1,2,3].forEach((item, index, arr)=> { console.log(`${item}, ${index}, ${arr}`)})`
- return은 undefined
- 원본을 바꾸지는 않지만 arr[index] 해서 값 넣으면 변경가능

### 콜백 함수의 반환값으로 새로운 배열을 만들기 : map

- forEach와 하는건 같은데 콜백의 리턴값으로 배열을 매핑한다
- 아이템을 순환하면서 콜백의 처리를 받아 그대로 배열로 만든다
- 원본 length와 리턴한 배열의 length 프로퍼티가 같다 (1:1 매핑)

### 콜백 함수의 true return에 대한 것만으로 배열 만들기 : filter

- map과 하는건 같은데 리턴할때 true 인 것의 item으로 필터링하여 배열을 만든다
- 원본 length >= 리턴한 배열의 length
- 조건으로 걸러내기 때문에 중복된거는 모두 제거
  - 특정 하나만 하려면 indexOf랑 splice (toSpliced) 섞어 쓰면 됨

### 반복해서 하나의 결과값으로 합쳐버리기 : reduce

- `[1,2,3,4].reduce((accumulator, currentValue, index, array) => accumulator + currentValue)`
  - accumulator :초기값 0으로 시작해서 콜백함수를 돌리고, 리턴값을 다시 그자리에 놓는다 (끝날때까지 누적)
  - currentValue : 현 배열 값
  - index : 배열의 위치
  - array : this
- 총 합, 평균, 최대값, 최소값 등 하나의 값이 return
- 평탄화도 되는데 그건 flat 쓰면 됨
- 중복 없는 Set 쓰면 중복제거 됨
- 빈배열로 호출하면 에러남, 초기값이 없어서

### 특정 요소가 존재하는지 확인 : some

- `[1, 2, 10, 15].some(item => item > 10)`
- 순환하면서 콜백의 리턴이 한번이라도 참이면 true, 모두 거짓이면 false return

### 모두 조건에 해당하는지 확인 : every

- `[1, 2, 10, 15].some(item => item > 0)`
- 순환하면서 콜백의 리턴이 모두 참이면 true, 한번이라도 거짓이면 false return

### 인수 중에 콜백함수가 true인 첫번째 값을 리턴 : find

- `[1, 2, 3, 4].find(item => item === 2 )`
- 첫번째 true item을 return
- 하나도 없으면 undefined

### 콜백이 true인 배열 위치 인덱스 전달 : findIndex

- 존재하지 않으면 -1 return
- `[1,2,5,4,52,6].findIndex(item => index === 2)`

#### 새삥 ES14(ECMA2023) : findLastIndex

- 역순으로 검색