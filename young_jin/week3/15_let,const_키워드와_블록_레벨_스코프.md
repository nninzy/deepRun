# 15.1 var 키워드로 선언한 변수의 문제점

- 1. 변수 중복 선언 허용

  ```
  var existName = "already";

  var existName = "exists";
  ```

        - 신규프로젝트에 새로투입된 인원이 이미 존재하는 변수명을 사용시 모든게 망가진다.

- 2. 함수레벨 스코프
  - 함수 블록만 빼고, 다른 블록내부에서 전부 전역변수가 된다.
  - 변수 호이스팅, 다른 프로그래밍 언어와 배치되는 부분이므로, 호이스팅이 되더라도 자제합시다.

# 15.2 let 키워드, const 키워드.

변수 중복선언 비허용

```
let hello = 7;
let hello = 9;

//오류 발생 const도 마찬가지

```

어떤식으로 사용할지 확실치 않은 변수는 일단 const로 선언합니다.

추후에 필요하다면 let으로 바꿔줍니다.

let과 const 자체에도 의도가 들어갑니다.

남의 코드를 읽을때 선언자로 변수의 용도를 예측 할 수 있어야합니다.
