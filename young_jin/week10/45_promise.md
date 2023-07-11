# 45_Promise

이 글은 제가 프로미스의 작동 방식 및 구조가 궁금하여

인터넷 떠돌아다니는 글들을 정리한 글입니다.

글을 읽으실때 haskell 의 정의를 자세히 볼 필요는 없습니다.

해석과 js 코드를 중점적으로 읽으시면 됩니다.

```
gpt

it seems that the text you provided
does not contain any specific errors or inaccuracies.

It discusses the concepts of functor, applicative, and monad in Haskell,
and provides examples and interpretations for each concept.

The explanations appear to be correct and coherent.
```

# 오류를 줄이는 방식

1. 모듈화: 의존성 분리

   - 전역변수사용을 최대한 줄이고, 함수단위로 지역변수를 컨트롤 한다.

     - 상호 의존성을 줄여, 사이드 이펙트를 방지한다.

<br>

2. 모듈 내부에서 오류처리

   - 모듈 스코프내부는 망하지만, 전체 스코프 (프로그램)이죽는것을 막아준다.

<br>

위 두가지 사항을 만족하는 try-catch 구문

내부스코프 에서 오류가 발생해도, 전체스코프 (프로그램) 은 안전하다!

```js
{
  // 상위 스코프
  try {
    /*
     * 오류 발생 가능성이있는 코드 진행
     * 오류 발생시 이 스코프 내부는 망함
     */
  } catch (error) {
    /*
     * 일단 try블록은 버리고 오류메시지 전송
     */
  }

  /*
   *  try catch 망해도 여기 (상위스코프)는 안망가짐
   */
}
```

## 그렇다면 특정 데이터 타입 내부에서 로직을 처리하면 어떨까?

<br>
Q. 모듈로도 쓸 수 있고, 내부에서 함수도 처리할 수 있는 데이터 타입 없나요?

<br>

A. category theory 분야의 `monad` 개념이 있습니다.

<br>

### category theory와 monad

<br>

