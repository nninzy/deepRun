# RegExP

정규 표현식(Regular Expression)

일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식언어

<br>

## 형식언어 구성요소

<br>

형식언어

```
특정한 법칙들에 따라 적절하게 구성된 문자열들의 집합
```

문자열

```
 문자집합에 포함되는 문자들의 유한 열거
```

문자집합

```
언어에서 사용하는 문자(alphabet)의 집합
```

<br>

## 정규언어

유한 오토마타를 사용하여 생성하거나 인식할 수 있는 언어

```
공집합 Ø은 정규 언어이다.

각 글자 a ∈ Σ에 대해 한원소집합 {a}는 정규 언어이다.

A가 정규 언어이면, 클레이니 스타를 붙인 A*도 정규 언어이다. (따라서 빈 문자열로만 이루어진 {ε}도 정규 언어이다.)

A와 B가 정규 언어이면, 합집합 A ∪ B와 문자열 연결 AB도 정규 언어이다.

그 밖의 언어는 정규 언어가 아니다.
```

<br>

## 정규언어 예시

문자 집합

```
{0,1}
```

문자열

```
0
1
01
11111000000
```

언어

```
Language = {문자열 | 문자는 {0,1}의 원소 }
```

정규표현식으로 나타낸 언어

```
^[0-1]*$
```

기호 해설

```
[]: 범위 기호

^: 시작 문자

*: 0번이상 반복

$: 끝나는 문자

```

<br>

# RegExp 역사

등장

```
정규언어를 기술하기위해 사용되던 표기법,
```

쓰임새

```
특정 패턴을 가진 문자열을 검색하거나 추출 및 치환하는데 유용,
자바, 파이썬, ECMA스크립트의 표준 문법으로 통합
```

<br>

## 자바스크립트의 RegExp객체

<br>

### 구성요소

- 패턴: 검색할 패턴
- 플래그: 검색조건

### 생성하기

- 1. 리터럴

```
/패턴/플래그

const regexp = /hello/im

플래그는 i, g, m, u, y 등이 있다.
여러개의 플래그를 이용할경우 붙여서 써준다
```

- 2. RegExp 생성자 함수

```
new RegExp
(패턴, 플래그)
(정규표현식)

// 패턴은 string이나 template literal일 필요가 없다

const regexp = new RegExp(/hello/, 'im');
const regexp = new RegExp('hello', 'im');
const regexp = new (/hello/im)
```

## wetube 복습

<br>

- 검색기능

```
export const search = async (req, res) => {
  const { keyword } = req.query;
  let videos = [];

  // 입력받은 키워드가 존재시 정규표현식 생성
  if (keyword) {
    videos = await Video.find({
      title: { // 키워드로 끝나는지 검사하는 정규표현식
        $regex: new RegExp(`${keyword}$`, "i"),
      },
    }).populate("owner");
  }
  return res.render("search", { pageTitle: "Search", videos });
};
```

- 비디오 라우터 아이디

```
videoRouter.get("/:id([0-9a-f]{24})", watch);


정규표현식 :id()내부의 문자들

[0-9a-f]{24}

param으로 전달되는 videoid의 패턴

0부터 9까지의 숫자, a부터 f 까지의 문자가
24번 이하로 반복되는 패턴을 가짐
```

<br>

## 기호 추가 정리

<br>

반복기호: 문자열 뒤에서 반복여부를 나타냄

```
n번이상 반복: {n,}

m번이상 n번이하 반복: {m, n}

1번이상 반복: + === {1, }

```

추상 문자열 정의 기호

- /hello/ 와 같이 구체적인 문자열이 아닐때 사용

```


\D === 숫자가 아닌 문자

\w === 숫자, 문자, 언더스코어

\W === \w의 여집합

추상 문자열 클래스 정의: []

범위기호 : -

[a-f]: 모든알파벳

[0-9]: 모든 숫자 문자 === \d

^ === not 기호

```

<br>

## 메서드

<br>

RegExp.prototype

```
RegExp.exec(문자열) -> array | null
RegExp.test(문자열) -> boolean
```

String.prototype

```
문자열.match(정규표현식) -> array

array는 첫번쨰 매칭정보를 담고있다.

g플래그를 사용시 array는 매칭된 모든 값을 포함한다


문자열.matchAll(정규표현식) -> array
```
