# `this` : 자기 참조 변수 self-referencing variables

`this`는 오브젝트 안에서 `.` 붙여 쓰려고 많이 쓰는데...

## *나* 안에서 변수가 아니고 *나* 라는 오브젝트(또는 인스턴스)를 가르키는 역할(변수의 역할) 

- 메서드의 안에서 *자기 자신 오브젝트(와 그 오브젝트의 무언가)*를 가르키키 위한 수단
- ~~자기 자신을 그냥 이름으로 부를 수도 있지만 그건 뺑뺑이(재귀적 호출? recursion) 도는 게 아닐까?~~
- this는 자기자신 그 무엇이지만 JS에서는 **형태에따라 동적으로 결정**

> this 바인딩 (Binding 가르켜서 대상과 묶기)은 렉시컬(명명당시)이 아니라 함수 호출 시 결정된다

### function() 의 this는?

(function 이란 단어를 쓰는 경우에 해당)
여기서 this = **전역 오브젝트 global object**(웹에서는 window)
메서드 내부의 중첩함수도 전역 오브젝트 바인딩, 일반함수로 콜백할때도 마찬가지
-> 변수하고 달리 *호출될 시점*이 중요

this는 메서드를 위한 것 -> 결국 this가 그렇게 의미 없다 (이벤트는 좀 다른데...)
function은 데이터 프로퍼티니까 앞을 가르킬게 딱히 없기도 함
(strict mode에서는 undefined 바인딩)

### 메서드에서 this는?

오브젝트의 메서드란 결국 참조하는 것이라... 
메서드를 호출한 오브젝트(접근자 프로퍼티 - 본체가 없고 누구를 가르키는 것이니까)가 this는 `.`(dot) 앞의 오브젝트

### new 생성자 함수constructor 에서 this

- 빵틀을 가르켜봤자 소용 없고 앞으로의 붕어빵들... new 로 만들어질(앞으로...) 인스턴스를 가르킴

### 프로토타입의 apply/call/bind 메서드에서 this

- 프로토타입용으로, 호출하면서 받은 첫번째 argument의 함수(오브젝트를)를 this로 Binding 묶는다.

