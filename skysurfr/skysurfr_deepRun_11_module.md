# Module

애플리케이션을 구성하는 개별 요소로 사용 가능한 코드 조각
각각의 모듈은 모듈 스코프를 가진다

모듈에서 명시적으로 선택해서 내보낼 수 있다 export
다른 모듈에서 export 한 것 중 전체 또는 선택해서 가져올 수 있다 import

개별적으로 존재하다가 필요에 따라 재사용

## ESM 방식

모듈 안의 변수들은 모듈 스코프이기 때문에 전역변수가 없다

### 모듈 방식을 안쓰면?

모듈 스코프를 갖지 않는다
전역변수가 있을 수 있다

### HTML에서 script로 모듈 불러오기

.js도 되지만 ESM임을 확실히 하기 위해 .mjs 권장

```HTML
<script type="module" src="app.mjs"></script>
```

### export

하나만 내보내기
export default 는 기본적으로 이름 없이 하나의 값을 export

```JavaScript
export default videoRouter;
```

개별로 내보내기

```JavaScript
export const getJoin = (req, res) =>
  res.render("join", { pageTitle: "Join SkyTube" });
export const getLogin = (req, res) => {
  return res.render("login", { pageTitle: "Login" });
};
```

오브젝트로 내보내기

```JavaScript
const pi = Math.PI;

function square(x) {
  return x*x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

export { pi, square, Person}
```

### import

- .js 는 생략할 수 있는데 .mjs 일 경우 확장자 생략 불가
- 특정 이름으로 export 한거는 그 식별자로 import 해야 한다
- as로 식별자를 변경할 수 있다
- export default로 한거는 { } 없이 임의의 이름으로 import 한다

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/import](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/import)

```JavaScript
import defaultExport from "module-name";
import * as name from "module-name";
import { export1 } from "module-name";
import { export1 as alias1 } from "module-name";
import { export1 , export2 } from "module-name";
import { foo , bar } from "module-name/path/to/specific/un-exported/file";
import { export1 , export2 as alias2 , [...] } from "module-name";
import defaultExport, { export1 [ , [...] ] } from "module-name";
import defaultExport, * as name from "module-name";
import "module-name";
let promise = import("module-name");
```
