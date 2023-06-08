# new Date()

- 인수 없이 생성자로 쓰면 표준 날짜 정보 리턴
- 인수에 숫자(밀리초) 넣으면 1970년 01월 01일 00시 00분 기준 경과 시간 리턴
- 인수에 스크립트가 해석 가능한 문자가 들어가면 그걸 해석해서 표준 날짜 형식으로 리턴
  - 어떻게든 해석해주려 한다

## 메서드

- Date.now() 1970년 01월 01일 00시 00분 기준 현재시간 밀리초로 리턴
- Date.parse() 인수를 텍스트로 받아 1970년 01월 01일 00시 00분 기준 밀리초 리턴
- Date.UTC() UTC 시간 기준으로 각 인수 숫자로 전달 받아 밀리초로 리턴
- Data.getFullYear(), getMonth()... 등등등 : 시간 갯 - 정수로 리턴
- Date.setFullYear(), setMonth()... 등등등 : 시간 셋 - 메서드 앞에 그거 시간 설정해줌
  - getDay는 일주일(0~6까지), get/setDate는 날짜
- toDateString() 문자열 날짜, toTimeString() 문자열 시간 반환
- toISOString() 가독성있는 표준 형식으로 반환, toLocalString("ko-KR") 인수에 그 지역 형식의 시간 문자열 반환
