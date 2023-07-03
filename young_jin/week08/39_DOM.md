# 39 DOM

## DOM

### DOM(Document Object Model)

<br>

웹 페이지의 구조화된 표현을 제공하는 API

- 웹페이지 조작을 위한 여러가지 property와 method를 제공

- 자바스크립트의 일부가 아니다. 단지 구현을 자바스크립트로 했을뿐

<br>

### DOM 기법

<br>

| 이름            | Real DOM                   | virtual DOM                   | Incremental DOM      |
| --------------- | -------------------------- | ----------------------------- | -------------------- |
| 설명            | 브라우저가 렌더링 하는 DOM | DOM 탐색이 쉽게 재구현한 객체 | Real DOM 사본을 보관 |
| 사용 프레임워크 | svelte                     | React, Vue                    | angular              |

<br>

### 공부 동기

React의 문법을 보고 DOM의 종류나 기법이 궁금해졌습니다.

- React에서는 ReactDom 객체의 메서드를 활용

```
import ReactDOM from "react-dom";

ReactDom.createRoot(DOM).render(<Componenet/>)

```

<br>

```
//함수로 정의

const Component = () => {

    return (
        <div>
        <h2>Component return jsx</h2>
        <p>jsx: javascript syntax eXtension for XML</p>
        </div>);

}

// 이게바로 ReactDOM
<Component/>

```

<br>

# DOM 의 동작

<br>

브라우저의 DOM은 정보 변화가 있을때마다 DOM Tree를 재생성

<br>

## 화면에 변화를 반영하는 방법

<br>

1. 탐색: DOM의 변경점 파악

2. 반영: 화면을 re-render

<br>

### 변경점 파악법

<br>

1. Dirty checking:

정기적으로 모든 노드의 데이터를 순회 - 변화탐지시 랜더링요청

- 재귀적으로 모든 노드를 상시 탐색하므로 굉장히 비효율적

<br>

2. Observable:

모든 구성요소 (컴포넌트)가 자신의 변경을 감지하는 도구 (event Listener) 보유.
데이터 변경시 스스로 re-render를 요청한다.

- DOM 을 통쨰로 다시생성해서 비효율적

<br>

### virtual DOM

등장배경

- 자바스크립트 DOM API의 설계는 그다지 효율적이지 않다
- DOM 위에서 직접 탐색, 업데이트하는것 - 비용을 많이 소모

=>

DOM의 정보를 그대로 갖고, 탐색속도가 빠른 객체를 만들자

- virtual DOM을 Observable 모드로 사용하자!

<br>

## virtualDOM : DOM변경 탐색을 위탁

<br>

1. virtualDOM 보관

2. 변경 발생시, virtualDOM 업데이트

3. virtualDOM을 토대로 DOM 수정

=>

```
과정은 늘었지만 탐색을 RealDOM에서 하지 않는것만으로
 효율 UP - RealDOM은수정만
```

<br>

## ![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220131100131/Group-1-2.jpg)

출처: https://www.geeksforgeeks.org/explain-dirty-checks-in-react-js/

<br>

# DOM: DOM tree

## Tree

그래프 이론에서 소개하는 그래프의 일종

<div style="display:flex justify-content: space-between;">
    <div>
    <h2> 그래프: 정점(노드)과 간선의 집합 </h2>
    <img src="https://www.tutorialspoint.com/discrete_mathematics/images/graph.jpg"
    style="width:300px; height:300px;" />
    </div>
    <div>
    <h2> 트리: 모든 정점(노드)간 경로가 주어져있고, Loop가 없는 그래프 </h2>
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Tree_graph.svg/1200px-Tree_graph.svg.png"
    style="width:300px; height:300px;" />
    </div>
</div>

<br>

### 트리의 구성요소

Node: 점
root: 최상단에 위치한 노드 -부모가 없다
leaf: 종단에 위치한 노드 - 자식이 없다

<br>

자바스크립트의 트리

- parent, child 관계를 갖는것은 전부다 트리이다.

<br>

# DOM Tree

<div style="display:flex;">
<img style="width:500px; height: 300px;" src="https://poiemaweb.com/img/dom-tree.png"/>
<ol style="font-size: x-large;">
    <li>Document: 문서 노드 
        <br>
        root 노드
    </li>
      <br>
    <li>Element: 요소 노드 
        <br>
        HTML TAG 표시
    </li>
      <br>
    <li>Attribute: 어트리뷰트 노드
        <br>
        HTML 태그의 attr 표기
    </li>
      <br>
    <li>Text: 텍스트 노드
        <br>
        HTML 태그 내부텍스트 표기
    </li>
</ol>
</div>

## <br>

# DOM 요소에 접근하는 방법

### Document.prototype : 한번만 사용가능

getElement

```

document.getElementById(string: id attr): HTMLElement | null
document.getElementsByClassName(string): HTMLCollection
document.getElementsByTagName(string): HTMLCollection
```

<br>

### Element.prototype: 여러번 사용 가능

querySelector

```
querySelector(string: css선택자): HTMLElement
querySelectorAll(string: css선택자) : NodeList
```

matches

```
matche
```

<br>

### HTMLCollection, NodeList

둘다 유사배열이자 이터러블

HTMLCollenction: living
NodeList: some-times living

