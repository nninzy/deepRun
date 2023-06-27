# DOM

## DOM 표준안 DOM Level

- DOM Level 1 ~ 3 는 w3c에서
- 지금은 DOM Level 4 시대 : **[DOM : Living Standard](https://dom.spec.whatwg.org/)**

## DOM의 개념

Document Object Model

[https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)

> 문서 객체 모델(The Document Object Model, 이하 DOM) 은 HTML, XML 문서의 프로그래밍 interface 이다. DOM은 *문서의 구조화된 표현(structured representation)*을 제공하며 *프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공*하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다. *DOM 은 nodes와 objects로 문서를 표현*한다. 이들은 웹 페이지를 스크립트 또는 프로그래밍 언어들에서 사용될 수 있게 연결시켜주는 역할을 담당한다.

### HTML Text Parsing을 통해 Node 오브젝트 변환

하나의 엘리먼트를 태그, 속성 컨텐츠로 텍스트를 분해 파악

- 여는 태그 `<li`
- 속성 이름 `class=`
- 속성 값 `"item">`
- 컨텐츠 `Banana`
- 닫는 태그 `</li>`

### Tree Data Structure

계층적 구조를 표현하는 비선형 자료구조

- root node
- parent node
- child node
- sibling node
- leaf node
- edge(link)

> 노드 오브젝트로 구성된 트리 자료구조를 DOM이라고 한다.

### 주요 Node Type

- 문서 노드 Document Node : window.document (페이지 당 하나)
- 요소 노드 Element Node
- 어트리뷰트 노드 Attribute Node
- 텍스트 노드 Text Node

#### 기타 Node Type

- Comment Node
- Doctype Node
- DocumentFragment Node (여러개 노드 생성 추가시)
- white space node(공백 : space, tab, return(개행, 줄바꿈) 등)

### 노드 오브젝트의 상속 구조

> DOM은 ECMAScript에 사양에 정의된 표준 빌트인 오브젝트 Standard Built-in Object가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 오브젝트 Host Objects이다

DOM도 프로토타입 상속 구조를 가지고 있다

1. Object - EventTarget - Node
   1. [Node]
      1. Document
         1. HTMLDocument
      2. Element
         1. HTMLElement
            1. HTMLHtmlElement
            2. HTMLHeadElement
            3. HTMLMetaElement
            4. HTMLLinkElement
            5. HTMLScriptElement
            6. HTMLBodyElement
            7. HTMLUListElement
            8. HTMLLIElement
            9. ...
      3. Attr
      4. CharacterData
         1. Text
         2. Comment

> DOM API를 통해 HTML 구조와 내용 스타일 등을 동적으로 조작할 수 있다

## DOM 메서드와 프로퍼티

### 요소 노드 얻기 getElement(s)

#### document.getElementById(elementId)

- 하나의 html 문서 안에서 id 이름은 유일(unique)해야 하는 것이 원칙이나
- 여러개 있어도 에러는 나지 않고,
- 여러개가 있다면, 가장 첫번째 id를 get 한다
- 없으면 null 리턴

### element.getElementsByTagName(elementTagName)

- HTMLCollection(여러개의 요소 노드를 가짐, 유사배열에 이터러블) 오브젝트 리턴
- 특정 요소의 자식, 자손 요소를 탐색해서 반환
- 없으면 빈 HTMLCollection 오브젝트 리턴

### element.getElementByClassName(elementClassName)

- 클래스 이름으로 검색한다는 것 이외에는 tagName과 동일

### element.querySelector(cssQuerySelector)

- CSS Selector로 특정 HTML 요소를 특정한다
- 여러개면 가장 첫번째 요소를 리턴, 없으면 null을 리턴
- cssQuerySelector가 문법에 맞지 않으면 DOMException Error 발생

### element.querySelectorAll(cssQuerySelector)

- CSS Selector에 해당하는 모든 요소를 찾는다
- 해당 요소가 있으면 NodeList(유사배열에 이터러블) 리턴, 없으면 빈 NodeList 리턴
- cssQuerySelector가 문법에 맞지 않으면 DOMException Error 발생

#### querySelector vs getElementById

id 검색은 id로 나머지는 query로

### _element.matches(cssQuerySelector)_

- CSS Selector로 특정 요소 노드가 있는지(true/false) 확인
- 이벤트 위임 방식을 사용할때 유용
  - 특정 노드에 이벤트 걸고 하위의 특정 엘리먼트를 체크(matches)해서 해당(event.target)엘리먼트에 이벤트 실행하는 방식

## HTMLCollection, NodeList

### HTMLCollection

- tagName className의 결과물
- 노드 오브젝트의 *상태 변화를 실시간으로 반영*하는 살아있는(Live) 오브젝트
  - 그래서 for문으로 0 부터 length 까지 순회할때 중간에 _탐색 조건을 내부에서 바꿔버리면 갯수가 달라져_ 잘못된 결과가 나올 수 있다
    - 해결책 1 : length 길이부터 0까지 역방향으로 돌기
    - 해결책 2 : while문으로 해당 요소가 남지 않을때 까지 순회
    - 해결책 3 : HTMLCollection 대신 Array로 변환해서 [...elements] forEach, map, filter, reduce 등 고차함수로 사용가능

### NodeList

- querySelectorAll의 결과물
- 노드 오브젝트의 상태 변화를 실시간으로 반영하지 않는다
- forEach, item, entries, keys, values 메서드 제공
- node.childNode 프로퍼티는 자식 노드를 리턴하는데 이거는 실시간이다
  - 그러니까 배열로 변환해 쓰자 ...spread나 Array.from 쓰자
  - 배열쓰면 forEach, map, filter, reduce 등 고차함수도 쓸수 있다

## 노드 탐색 프로퍼티

- node.parentNode, node.previousSibling, node.nextSibling, node.firstChild, node.childNodes
- element.previousElementSibling, element.nextElementSibling, element.firstElementChild, element.lastElementChild, element.child
- getter만 존재하는 읽기 전용 접근자 프로퍼티, 할당하면 에러없이 무시한다
- 노드 탐색 시에는 공백 텍스트 노드(white space node)를 주의해야 한다

> Node와 Element의 차이? Element는 태그가 들어간것만이고 노드는 텍스트 요소들도 포함

### 자식 노드 존재 확인 메서드 node.hasChildNodes()

- 자식 존재 유무 반환
- element 자식만 확인하려면 element 관련 프로퍼티 사용

### 부모노드 탐색 node.parentNode

- 부모노드가 텍스트 노드인 경우는 없다

### 노드 정보 프로퍼티

#### node.nodeType

노드타입 상수 반환

- node.nodeType : ELEMENT_NODE 상수 1 리턴
- node.nodeType : TEXT_NODE 상수 3 리턴
- node.nodeType : DOCUMENT_NODE(최상단 노드) 상수 9 리턴

#### node.nodeName

노드 이름을 문자열로 반환

- node.nodeName : 요소 노드 - 태그 이름을 대문자 문자열로 반환 ("UL", "LI" 등)
- node.nodeName : 텍스트 노드 - "#text" 반환
- node.nodeName : 문서 노드 - "#document" 반환

## 요소 노드 조작

### node.nodeValue

- getter setter 다 가능
- 텍스트를 바꿀 수 있다
- 텍스트 노드일때만 텍스트를 리턴하고 딴거면 null 리턴

### node.textContent

- HTML 마크업 무시하고
  - 요소 노드의 텍스트와 자손노드의 텍스트를 getter
  - 요소 노드의 텍스트와 자손노드의 텍스트를 무시하고 setter
    - 값이 html처럼 생겼어도 파싱 안함
- HTML 무시하고 텍스트 리턴

#### node.innerText

- CSS 의존적이라 visibility: hidden 을 잘 지켜줌
- CSS 거쳐가서 textContent보다 느리다

## DOM 조작 manipulation

DOM 조작하면 리플로우 리페인트 발생하니까 조심해서 다뤄야 한다

### element.innerHTML 프로퍼티

- HTML 마크업을 지켜주면서 텍스트로 getter 해준다
- HTML 마크업을 지켜주면서 텍스트를 DOM으로 파싱해서 반영해준다
  - 이때 자식노드가 존재하면 제거된다
- _입력받은 데이터를 그대로 innerHTML에 할당하면 XSS (Cross-Site Scripting Attacks)에 취약하니 주의할 것_
  - HTML5는 script 태그 내의 코드는 실행하지 않는다
  - 태그 요소의 on이벤트는 가능해서 강제로 에러 발생할 수 있다
    - 방지용 라이브러리 HTML sanitization : [https://github.com/cure53/DOMPurify](https://github.com/cure53/DOMPurify)

### element.innerAdjacentHTML() 메서드

- 기존 요소에 영향주지 않고 위치를 지정해 새로운 요소를 삽입
- 첫번째 인자
  - `"beforebegin"`
  - `"afterbegin"`
  - `"beforeend"`
  - `"afterend`
- 두번째 인자
  - 삽입할 HTML 텍스트
- 삽입만 하기 때문에 innerHTML보다 빠르나 XSS 취약점은 동일

### createElement(elementName), createTextNode(text)

- 엘리먼트, 텍스트노드를 생성한다
- 어디에도 연결되지 않은 독립적인 상태

### node.appendChild(nodeObj)

- 인자로 받은 노드를 자식으로 뒤로(막내로) 붙여준다

> 여러 노드를 생성해서 document에 붙일때는 미리 다 조립해두고 붙이는게 리플로우 리페인트가 덜 일어난다

### node.insertBefore(newNode, childNode)

- 첫번째 인자 노드를 두번째 인자 노드의 앞에 붙인다
- 두번째 노드는 호출한 노드의 자식노드여야 하고 아니면 DOMException 에러난다
- 두번째 노드가 null이면 appendChild 처럼 작동한다

> appendChild, insertBefore로 기존 노드를 가져다 붙이면 노드가 이동한다

### node.cloneNode(true | false)

- false 거나 인자가 없으면 해당 노드만 클론해서 리턴(shallow copy)
- true면 하위노드까지 복제 리턴(deep copy)

### node.replaceChild(newChild, oldChild)

- oldChild를 newChild로 대체하고 oldChild는 제거
- oldChild는 node의 자식이어야 한다

### node.removeChild(child)

- 인수의 노드를 삭제한다
- node의 자식이어야 한다

## Attribute

### html attribute 프로퍼티

- 해당 속성의 초기 상태
- 공통으로 사용하는 global attribute (id, class, style, title 등)
- 이벤트에 사용하는 event attribute (on으로 시작하는)
- 특정 요소에만 사용할 수 있는 attribute 도 있다 (type, src, href 등)
- element.attributes 프로퍼티에 읽기 전용(getter)으로 담겨있다
  - 예를 들어 `element.attributes.id.value` 이런 식으로 접근 가능

### DOM의 attribute 프로퍼티 관련 get/set/remove 메서드

- 해당 속성의 최신 상태를 반영
- element.getAttribute(attributeName)
- element.setAttribute(attributeName, attributeValue)
- element.removeAttribute(attributeName)

> html의 프로퍼티는 html 속성의 초기 상태
> 최신 상태는 DOM의 프로퍼티에서 각 메서드로 관리 : 변경을 반영

#### element.attributes 와 DOM attribute 프로퍼티와의 관계

- id : element.attributes 와 DOM attribute 와 1:1 대응
- input 요소들의 value attribute도 1:1 대응
  - attributes는 초기값, 프로퍼티는 최신값
- class attributes는 className classList 와 대응
- for attributes는 htmlFor 와 대응
- td colspan 대응 프로퍼티가 없다
- textContent 도 대응 프로퍼티가 없다
- attribute 이름은 camelCase를 따른다 maxlength -> maxLength

> 이거 왠지 react에서 볼것만 같다

#### attribute의 타입

대부분 텍스트이지만 아닐수도 있다(checked는 true/false)

## 스타일 element.style

- 인라인 스타일만 가능
- 읽고 쓰기 가능 getter/setter
- 대응 오브젝트(CSSStyleDeclaration Object)가 있는데 이것도 camelCase 쓴다 backgroundColor
- 원래대로(kebab-case) 쓰려면 ["background-color"] 요렇게
- value값에 단위 지정이 필요하면 단위 붙여 문자열로 전달

### 요소에 적용된 CSS 스타일 참조

- 사실 위에꺼는 모든 스타일을 건드리진 못한다
- window.getComputedStyle(element[, pseudo])
  - 첫번째 인자에 해당하는 엘리먼트에 있는 모든 CSS 스타일을 참조
  - 두번째 인자는 pseudo element(:after :before 등)를 지정
  - CSSStyleDeclaration 오브젝트에 담아서 리턴한다

## HTML 클래스

### element.className

- 읽고 쓰기 가능 getter/setter
- 텍스트 공백으로 각 클래스구분

### element.classList

- 유사배열 오브젝트로 클래스 관리 DOMTokenList
- element.classList.add(className)
- element.classList.remove(className)
- element.classList.item(index) // return className string
- element.classList.contains(className) // return true/false
- element.classList.replace(oldClassName, newClassName)
- element.classList.toggle(className[, force]) // 첫번째는 토글 두번째는 true 리턴하면 강제 추가, false 리턴하면 강제 삭제(옵션)
