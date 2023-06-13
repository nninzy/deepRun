# String

built-in 객체, 모든 환경의 JS에서 사용가능

# 속성: property

문자열의 길이

```
length
```

# 메서드

문자열은 변경 불가능(immutable)한 원시 값입니다.

String 객체의 모든 메소드는 기존의 String을 변형하지 않습니다.

언제나 새로운 문자열이나 다른 타입의 데이터를 반환합니다.

## 문자열 매칭

<br>

- 메서드의 인자가 해당 문자열에 포함되는지 확인

### indexOf

```
String(문자열) -> 첫번째 인데스 | -1
```

### includes

```
String.(문자열) -> boolean
```

### startsWith

```
String.(문자열)
String.(문자열, 시작인덱스)

-> boolean
```

### endsWith

```
String.(문자열) -> boolean
```

<br>

## 인덱스 매칭

<br>

### charAt

```
String.charAt(인덱스) -> string

인덱스에 해당하는 문자 반환

값이 없을시  "" 반환
```

### at

```
String.at(인덱스) -> string | undefined

인덱스에 해당하는 문자 반환


-
음수도 사용가능

    양수: 0 ~ (length - 1)

    음수: (-length) ~ -1

값이 없을시 undefined 반환
```

<br>

## 문자열가공- 자르기

<br>

### subString

```

String.subString(n)
String.subString(n,m)

 -> string

인덱스범위 n 부터 m까지의 문자열을 생성 및 반환

인덱스 n부터시작하는 문자열을 생성 및 반환
```

subString의 인자관리

```
n < m 이어야 정상입력


- 잘못된 입력 관리


m > m: 인수위치 교환

n < 0 or n === NaN : 0으로 취급

n > String.length : str.length 로 치환
```

<br>

### slice

```
subString과 동일, 단 음수 사용 가능

String.slice(n, m)
String.slice(n)

-> string

이떄 n이 음수라면, 뒤에서 n자리를 반환

```

<br>

## 문자열가공- 대소문자 변환

<br>

### toUpperCase

```
대문자로 변환

String.toUpperCase() -> string

```

### toLowerCase

```
소문자로 변환

String.toLowerCase()
```

<br>

## 문자열가공 - 변형문자열 생성

<br>

### trim

공백 제거

```
앞뒤전부 제거

String.trim() -> string
```

### 공백 부분제거

<br>

앞부분 공백 제거

```
trimStart
trimLeft
```

뒷부분 공백 제거

```
trimEnd
trimRight
```

<br>

### repeat

```
반복 문자열 생성

String.repeat(반복횟수) -> string

입력받은 횟수만큼 반복하는 문자열 생성
```

### replace

```
치환

String.replace(내부 문자, 치환될 문자) -> string

1번인자와 같은 값을 갖는 첫번째 값만
2번 인자로 치환
```

### replaceAll

```
replaceAll(내부문자 | RegExp, string) -> string;

1번인자와 같은 값을 갖는 모든 값을
2번인자로 치환
```

### split

```
배열로 변형

String.split(정규표현식이나 문자열)
String.split(정규표현식이나 문자열, 배열의 길이)

-> array

첫번째 인자를 구분으로 문자열을 조각낸다,
조각난 부분을 성분으로 갖는 배열을 반환한다.


* array의 join과 반대의 일을 한다
```

### padStart

```
String.padStart(목표길이, 채워넣을 문자) -> string

기존 문자열의 앞부분에 원하는 길이까지 문자를 채워넣는다
```

### padEnd

```
String.padEnd(목표길이, 채워넣을 문자) -> string

기존 문자열의 뒷부분에 원하는 길이까지 문자를 채워넣는다.
```

### concat

```
문자열0.concat(문자열1, 문자열2) -> string

문자열0문자열1문자열2 를 반환한다

- 성능 및 가독성문제로 + 연산을 더 많이사용한다

```