<br>

# DOM을 탐색하는방법

<br>

## Node.prototype메서드

```
childNodes: NodeList
fisrtChild: Node object
lastChild: Node object

previousSibiling: Node Object
nextSibiling: Node Object
```

<br>

### NODE탐색을 잘 사용하지 않는 이유

Q. 왜 위치를 토대로 태그를 부르지 않고,
꼭 class나 id값으로 고유값을 지정하여 쓰는가?

```
ex) 잘 모르는 조카를 부를때 이름을 안외워도
상대적 위치로 편하게 호출할수있음

-누구네 첫째야~ 누구네 막내야
```

A. Text 노드가 존재하기때문

- 줄바꿈, 띄어쓰기 등의 공백은 텍스트 노드가 할당된다

HTML 태그만 호출이되는게아님

```
사람을 부르는상황

- 누구네 막내야~

=> 수철이 (요크셔테리어:2세) 등판시 곤란
```

---

HTML 문서

```
<html> //여기가 공백
    <head>
    </head>
    <body>
    </body>
</html>
```

![](https://i.stack.imgur.com/1ohA8.jpg)

<br>

ul을 사용한 예제

```
//ul의 firstChild


<ul><li></li></ul>
=> li


<ul>
<li></li>
</ul>
=> 공백 text
```

---

### 그런데 parentNode는 쓴다

text 노드는 자식이 없다.

<br>

## Element.prototype

Text노드를 거르기위해 사용하는 메서드들

```
childeren HTMLCollection

firstElementChild : HTMLElement | null

LastElementChild : HTMLElement | null


previousElementSibiling: HTMLElement | null
nextElementSibiling: HTMLElement | null
```

<br>

![](https://poiemaweb.com/img/HTMLElement.png)

# 요소 Node의 텍스트 조작

Node.prototype.nodevalue

- 자신의 text 조작

Node.prototype.textContent

- 자신 및 모든 자손의 text 조작

# DOM 조작

## innerHTML

- HTML 태그 내부 변경

<br>

### XSS 공격 cross site scripting attack

유저가 직접입력하는 값을 innerHTML로 받으면 안된다.

        - 악의적인 유저의 공격용 태그 삽입을 허용하기 때문`<script>`

```
// userInput => <script> 복잡한 해킹 툴</script>;

coredata.innerHTML = userInput

=> 정보 탈취 / 사이트 마비 가능
```

```
노마드 코더 강의의 댓글에서도
html코드를 입력시, <>내부 문자가 검열된다
```

<br>

## innerAdjecentHTML

```
<p>foo</p>

innerAdjecentHTML(position, DOMString)
```

![](https://poiemaweb.com/img/insertAdjacentHTML-position.png)

<br>

## innerText

- HTML 태그 내부의 text만 변경
- css에 종속적

자식이 있는 요소에는 Nodevalue, 없는 요소에는 textContent를 사용하자

<br>

# HTML 태그 조작 - 생성 및 추가

<br>

## 태그 생성

```
document.createElement()

```

<br>

## 태그 추가

<br>

### Node.prototype

<br>

```
parent.appendChild()

parent.insertBefore(new, oldChild)

```

<br>

### Element.prototype

<br>

지정위치에 삽입

```

parent.append() 마지막

parent.prepend() 첫번째

sibiling.after() 다음

sibiling.before() 이전
```

<br>

## 삭제 및 교체

<br>

교체

```
Node.prototype.replaceChild(new, old) new를 old로 교체
```

삭제

```
Node.prototype.removeChild
```

# Attribute

<br>

Tag 요소노드 하위 객체의 프로퍼트로 존재

```
// Li element의 attr(value)에 접근

const li = document.getElementById("todoli");

li.value ="fisrt-todo";
```

<br>

그런데, Attribute 노드가 따로있다.

![](https://poiemaweb.com/img/dom-tree.png)

## 역할: 초깃값 저장

## 예제 body - style attribute 설정

### Attribute 노드를 통해 설정

<br>

getAttribute/ setAttribute 사용

```
document.body.setAttribute("style", `background-image:이미지주소`);


css attriubute의 표기법
kebab-case를 그대로 사용중
```

<br>

### element의 attr 프로퍼티 사용

<br>

객체 프로퍼티 동적 변경

```
const bodyTag = document.querySelector("body");
bodyTag.style.backgroundImage = 이미지주소

property 접근법
camelCase로 접근
```

# 배운점

1. querySelector만 반복 사용할 수 있는 이유

Element.prototype 메서드라서

- getElement 시리즈는 Document.prototype 메서드

   <br>

2. 노마드코더에서 내 요약댓글 코드를 검열하는 이유:

XSS공격 방지

- innerHTML 이나 innerAdjecentHTML 을 사용하는것으로 추정
- textContent나 innerText 사용을 바람

<br>

3. 위치를 통한 자식태그 호출 및 비슷한 메서드의 존재 이유: Text 노드의 존재

   - 기왕이면 중간에 Element가 들어간 메서드를 사용

<br>

4. DOM의 중요성

   - 당장 취직목표가 프론트엔드가 아니더라도 중요한것 같다.
   - 웹스크래핑, 먼 훗날 개인 웹사이트제공 서비스 관리등에도 필수적
