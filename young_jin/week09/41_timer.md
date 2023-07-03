# 41 Timer

<br>

## 호출 스케쥴링

<br>

- 함수를 바로 호출하지 않고, 일정 시간 이후에 실행되도록 `예약` 하는것

- setTimeout, clearTimeout, setInterval and clearInterval 은 global function

  - 전역객체의 메서드. host객체이다.

<br>

## setTimeout / clearTimeOut

<br>

### setTimeout

<br>

함수의 실행에 딜레이를 걸어주는 함수.

- 1번인자로 받은 함수를, 2번인자에 입력된 시간이 지난후 실행해준다.

  - 2번인자의 단위는 ms (1/1000 초)이다. 실제 호출까지 딜레이는 더 길수도 있다.

- 3번 인자부터는 1번 인자의 argument가 된다.

```
setTimeout(
        p1: callback,
        p2: time to delay (ms),
        ...argumentsForCallback
    ): timeoutID
```

반환값인 `timeoutID` 는 양의 정수로, `setTimeout`의 타이머를 식별하는데에 사용된다.

    - Node.js 환경에선 객체이다.

<br>

### clearTimeout

<br>

setTimeout의 타이머, 즉 호출 스케쥴링을 취소하는 함수

`setTimeout`의 반환값을 인자로 입력받는다.

<br>

## setInterval / clearInterval

<br>

### setInterval

<br>

함수를 일정 간격(딜레이)마다 반복 실행시켜주는 함수

인자 및 반환값은 `setTimeout`과 같다.

<br>

### clearInterval

<br>

setInterval의 타이머, 다시말해 호출 스케쥴링을 취소하는 함수

`setInterval`의 반환값을 인자로 입력받는다.

<br>

### notice

<br>

setTimeout과 setInterval의 반환값 pool 집합은 동일하다.

clearInterval과 clearTImeout은 모두 두 함수의 timer를 취소시킬 수 있다.

```
일관성을위해 이름의 뒷부분이 겹치는 함수끼리 함꼐 사용하자.

setTimeout + clearTimeout

setInterval + clearInterval
```

<br>

## Debounce 와 throttle

<br>

Underscore, Lodash 같은 패키지에 정의된 메서드

`scroll`, `resize`, `input`, `mousemove` 등과 같이

짧은 시간안에 연속적으로 발생하는 이벤트에의한

웹페이지 성능저하를 막기위한 도구

<br>

### debounce의 의미

<br>

bounce:

(v) jump repeatedly up and down, typically on something springy.

반복적으로 위아래로 점프하는것, -전형적으로 스프링 같은 물체위에서

(n )a sudden increase or improvement in rating or value

비율이나 값의 갑작스러운 상승

<br>

debounce:

(v )비율이나 값의 하락, 반복을 제거하여 안정적으로 만들기

- debounce는 사전에 등제되어있지 않습니다. 제 개인 생각입니다.

<br>

### debounce 메서드

<br>

이벤트가 연속적으로 발생시, 이벤트 핸들러를 호출하지 않는다.

마지막 이벤트가 발생하고 지정된 delay 만큼의 시간이 흐르면 한번 호출한다.

<br>

### throttle의 의미

<br>

throttle

(v) to prevent something from succeeding

연속적인 흐름을 막는것

(n) a device that controls how much fuel goes into an engine

유체나 기름의 흐름을 조절하는 장치

<br>

### throttle 메서드

<br>

이벤트가 연속적으로 발생시, 특정한 delay 간격마다

한번씩만 이벤트 핸들러를 호출하게 한다.
