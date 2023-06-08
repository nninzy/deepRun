# 클로저 closure

- 함수를 일급 오브젝트 취급(너는 특급대우 받는 First Class 오브젝트야)하는 함수형 프로그래밍 언어에서는 사용하는 개념
- 함수형 프로그래밍 언어(함수가 처음부터 끝까지인 언어들 - 명령보다는 함수를 사용하고, 수학의 함수처럼 x입력이 있으면 y출력이 있고... 가변데이터 노노)

## 클로저란?

>> "A closure is the combination of a function and the lexical environment within which that function was declared."
>> 클로저는 조합이다. 함수와 선언적 환경의, 함수가 선언한 곳에서
>> ECMA Script

-> 함수가 호출해서 처리까지 다 끝난 거 같은데 문맥적으로 합당한 어딘가 누군가가(선언한 곳이 범위라서) 부르는 상황이라면?

```/* JavaScript*/
const x = 1;

function outer() {
  const x = 10;
  const inner = function() { console.log(x); }; // inner라는 변수에 함수 하나를 담는다, 안에 상위 변수 인자도 담아 써주고...
  return inner; // 그 inner()를 출력한다
}

const innerFunc = outer(); // outer 뒤에 () 가 붙었으니까 여기서 실행되고 주기가 끝났고(메모리에서 지워진 거 같고...), 값을 리턴하는데... 어라 리턴하는게 닫혀있는 함수의 인자를 가진 inner 함수??
innerFunc(); // inner 함수 is free
```

**여기서 중요한 건 변수 선언(정의) 한 곳이다** : lexical environment

궁극적으로 모든 함수는 상위 스코프를 기억하니까 궁극적으로 클로저라고 할 수 있지만, 모든 함수가 클로저는 아니다. 엔진이 문맥보고 아니다 싶은 건 안쓴다

### 클로저의 조건

1. 중첩함수
2. 상위 스코프의 식별자를 참조
3. 중첨 함수가 외부 함수보다 더 오래 살아남는 경우

여기서 상위 스코프의 식별자를 자유 변수 free variables 라고 부른다(누가 밖에서 바로 못건드는 자유 상태)
함수가 자유 변수에 대해 닫혀있다? -> 자유 변수에 의해 묶여있는 함수

```/* JavaScript */
// 클로저 메인
function makeCounter(aux) {
  // 자유 함수
  let counter = 0;

  // 자유 함수를 받아 외부 인자를 이용해 리턴
  return function() {
    counter = aux(counter);
    return counter;
  }
}

//보조함수
function increase(n) {
  return ++n;
}
function decrease(n) {
  return --n;
}

// 변수에 담았을때 독립적으로 실행한다
const increaser = makeCounter(increase);
const decreaser = makeCounter(decrease);

console.log(increaser());
console.log(increaser());
// 다른 변수에 담았을때 원래의 함수에서 자유 변수가 바뀌지 않고(오염되지 않고 따로 만들 수 있다) 실행
console.log(decreaser());
console.log(decreaser());
```

## 캡슐화 encapsulation

오브젝트 내부의 프로퍼티를 오브젝트랑 밀착시켜서 못건들게 하는거

JS는 class의 private 필드로 캡슐화(ES2019)

[MDN: Private class fields](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes/Private_class_fields)
