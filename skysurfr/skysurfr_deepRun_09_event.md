# Event

[https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks/Events](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks/Events)

> 이벤트(event)란 여러분이 프로그래밍하고 있는 시스템에서 일어나는 사건(action) 혹은 발생(occurrence)인데, 이는 여러분이 원한다면 그것들에 어떠한 방식으로 응답할 수 있도록 시스템이 말해주는 것입니다.

## Event Handler

> Events are signals fired inside the browser window that notify of changes in the browser or operating system environment. Programmers can create event handler code that will run **when an event fires, allowing web pages to respond appropriately to change.**

- 이벤트가 언제 일어날까를 사용자가 결정하는게 아니라
- 브라우저에게 위임한다
- 특정 이벤트 프로퍼티에 함수를 할당하고
- 해당 이벤트가 발생할 때 할당한 함수를 브라우저가 호출

## Event Type

- Mouse Event
  - click / dblClick
  - mouseDown / mouseUp
  - mouseEnter / mouseLeave
  - mouseMove
    - event.clientX / event.clientY 마우스의 위치
  - mouseOver
  - mouseOut
- Keyboard Event
  - keyDown / keyUp
  - ~~keyPress (문자키를 눌렀을때만, deprecated 사용하지말것)~~
- Focus Event
  - focus / blur (no bubbling)
  - focusIn / focusOut (bubbling, addEventListener Only)
- Form Event
  - submit (텍스트 인풋에서 엔터, submit 버튼 클릭)
  - ~~reset (안씀)~~
- Value Change Event
  - input (입력 그 자체)
  - change (입력 후 요소가 포커스를 잃었을때 발생)
  - readyStateChange (HTML 문서의 로드, 파싱상태 변경 시)
    - document.readyState 프로퍼티의 값인 loading, interactive, complete 가 변경될때 발생
- DOM Mutation Event
  - DOMContentLoaded (HTML 문서의 로드 파싱이 완료 -> DOM 생성이 완료되었을 때)
- View Event
  - resize (window 오브젝트의 크기가 변경될 때 연속적으로 발생)
  - scroll (HTML 문서나 HTML요소 스크롤 시 연속적으로 발생)
    - [Intersection Observer API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)
      - 연속 스크롤 이벤트 발생시 부하, 무한 스크롤(Infinite-scroll), 지연 로딩(Lazy loading) 구현을 위한 스크롤 API
- Resource Event
  - load (DOMContentLoaded 후, 이미지, 폰트 등 리소스 로딩까지 완료되었을때 - 주로 window)
  - unload (문서나 하위 리소스가 언로드 중 일때)
    - beforeUnload (unload 전)
      - 실제 페이지를 떠날 것인지 묻는 대화 상자 표시(preventDefault 사용) -> 작동이 불안하다
  - error (리소스 로딩 에러)

## Event 등록 방식

### 이벤트 핸들러 어트리뷰트 방식

on{이벤트이름}으로 태그 속성으로 핸들러(함수) 등록 : 호출문 등록 (인수 전달을 위해)

### 이벤트 핸들러 프로퍼티 방식

on{이벤트이름}의 핸들러 프로퍼티에 함수를 바인딩하는 방식
하나의 이벤트에 하나의 핸들러만 되는 단점

```/* JavaScript */
$button.onclick = function() {
  console.log("button click");
}
```

#### 핸들러 프로퍼티 방식으로 등록한 이벤트 제거

```/* JavaScript */
$button.onclick = null;
```

### addEventListener 메서드 방식 이벤트 등록

- 하나의 eventTarget에 여러개 eventType 등록 가능
- 동일한 이벤트 핸들러는 하나만 등록

```/* JavaScript */
eventTarget.addEventListener("eventType", functionName [, useCapture{false|true}])
```

- eventTarget : 이벤트 타겟 엘리먼트
- eventType : on을 안붙인다
- functionName : 이벤트 핸들러 등록, 호출문 아니고 선언문 넣을 것
- useCapture
  - false (버블링, 생략가능 기본값)
  - true (캡처링)

#### removeEventListener

- 등록한 인수와 동일해야 제거 가능
- 이벤트 핸들러도 참조 안되는 화살표 함수 등으로 등록하면 제거 안된다

```/* JavaScript */
eventTarget.removeEventListener("eventType", functionName)
```

### 이벤트 오브젝트

- 이벤트 오브젝트는 이벤트 핸들러의 첫번째 인수로 등록

```/* JavaScript */
document.body.addEventListener("click", (event) => {
  console.log(event.clientX, event.clientY);
})
```

```/* JavaScript */
const handleInput = (e) => {
  console.log(e.target.value);
}
input.addEventListener("click", handleInput);
```

#### 이벤트 오브젝트 공통 프로퍼티

