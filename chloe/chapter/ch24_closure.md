# 24장. closure

### closure

- 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성 중 하나
(자바스크립트 고유의 개념이 아님 ⇒ ECMAScript에는 별도의 설명 없이 MDN 참고 필요)

> 함수와 그 **함수가 선언된 렉시컬 환경**과의 조합 - MDN
> 
- 렉시컬 스코프 (정적스코프)
자바스크립트 엔진은 함수를 어디에 정의했는지에 따라 상위 스코프를 결정함
다시 말해, 상위 스코프는 함수 정의가 위치하는 스코프임
- 함수 객체의 내부 슬롯 [[Environment]]
함수가 자신이 정의된 환경, 상위 스코프의 참조를 저장하는 공간
현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킴
    - 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 상위 스코프임
- 클로저의 본질
= 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있음
- 자바스크립트의 모든 함수는 상위 스코프를 기억하기에 이론적으로 모든 함수는 클로저이나, 일반적으로 모든 함수를 클로저라고 하지는 않는다
    - 상위 스코프의 어떤 식별자로 참조하지 않을 경우 클로저라고 하지 않음
    → 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않기 때문
    → 클로저의 불필요한 메모리 점유를 걱정할 필요 없음
    - 상위 스코프의 식별자를 참조하고 있더라도, 외부 함수보다 중첩 함수의 생명주기가 짧을 경우 (외부 함수 외부로 중첩함수가 반환 X) 클로저 본질에 부합하지 않기에 클로저라고 하지 않음
- 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고, 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적

<aside>
💡 클로저

- 상위 스코프의 식별자를 참조하고 있고, 외부 함수보다 중첩 함수의 생명주기가 긴 경우
- 자유 변수(클로저에 의해 참조되는 상위 스코프 함수)에 묶여 있는 함수
</aside>

### closure의 활용

- 클로저를 사용하는 이유
    
    = 상태를 안전하게 변경하고 유지하기 위함
    (상태가 의도치 않게 변경되지 않도록 상태를 은닉 & 특정 함수에게만 상태 변경 허용)
    
- 클로저의 활용
    - 함수형 프로그래밍에서 부수 효과를 최대한 억제해 오류를 피하고 프로그램의 안정성을 높이고 싶을 때

### 캡슐화와 정보 은닉

- 캡슐화 = 프로퍼티(객체 상태)와 메서드 (프로퍼티 참조 & 조작하는 동작)를 하나로 묶는 것
- 캡슐화를 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 함(정보 은닉)
    - 정보은닉의 효과
        1. 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지 → 정보 보호
        2. 객체 간의 상호 의존성 (결합도)을 낮춤

### 클로저를 사용할 때 자주 발생하는 실수

…? 이해 못함…

전반적으로 클로저에 대해 이해하지 못함…