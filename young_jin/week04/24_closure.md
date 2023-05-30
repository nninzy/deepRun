# closure

함수형 프로그램언어만 가질수 있다.

     -함수의 반환값에 함수가 올수 있어야한다.

자신을 포함하고 있는 외부함수보다 더 오래 유지되며,

외부함수의 지역 변수에 접근할 수 있는 내부함수를 클로저(Closure)라고 부른다.

---

# 예시코드

```
const outerFunc = function () {
  const x = 10;
  const innerFunc = function () { console.log(x); };
  return innerFunc; // 이게 closure
}


const inner = outerFunc();
inner();
```

inner 함수는 자신의 상위 스코프인 outerFunc의 x를 가져온다.

<br>

![](source/closure.jpg)

- x를 자유변수(Free variable)라 부른다.

---
