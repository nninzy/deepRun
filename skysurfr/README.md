# 개발장이대피소의 '모딥다' 스터디 github

## git cui 로 진행하는 fork 후 협업하는 과정

상태 확인 (외부 연결 상태 확인)

> git remote -v

땡겨 받기 (origin이 아니라 upsteam에서)

> git fetch upstream

합치기 (이것도 외부것)

> git merge upsteam/main

컨플릭 잘 해치고 커밋, 푸시하고, 깃헙에서 PR

PR할때 이슈번호 (#5 같은거...) 달아주기

## 내 명명규칙

### 파일 형식

`(deepRun_주차_발표날짜_주제키워드.md)`

> skysurfr/deepRun_02_230516_naming.md

### PR 제목 (Issues에 있는 해당 주차 계획 부분과 연결)

`[XX주차] 닉네임 - ch 해당범위 대표주제`

> 제목 : [02주차] skysurfr - ch 9~12 네이밍

## 모딥다 스터디 규칙

[https://github.com/nninzy/deepRun/issues/2] 참고용 요약

### Repository Fork (완료)

<details>

1. github 상단 오른쪽 fork 클릭 후 본인 github 내 fork된 repository에서 기록 진행
2. fork된 repository 내에서 Sync fork해서 업데이트 진행 필요
3. fork된 repository 내에서는 main branch로 이름이 시작되며 이때는 굳이 branch를 파실 필요 없습니다. 개인 공간이니까요.
4. 기록은 **본인 폴더 내에서 자유롭게** 진행합니다. 타인을 위해서가 아니라 본인 스스로 기록하는 습관을 위해서 정리해주신다고 생각해주세요.
</details>

### 개인 파일 관리

폴더 이름 개인 닉네임 사용, 폴더 내에서 주차 구분 가도록

### Pull Request

> 제목 형식 : [XX주차] (본인 닉네임) - (PR 주제)<br>
> 내용 : 이번 PR에 대한 _간략한 설명_ (무엇을 기록했는지) / 이번 주차 책을 읽으면서 _느낀점_

한주간 기록한 내용을 스터디 회의 전 까지<br>
**PR은 이전 PR이 merge가 진행되지 않는 이상 추가 생성할 수 없으며 push한 내용은 이전 PR에 들어가게 되니 주의**<br>
**PR할 때 반.드.시. Issues에 있는 해당 주차 계획 부분과 연결)**

## Issue 관련

### Issues

게시판 기능, 주차별 계획, 공지사항, 자유로운 이슈 가능<br>
_주차별 계획 이슈에는 본인 PR 연동_<br>
주차별 이슈 내 **댓글로 이번 주차와 관련해서 팀원들과 이야기하고 싶은 내용들, 주제들** 언급

### Wiki

스터디 미팅 이후 나온 내용들이나 레퍼런스 등에 대한 기록이 진행될 예정, 그 외에 본인이 팀원들과 공유하고 싶은 내용들, 문서들