![](https://teamdable.github.io/techblog/assets/images/moand-and-functional-architecture/category.png)

category theory는 사상(함수의 일반화)에대한 분류를 연구하는 학문

사상은 한 집합에서 다른 집합으로의 원소간 대응 관계이다.

- X -> Y

우리도 기초적인걸 해본적이 있다.!

### 사상의 분류

<br>

함수

```
집합 X의 원소를 x, Y의 원소를 y 라하자.


1. 모든 x 값에 대응하는 y 값이 존재한다.
2. x 값하나에 대응되는 y 는 한개이다.

ex) y = sinx
```

일대일 함수

```
1. 하나의 x값에 대응되는 y 값은 다른 x 값에 대응되지 않는다.

y= f(x)

- y1 = y2 === x1 = x2
- f(x1) = f(x2) === x1 = x2

ex) y = x
```

방정식

```
x값에 y 값이 두개 이상 대응가능하다.

x^2 + y^2 = 1
```

### monad?

<br>

monad는 사상 내부에서 사상을 처리하고 사상을 반환하는 사상의 일종이다.

어렵지않다.

<br>

클로저도 함수 내부에서 함수를 실행하고, 반환값도 함수인 함수의 일종!

<br>

### 모나드 개념의 상속 구조

functor(함자) -> applicative(적용자) -> monad(모나드)

포함 관계도

```
사상(map){
    함자(functor){
      적용자(applicative){
        모나드(monad){}
    }
  }
}
```

<br>

## functor: 함자

<br>

### functor in haskell

<br>

`타입 생성자 f`에 대해 적절한 함수 `fmap :: (a -> b) -> f a -> f b`이 존재하여 다음 성질들을 만족할 때,

`타입 생성자 f`를 `함자(Functor)`라고 한다.

```
1. 항등함수 id :: a -> a에 대해 fmap id :: f a -> f a도 항등함수다.

2. 모든 함수 f :: b -> c와 g :: a -> b에 대해 fmap (f . g) ≡ fmap f . fmap g이다.
```

<br>

### 해석

<br>

`타입생성자 f`를 `변수를 가두는 상자` 라고 생각하자.

`fmap` 은 함수`(a->b)`와 상자`f`를 입력받아

상자속에서 함수를 처리하게하는 함수이다.

함수의 처리가

`멱등성` (dempotence), `함수의 결합법칙 ` ( associative property)

이 성립하게 하는 `fmap`이 존재한다면 `f`를 `functor` 이라고 한다.

<br>

예시 비유

fmap

```js
// 타입생성자 f : [] - 변수를 가두는 상자

// [3]에서 내부 값을 꺼내지않고 (* 2) 함수를 적용 를 해주고 싶다.

// a->b
const double = (number) => number * 2;

[3].map(double); //return [6]
```

<br>

멱등성

```
같은 입력값을 넣으면 언제나 같은 출력값을 보장하는 성질

y = 2x : x에 1을 넣으면 언제나 y 값은 2이다.

- 멱등성이 보장된다

gpt : 같은질문 (같은값)을 입력해도 다른답변을 출력한다.

- 멱등성이 없다.
```

<br>

함수의 결합법칙

```
조삼모사

f ⊙ g ⊙ h === f ⊙ (g ⊙ h)

const double = (number) => number * 2;
const plusThree = (number) => number + 3;

// f: map,  g: plusThree, h: double

[3].map(double).map(plusThree);

[3].map(plusTree(double))
```

<br>

타입생성자 f (`[]`) 는 `함수의 결합법칙`, 과 `멱등성`을 갖는 함수
fmap(`map`) 을 가지고 있으므로, `functor`이다.

<br>

## applicative: 적용자

<br>

### applicative in haskell

<br>

함자 f에 대해 적절한 함수 `pure :: a -> f a`와 `(<*>) :: f (a -> b) -> f a -> f b`가 존재하여

다음 성질들을 만족할 때, f를 `적용자(Applicative)`라고 하며,

임의의 타입 a에 대해 `f a`를 `액션(action)`이라 한다.

```
1. 모든 함수 f :: a -> b와 액션 v :: f a에 대해 pure f <*> v ≡ fmap f v이다.

2. 모든 액션 u :: f (a -> b)와 값 y :: a에 대해 u <*> pure y ≡ fmap (f -> f y) u이다.

3. 모든 액션 u :: f (b -> c), v :: f (a -> b)와 w :: f a에 대해 fmap (.) u <*> v <*> w ≡ u <*> (v <*> w)이다.
```

### 해석

<br>

`상자내부의 함수`를 `상자 내부의 변수`에 적용할 수 있게하는 함수 `<*>` 가 존재시

그 `상자(타입)`을 `적용자`(applicative) 라고 한다.

- a 의 `액션` f a: `a를 넣은 상자`

```js
//역시 배열을 [] 상자 라고생각.
//상자내부의 함수를 상자내부의 변수와 사용

const functionBox = [(x) => x + 1, (x) => x * 2]; // 함수가 든 상자
const valueBox = [1, 2, 3]; // 변수가 든 상자

const results = functionBox.flatMap((fn) => valueBox.map(fn));

// result [2, 3, 4, 2, 4, 6]
```

따라서 `[]`는 내부의 함수를 내부의 값에 적용할 수 있는 함수 `<*>` 이 존재한다.

적용자 `applicative`이다.

<br>

### monad 모나드

<br>

### monad in haskell

<br>

적용자 m에 대해 적절한 함수 `(>>=) :: m a -> (a -> m b) -> m b`가 존재하여
다음 성질들을 만족할 때, m을 `모나드(Monad)`라고 한다.

- `(>>=) :: m a -> (a -> m b) -> m b` `flatMap` 이라 한다.

```
1. 모든 값 x :: a와 함수 k :: a -> m b에 대해 pure x >>= k ≡ k x이다.

2. 모든 액션 u :: m a에 대해 u >>= pure ≡ u이다.

3. 모든 액션 u :: m a, 함수 k :: a -> m b와 h :: b -> m c에 대해 u >>= (\x -> k x >>= h) ≡ u >>= k >>= h이다.

4. 모든 액션 u :: m a와 함수 f :: a -> b에 대해 u >>= (pure . f) ≡ fmap f u이다.

5. 모든 액션 u :: m (a -> b)와 v :: m a에 대해 u >>= (\f -> v >>= (\x -> pure (f x))) ≡ u <*> v이다.
```

### 해석

(>>=) 는 fmap과 `<*>` 보강판

`fmap :: (a -> b) -> f a -> f b`

fmap 다시 살펴보기

```js
//f : []
//a->b : num => num + 3
//f a : [1,2,3]

[1, 2, 3].map((num) => num + 3);

//f b : [4,5,6]
return [4, 5, 6];
```

fmap의 문제: 이중포장

```js
// 그런데 a->b가 상자를 반환한다면?
// a -> f b 라면
["hello", "hi", "bye"].map((item) => item.split(""));

// 반환값은 이중포장 상자이다.
[
  ["h", "e", "l", "l", "o"],
  ["h", "i"],
  ["b", "y", "e"],
];
```

flatMap은 1단 포장을 보장하는 메서드이다.
`(>>=) :: m a -> (a -> m b) -> m b`

```js
arr.flatMap(callback(currentValue[, index[, array]])[, thisArg])

let arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]);
// [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]
```

즉 모나드는 2중 포장 상자를 반환하는것을 방지하는 함수를 갖는 상자이다.

### array는 모나드?

<br>

배열은 엄밀한 모나드가 아니다.

상자와 배열은 시각적 이해를 돕기위한 도구

실제 모나드의 `상자` 는 아예 다른 모듈을 뜻한다.

- 모나드는 내부에 여러 상자를 다룰 수 있다.

  - 여러 모듈을 가질 수 있다.

  - 오류 발생시 오류를 담는 상자를 사용할 수 있다.

    - 해당 상자내부 코드는 무력화되겠지만, 다른상자내부나 외부는 괜찮다.

위의 조건을 만족하는것이, 프로미스이다.

# promise

<br>

1. 모듈화

   - 상자내부에서 코드진행

2. 모듈내부 자체 오류 처리

   - 상자 내부 상자에서 오류 처리

![](https://teamdable.github.io/techblog/assets/images/moand-and-functional-architecture/monad.png)

<br>

상자(모나드)는 `일반 값을 반환하는 함수`를 받아서 처리된 값을 상자로 포장하여 반환

`상자를 반환하는 함수`를 받으면 추가적인 포장 없이 1중 포장된 상자를 반환.

### 포장된 값을 전달하는 이유

<br>

오류가능성이 있는 코드를 노출시키고 싶지 않기때문에

<br>

```js
class MyMonad {
  constructor(value) {
    this.value = value;
  }

  static of(value) { // 이중포장 방지
    if (value instanceof MyMonad) return value
    return new MyMonad(value);
  }

  chain(fn) {
    //reject + resolve
    if (this.value === null || this.value === undefined) {
      return MyMonad.of(null);
    }
    return MyMonad.of(fn(this.value));
  }

  resolve(value) {
    // 프로미스 상태를 fulfilled로 바꿈
    return MyMonad.of(value);
  }

  reject(error) {
    // 프로미스 상태를 rejected로 변경
    return MyMonad.of(error);
  }
}

// 외부 api: 비동기 함수이며 오류 가능성이 있는 함수들
const getUserId = () => {
  return "userID";
};

const getUserEmail = (userId) => {
  return  MyMonad.of(
    setTimeout(() => {
      const user = {
        id: userId,
        name: "young-jin",
        email: "likeRudin@github.com",
      };
      resolve(user);
    }, 1000);
  );
};

MyMonad.of(getUserId()) // value: "userID"인 모나드를 반환
  .chain(getUserEmail) // 상자를 반환하나,
  .chain((user) => MyMonad.of(user.email)) // 상자에서 user.email을 포장한 상자를 반환
  .chain((email) => console.log("Email:", email)); // console.log로 상자내부로 값을 표시
```

위의 모든 로직은 모나드 블럭(프로미스) 내부에서 처리된다.

<br>

이중포장 상자반환을 방지하는이유: 오류를 포장된 상자를 넣고 싶으니까!

```js
const promiseAjax = (method, url, payload) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open(method, url);
    xhr.setRequestHeader("Content-type", "application/json");
    xhr.send(JSON.stringify(payload));

    // 이벤트 리스너
    xhr.onreadystatechange = function () {
      // 서버 응답 완료가 아니면 무시
      if (xhr.readyState !== XMLHttpRequest.DONE) return;

      if (xhr.status >= 200 && xhr.status < 400) {
        // resolve 메소드를 호출하면서 처리 결과를 전달
        resolve(xhr.response); // Success!
      } else {
        // reject 메소드를 호출하면서 에러 메시지를 전달
        reject(new Error(xhr.status)); // Failed...
      }
    };
  });
};
```

우리의 Promise

```
const getWeather = function (lan, lon) {
    const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${lan}&lon=${lon}&appid=${API_KEY}&units=metric`;


     fetch(apiUrl)
     //response 가 들어있는 상자 반환

     .then((response)=> response.json())
     // 상자내부에서 response를 .json 함수 적용후 해당값을 포장한 상자 반환

     // 다시 상자내부에서 방금 .json처리된 값에 필요한 로직 처리
     .then((data)=>{
        console.log(data);
        city.innerText = data.name; // 전역변수를 호출지점
        weather.innerText = `${data.weather[0].main} / ${data.main.temp}`;
    });

}
```

프로미스 및 배열복사 메서드 (map, filter) 상자 내부 처리의 이점

1. 모듈화

- 내부에서 다루는 요소를 외부 스코프에 노출시키지 않는다

  - 사이드 이펙트를 줄임

2. 체이닝이 가능

   - 상자 내부에서 로직을 처리한 후 반환하는 값도 상자

   - 메서드의 연쇄호출이 가능하다.

```
array.map().map()
fetch().then().then()

 과 같이 연쇄적인 호출 처리 로직이 직관적이다.
```

프로미스의 이점

3. 내부에서 오류처리

   - 내부에 상태를 나타내는 변수가 존재

   - 그 변수의 상태에따라 다른 상자를 처리

<br>

| pending   | 비동기 처리 수행 전     | resolve 또는 reject 함수 호출 전       |
| --------- | ----------------------- | -------------------------------------- |
| fulfilled | 비동기 처리 수행 (성공) | resolve 함수가 호출                    |
| rejected  | 비동기 처리 수행 (실패) | reject 함수가 호출된 상태              |
| settled   | 비동기 처리 수행        | resolve 또는 reject 함수가 호출된 상태 |

# functor, applicative, monad 요약

![](https://i.imgur.com/d5Y9CQv.png)
