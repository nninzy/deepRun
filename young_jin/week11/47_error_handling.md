# 에러처리

에러가 발생하지 않는 코드를 작성하는것은 불가능!
에러를 대처 혹은 출력하는것이 모두 중요하다.

## try - catch - finally

```js
try {
  //오류가 발생할 수 있는 코드
} catch (err) {
  //브라우저에 보여주기
  alert(err);
  // 콘솔에 보여주기
  console.log(err);
} finally {
  // 무조건 실행되는 코드
}
```

`try{}`에 오류가 발생할 가능성이 있는 코드를 작성.

에러 발생시 제어흐름이 `catch{}` 쪽으로 넘어간다.

`finally{}` 는 언제나 실행된다.

### setTimeout은 못잡는다

그래서.. setTimeout 내부 비동기 콜백에 사용해줘야한다.

```js
setTimeout(() => {
  try {
  } catch (e) {
  } finally {
  }
});
```

### throw Error

일명 `에러 던지기`

인자로 들어온것이 `string`이 아니면 에러를 던지는 함수

```js
const returnString = (str) => {
  try {
    if (typeof str !== "string") {
      throw new Error("this is not a string");
    }
    const greet = `${str}`;
    return greet;
  } catch (e) {
    console.log(e);
    const greet = `default`;
    return greet;
  }
};

getString([]);
```

콘솔에 출력되는 코드

```s
Error: this is not a string
    at getString (<anonymous>:4:13)
    at <anonymous>:1:1
'default'
```

<br>

제가 유투브 클론할때 사용한 에러 추적법

1. morgan을 본다.
2. 함수에 `console.log`를 도배해서, 실행 분기 및 변수를 확인한다.

댓글 작성 함수

```js
export const uploadComment = async (req, res) => {
  console.log("uploadComment is running"); // 함수 실행
  const userId = req.session.user._id;
  const text = req.body.text;
  const videoId = req.params.id;

  // 들어온 변수 확인
  console.log(`uploadComment vars: text: ${text}, videoId: ${videoId} `);

  const video = await Video.findById(videoId);
  if (!video) {
    // 분기 실행알림
    console.log("uploadComment: fail video not found");
    return res.sendStatus(404);
  }

  const user = await User.findById(userId);
  if (!user) {
    // 분기 실행알림
    console.log("uploadComment: fail user not found");
    return res.sendStatus(404);
  }

  const comment = await Comment.create({
    text: text,
    owner: userId,
    video: videoId,
  });
  video.comments.push(comment._id);
  await video.save();

  user.comments.push(comment._id);
  await user.save();
  // 함수 실행 성공 알림
  console.log("uploadComment: succesfully operated");
  return res.status(201).json({ newCommentId: comment._id });
};
```
