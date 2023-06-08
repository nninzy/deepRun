# 26장. ES6 함수의 추가 기능

### 함수의 구분

- ES6 이전의 JS 함수 = 다양한 목적으로 사용
(일반적인 함수로서 호출, 생성자 함수로서 호출, 메서드로서 호출 등 사용 목적에 따라 구분 X)
⇒ callable이면서도 constructor
    - 편리해보이나 실수를 유발시킬 수 있으며, 성능 면에서도 손해임
        - 객체에 바인딩된 함수를 생성자로 호출하는 경우가 문법상 가능함
        - 콜백 함수 등 생성자 함수로 호출되지 않아도 불필요한 프로토타입 객체 생성
- ES6 이후 함수를 목적에 따라 세가지 종류로 구분함
    1. 일반 함수 (**constructor**, prototype, arguments)
    = ES6 이전 함수와 차이 없음
    2. 메서드 (**non-constructor**, prototype X, super O, arguments O)
    3. 화살표 함수 (**non-constructor**, prototype X, super X, arguments X)
    
    async함수, 제너레이터 함수도 존재
    

### 메서드

- ES6 이전 = 명확한 정의 X. 객체에 바인딩된 함수를 일컫는 의미로 사용
    
    ES6 이후 = 명확한 정의 O. **메서드의 축약 표현으로 정의된 함수만을 의미**
    
- non-structor = 인스턴스 생성 불가능 (생성자 함수로서 호출 불가)
⇒ prototype 프로퍼티 존재 X. prototype 생성 X
    - 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor
- 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 가짐
(메서드가 아닌 함수는 super 키워드 사용 불가, 메서드 본연의 기능)
    
    ⇒ super 키워드 사용 가능
    

⇒ 익명 함수 표현식 할당하는 이전의 방식으로 메서드 정의 사용 안하는 것이 좋음

### 화살표 함수

- function 키워드 대신 화살표를 사용해 기존의 함수 정의 방식보다 간략하게 함수를 정의하는 것
- 내부 동작 역시 기존의 함수보다 간략함
- 유용한 곳 = 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제의 대안으로 적합
- non-constructor
⇒ prototype 프로퍼티 존재 X. prototype 생성 X
- 중복된 매개변수 이름 선언 불가 (일반 함수의 경우에는 중복 매개 변수 사용 시 에러 발생 X)
- 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않음
(스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조)