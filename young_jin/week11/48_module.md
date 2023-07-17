# 48. 모듈

자바스크립트는 원래 모듈을 지원하지 않았다.

import, export 자체가 존재하지 않았다.

한 프로젝트 내부라면, 파일이 달라도 자바스크립트는 스코프가 겹친다

### 모멘텀클론

다음 파일들은 전부 변수 선언 스코프를 공유한다.

```html
<script src="./public/js/clock.js"></script>
<script src="./public/js/todo.js"></script>
<script src="./public/js/quotes.js"></script>
<script src="./public/js/background.js"></script>
<script src="./public/js/weather.js"></script>
```

## ESM module

script 태그 내부에 `type="module"` 값을 넣어준다.

단, 이렇게될경우 파일경로를 통해 html을 브라우저에서 실행하면, 자바스크립트는 실행되지않는다.

로컬이든 가상환경이든 무조건 서버가필요하다.

확장자는 `mjs` 를 사용하길 권장한다.

`<script src="myscript.mjs" type="module"></script>`
