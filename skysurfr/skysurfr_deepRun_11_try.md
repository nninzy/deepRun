# 에러처리

## [try ... catch](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/try...catch) 문으로 에러 처리

> try...catch 문은 실행할 코드블럭을 표시하고 예외(exception)가 발생(throw)할 경우의 응답을 지정합니다.

## try 문의 3가지 형식

- try ... catch : try 안의 에러를 캐치 : 즉시 중단
- try ... finally : try 안의 에러를 캐치하지 않지만 반드시 한번은 실행
- try ... catch ... finally : 에러를 캐치하고 다음 동작도 실행, 에러 발생 확인해도 강제종료 하지 않고 프로그램 실행

### 예제

[중첩된 try 블럭](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/try...catch#nested_try-blocks)

```JavaScript
try {
  try {
    throw new Error('oops');
  }
  finally {
    console.log('finally');
  }
}
catch (ex) {
  console.error('outer', ex.message);
}

// Output:
// "finally"
// "outer" "oops"
```

실전 예쩨

[https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/videoController.js](https://github.com/pollyjean/wetube-reloaded/blob/main/src/controllers/videoController.js)

```JavaScript
export const removeComment = async (req, res) => {
  //...
  try {
    relatedVideo.comments = relatedVideo.comments.filter(item => String(comment._id) !== String(item));
    commentUser.comments = commentUser.comments.filter(item => String(comment._id) !== String(item));
    await Comment.findByIdAndDelete(id);
    await relatedVideo.save();
    await commentUser.save();
    return res.sendStatus(201);
  } catch (error) {
    console.log(error);
    return res.sendStatus(400);
  }
//...
}
```

## [Error](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Error) 오브젝트

> Error 객체는 런타임 오류가 발생했을 때 던져집니다. Error 객체를 사용자 지정 예외의 기반 객체로 사용할 수도 있습니다.

### 에러 오브젝트 종류

- Error 일반 에러
- SyntaxError 문법 에러 - 해석할 수 없는 문법
- ReferenceError 참조 에러 - 참조 할 수 없는 식별자를 참조했을 때
- TypeError 타입 에러 - 피연산자, 인수의 데이터 타입이 유효하지 않을 때
- RangeError 범위 에러 - 숫자값의 허용 범위를 벗어났을 때
- URIError URI 에러 - encodeURL / decodeURI 함수에 부적절한 인수를 전달했을 때
- EvalError eval 에러

## 사용자 지정 에러 만들기

`const error = new Error("invalid");`

### 에러 오브젝트 프로퍼티

- message 프로퍼티 : 생성자 함수에 인수로 전달한 메시지
- stack 프로퍼티 : 에러를 발생시킨 콜스택의 호출 정보 (디버그용, 비표준)

### 사용자 생성 에러를 던지기 throw

에러를 생성자로 만들고 발생시키려면 던져야 된다
try 블럭 내부에서 던지면 즉시 캐치 블럭 실행

```JavaScript
try {
  throw new Error("이런!");
} catch (e) {
  alert(e.name + ": " + e.message);
}
```

#### 에러의 전파

중간에 캐치하지 않으면 호출자 caller 방향으로 전파된다 -> 캐치하지 않으면 프로그램은 강제 종료

해당 실행 컨텍스트 -> 호출자 방향으로 -> 전역 실행 컨텍스트 방향으로 전파됨

setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다 -> 에러를 전파할 호출자가 존재하지 않는다
