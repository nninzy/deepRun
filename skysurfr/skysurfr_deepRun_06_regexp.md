# Regular Expression 정규 표현식

> A regular expression is a sequence of characters that specifies a **match pattern in text**.
> Usually such patterns are used by string-searching algorithms for "*find*" or "*find and replace*" operations on `strings`, or for `input validation`.

## JS에서 사용하기

정규 표현식 리터럴, `test()`

``` /* JavaScript */
const lyrics = "We are the Champions.";
const regExp = /the/i;
regExp.test(lyrics); // true
```

RegExp 생성자 함수

``` /* JavaScript */
const lyrics2 = "We will, We will rock you.";
const regExp2 = new RegExp(/you/i);
regExp2.test(lyrics2); // true
```

### RegExp 메서드

`exec()` : [0] 배열로 첫번째 일치 위치 index 반환, 실패하면 null 반환

``` /* JavaScript */
const lyrics3 = "Is this the real life? Is this just fantasy?";
const regExp3 = /this/i;
regExp3.exec(lyrics3); // regex1:1 ['this', index: 3, input: 'Is this the real life? Is this just fantasy?', groups: undefined]
```

`match()` : 찾은 모든 문자열을 배열로 반환

``` /* JavaScript */
const lyrics4 = "Why can't we give love, give love, give love, give love, give love, give love, give love, give love, give love?";
const regExp4 = /give/g;
regExp4.match(lyrics4); // (9) ['give', 'give', 'give', 'give', 'give', 'give', 'give', 'give', 'give']
```

### 플래그 : 검색 설정

안쓰면, 대소문자 구별 한번만 검색
조합해서 사용도 가능

- `i` ignore case
- `g` global
- `m` multi line

특수 패턴들

`.` 점 하나당 한 자리 (예: `...` 세자리)
`{m,n}` 최소 m번 최대 n번 반복하는 문자열 (예: `{2,3}` AA AAA)
`{m,}` 최소 m번 반복하는 문자열
`+` 앞에 있는 패턴이 최소한 한번 이상 반복 `{1,}`과 동일
`?` 앞에 있는 패턴이 최대 한번(0번 포함) 반복 `{0,1}`과 동일
`|` or
`[AB]` [ ] 내에 각 문자 or
`[AB]+` [ ] 내의 내용이 한번 이상 반복
`[A-B]` 범위지정 (예: `[가-힣]` 한글전체)
`\d` 숫자
`\D` not 숫자
`\w` `[A-Za-z0-0_]`
`\W` not `[A-Za-z0-0_]`
`[^A]` 꺾쇠 안 `^`는 not
`^A` 꺾쇠 아닌 `^`는 문자열의 시작 위치
`$A` 문자열의 마지막
`\s` 스페이스(공백)문자 [\t\r\n\v\f] 와 같음

## 정규식 왜 개꿀인가

- 필터링 만능 치트키
- 크롤링 할 떄
- 입력 폼에서 보안체크 할 때
- 남의 코드에 정규식 있을때 응용