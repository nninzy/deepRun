# 1. event-driven-programing

### event: 사건

```
Events are things that happen in the system you are programming,
 which the system tells you about so your code can react to them.

이벤트는 프로그래밍 하고있는 시스템에서 발생하는 것이다,
시스템이 이벤트의 발생을 알려주고, 당신이 작성한 코드는 이에 반응할 수 있다.
```

### event-driven-programing

프로그래밍 패러다임.

프로그램은 이벤트를 처리하기 위한 것이다.
발생하는 이벤트에 의해 실행되는 코드(함수)가 결정된다.

# 2. event의 종류

브라우저는 이벤트를 감지한다.

이벤트에는 다양한 종류가 있다.

입/출력장치(키보드, 마우스 등) 조작
브라우저상태나 특정 DOM 요소 조작 등에 의해 발생한다.

- UI 및 브라우저 동작

```
Event	Description
load	웹페이지의 로드가 완료
unload	웹페이지가 언로드 (주로 새로운 페이지를 요청한 경우)
error	자바스크립트 오류, 요청한 자원이 존재하지 않는 경우
resize	브라우저 창의 크기를 조절
scroll	사용자가 페이지를 위아래로 스크롤할 때
select	텍스트를 선택했을 때

```

- 입 출력장치

```
키보드

keydown	키를 누르고 있을 때
keyup	누르고 있던 키를 뗄 때
keypress	키를 누르고땔 때

마우스

click	마우스 버튼을 클릭
dblclick	마우스 버튼을 더블 클릭
mousedown	마우스 버튼을 누르고 있을 때
mouseup	누르고 있던 마우스 버튼을 뗄 때
mousemove	마우스를 움직임 (터치스크린에서 동작하지 않는다)
mouseover	마우스를 요소 위로 움직였을 때 (터치스크린에서 동작하지 않는다)
mouseout	마우스를 요소 밖으로 움직였을 때 (터치스크린에서 동작하지 않는다)

```

- DOM 조작

```
focus/focusin	요소가 포커스를 얻었을 때
blur/foucusout	요소가 포커스를 잃었을 때


FORM

Event	Description
input	input 또는 textarea 요소의 값이 변경
 	contenteditable 어트리뷰트를 가진 요소의 값이 변경
change	select box, checkbox, radio button의 상태가 변경
submit	form을 submit (버튼 또는 키)
reset	reset 버튼을 클릭 (최근에는 사용 안함)


Clipboard

cut	콘텐츠를 잘라내기할 때
copy	콘텐츠를 복사할 때
paste	콘텐츠를 붙여넣기할 때
```

---

# 3. Event handler

특정 이벤트를 감지하면 실행할 함수

```
// 이벤트 핸들러
const handleListen = () => {
    console.log(`server is running on port: ${PORT}`);
}

app.listen(PORT, handleListen)
```

다양한 방식으로 등록이 가능하다.

## attribute 이벤트 핸들러 방식

= 인라인 이벤트 핸들러방식

HTML의 attribute(요소) 에 이벤트핸들러를 등록한다.

```
 <button onclick="clickHandler()">Click me</button>
```

여러개의 이벤트 핸들러를 등록하고 싶은경우

`;`를 사용하여 구분해준다.

<br>

### 주의할점

현재 일반적인 자바스크립트 웹개발은 html 파일과 자바스크립트파일을 분리하고있다.

attribute 방식은 CBD (Component Based Development)방식을 사용하는 프레임워크
말고는 사용하지 않는다.

- ex) react, vue, angular

## property 이벤트 핸들러 방식

자바스크립트에서 DOM 객체를 생성하여
`on이벤트이름` 형식으로 미리 정의된 타입의 이벤트 리스너에
이벤트 핸들러를 등록한다.

이떄 이벤트 리스너 이름은 `camelcase`를 따르지 않는다.
전부 소문자를 사용한다.

```
//html
<div class="root"> </div>

//js
const root = document.querySelector(.root);

root.onclick = () => {
    alert(`root div is clicked`);
}
```

이 방식은 하나의 이벤트 핸들러만 등록할 수 있다.

## addEventListener 방식

`addEventListener` 메서드를 이용하여 이벤트 및 핸들러를 지정해준다.

![](https://poiemaweb.com/img/event_listener.png)

- 커스텀 이벤트도 받을 수 있다.

- 캡처링, 버블링 지원

- 하나의 이벤트에 다수의 이벤트 핸들러를 추가 할 수 있다.

- 모든 DOM 요소(html, xml, svg)에 동작한다.

- removeEventListener메서드를 통해 바인딩을 해제할 수 있다.

예시코드

# this binding

|      | attribute | property               | addEventListener             |
| ---- | --------- | ---------------------- | ---------------------------- |
| this | 전역객체  | 이벤트에 바인딩된 요소 | 이벤트리스너에 바인딩된 요소 |

# 이벤트의 전파 캡처링과 버블링

![](https://poiemaweb.com/img/eventflow.svg)

1. td 클릭

2. 이벤트 객체 및 이벤트 발생

3. 하향이동: window -> document. -> td(target)

   - 캡처링

4. td

   - 이벤트 타겟 단계

5. 상항이동: td->.. -> window

   - 버블링

`addEventListener`의 3번째 인자는 캡처링 사용여부를 나타내는 boolean

### 버블링

```
<body>
  <p>버블링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    });

    // 버블링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    });

    // 버블링
    button.addEventListener('click', function () {
      console.log('Handler for button.');
    });
  </script>
</body>
```

버블링 진행 순서대로 console.log가 출력된다.

```
Handler for button.
Handler for paragraph.
Handler for body.
```

### 캡처링

```
<body>
  <p>캡처링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 캡처링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    }, true);

    // 캡처링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    }, true);

    // 캡처링
    button.addEventListener('click', function () {
      console.log('Handler for button.');
    }, true);
  </script>
</body>
```

콘솔 출력 순서 - 하향식으로 퍼저나가는걸 볼 수 있다

```
Handler for body.
Handler for paragraph.
Handler for button.
```

### 이벤트 위임 (delegation)

```
<ul id="post-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

위의 li 태그마다 이벤트를할당해주고 싶다면 6개의 이벤트리스너 및 핸들러가 필요하다.

이벤트 위임은 부모 개체 하나에 이벤트핸들러를 할당해주고,
내부에서 자식을 판별할 수 있게 해준다.

redux-store랑 여러모로 닮았다!

```
 <script>
    const msg = document.querySelector('.msg');
    const list = document.querySelector('.post-list')

    list.addEventListener('click', function (e) {
      // 이벤트를 발생시킨 요소
      console.log('[target]: ' + e.target);
      // 이벤트를 발생시킨 요소의 nodeName
      console.log('[target.nodeName]: ' + e.target.nodeName);

      // li 요소 이외의 요소에서 발생한 이벤트는 대응하지 않는다.
      if (e.target && e.target.nodeName === 'LI') {
        msg.innerHTML = 'li#' + e.target.id + ' was clicked!';
      }
    });
```

## 기본동작변경

이벤트별 기본 동작, 혹은 이벤트 전파를 멈추는 메소드가 있다.

```
Event.preventDefault() // 기본동작 멈춤
 Event.stopPropagation() // 이벤트 점파 멈춤
```

custom 이벤트에는 기본동작이 없다.