- event.type
- event.target : 이벤트 발생시킨 타겟
- event.currentTarget (this) : 이벤트 묶은 엘리먼트 타겟 (이벤트 핸들러 내부의 this)
- event.eventPhase : 전파단계
  - 0 없음, 1 캡처링, 2 타겟, 3 버블링
- event.bubbles : true/false
  - false only : focus/blur/load/unload/error/mouseEnter/mouseLeave
- event.cancelable : true/false
  - false only : focus/blur/load/unload/error/dblClick/mouseEnter/mouseLeave
- event.defaultPrevented : true/false
- event.isTrusted : true/false (실작동으로 일어난 이벤트냐 핸들러 안에서 click, dispatchEvent 메서드로 인위적으로 만든 이벤트냐 구분)
- event.timeStamp : 발생한 시간(밀리초)

#### 이벤트 오브젝트 마우스 프로퍼티

##### 마우스 포인터 위치 정보

![포지션 기준점](assets/images/ftJXC.png)
[via stackOverflow](https://stackoverflow.com/questions/6073505/what-is-the-difference-between-screenx-y-clientx-y-and-pagex-y)

- MouseEvent.clientX/Y 뷰포트 ViewPort 좌표계에서 마우스 포인터의 X/Y축 좌표
  - 브라우저 화면 기준
- MouseEvent.offsetX/Y 대상 노드의 안쪽 여백 경계를 기준으로 한 마우스 포인터의 X/Y축
  - 마우스가 클릭한 대상 엘리먼트 기준
- MouseEvent.pageX/Y 전체 문서를 기준으로 한 마우스 포인터의 X/Y축 좌표
  - 문서 전체 기준(안보이는 부분 포함)
- MouseEvent.screenX/Y 전역 (화면) 좌표계에서 마우스 포인터의 X/Y축 좌표
  - 모니터 기준

#### 키보드 이벤트 프로퍼티

- KeyboardEvent.altKey / KeyboardEvent.ctrlKey / KeyboardEvent.shiftKey / KeyboardEvent.metaKey : 특정 키 눌렀을때의 마우스 이벤트 발생 감지
- key : 발생한 키를 리턴
- ~~keyCode : 해당 키코드를 리턴(keyCode 말고 key 쓰세요 deprecated 됨)~~

## 기본 동작 멈춰

event.preventDefault();

## 이벤트 전파

여기서 설명했음

[skysurfr_deepRun_04_constructor.md](skysurfr_deepRun_04_constructor.md)

- DOM 구조를 생각해보면 거의 대부분 부모노드 위에 자식노드가 겹쳐있으니까
- 버블링은 부모로 전파
- 캡처링은 자식으로 전파
- 타겟은 도달
- event.target 은 클릭한놈
- event.currentTarget 은 이벤트건놈
- event.stopPropagation() 은 이벤트 전파 멈춰

### 이벤트 위임

이벤트 전파 속성을 이용해서 상위 (보통 body나 main 등에다 걸고)에 이벤트 걸고 핸들링 내부에서 조건 걸어 캐치

- 캐치는 주로 event.target.matches(querySelector) 를 이용

### this

이벤트용으로는 this 쓰지 말고 currentTarget, target 쓰셈

[skysurfr_deepRun_04_this.md](skysurfr_deepRun_04_this.md)

- 화살표 함수(무명함수)의 this는 상위 스코프, 아래의 경우 window

`element.onClick = (e) => { console.log(this) };`
`element.addEventListener("click", (e) => { console.log(this) })`

### 이벤트 생성자

- 이벤트는 암묵적으로 생성자 함수에 의해 생성

```/* JavaScript */
let eventNew = new Event("foo");
console.log(eventNew);

let eventClick = new MouseEvent("click");
let eventKeyboard = new InputEvent("change");
```

### 커스텀 이벤트

- 생성자 함수에 의해서 생성한 커스텀 이벤트(타입 이름도 따로 줄 수 있다)
- 일반적으로 CustomEvent 생성자 이벤트 함수를 사용
- 생성자 함수의 두번째 인자로 프로퍼티 전달
- isTrusted는 false됨

```/* JavaScript */
const newMouseEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 50,
  clientY: 100
});
```

#### 커스텀 이벤트 디스패치

인수로 받은 타입의 이벤트를 발생시키는 메서드
커스텀 이벤트 발생하게 할 수 있다
일반 이벤트와 차이 : 이 아이는 동기식(sync) / 다른 이벤트는 비동기식(async)

```/* JavaScript */
$button.dispatchEvent(newMouseEvent);
```

커스텀 이벤트는 addEventListener로만 등록 가능 (on 뭐시기를 못쓰니까)

근데 이거 책 계속 들여다 봐도 언제 어떻게 써야하고 왜 쓰는지 아직 잘 모르겠다 ~~이거 잘쓰면 왠지 클래스도 막 쓸 거 같고 프로토타입도 자유자재로 쓸 거 같고...~~
