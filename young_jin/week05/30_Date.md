# Date

시간정보를 가지고있는 데이터 객체

<br>

## 시계 구현시 Date를 반복 생성하는이유

생성시 설정값 or 현재 시각으로 값이 고정되기 떄문이다.

<br>

모멘텀 코드

```
const setTime = function() {
    const date = new Date();
    const hours = date.getHours().toString().padStart(2,"0");
    const minutes = date.getMinutes().toString().padStart(2, "0");
    const seconds = `seconds: ${date.getSeconds().toString().padStart(2, "0")}`;

    const clock = `${hours}:${minutes}`;
    clockTag.innerHTML = `<abbr title="${seconds}">${clock}</abbr>`;

}

setTime();
setInterval(setTime, 1000);
```

<br>

## 인자로 시간 설정하기

<br>

- 단위별 인자 사용하기

```
// 년 월 일 시 분 초 miliesecond를 순서대로 넣어준다
Date(2023, 6, 8, 24, 0, 0 ,0);
```

- dateString

```
// 가독성 good
// prefix 0 은 시 분 초에만 사용가능하다,

Date('2023/6/8/00:00:00')
```

<br>

## new 사용 / 미사용

<br>

- new 사용시 Date 객체 반환

- 미사용시 문자열을 반환

<br>

## milieSecond를 반환하는 메서드

### Date.now()

1970년 1월 1일 00:00:00(UTC) 부터 지금까지의 milieSeconds를 반환

- 여러 날짜간 간격 계산 가능

- 여러 데이터에 고유키를 부여가능

<br>

### Date.parse(dateString)

1970년 1월 1일 00:00:00(UTC) 부터 dateString 까지의 milieSeconds를 반환

<br>

### set~~ 메서드

getFullYear과 같이 get~~ 형태의 메서드에 대응하는

set~~ 메서드가 존재

객체의 값을 설정 가능

<br>

## 기타: getMonth의 특이함

Date 객체에 입력된 "달" 정보를 숫자로 반환한다.

- 특이하게 0부터 11까지를 사용
