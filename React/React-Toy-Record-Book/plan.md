# 아이디어

일단 무엇을 구현해야하는지, 어떻게 구현해야 하는지에 대한 정의가 필요하다.

1. 일단 TIL을 진행하면서 하루에 몇시간 공부했는지가 궁금하기 때문에 해당 일에 몇 시간을 했는지에 대한 시간입력이 필요하다.
2. 해당 일에 공부하고 난 후 한줄평을 적을 수 있는 텍스트 입력이 필요하다.
3. 해당 월에 대한 전체 시간이 계산되어야 한다. etc (2022년 2월 총 공부시간 : 30시간)
4. 해당 년에 대한 전체 시간이 계산되어야 한다. etc (2022년 총 공부시간 : 60시간)
5. 각 일에 해당하는 공부 시간이 계산되어야 한다. etc (2022년 2월 3일 총 공부시간 : 2시간)
6. 전체 공부시간 리스트를 개별 클릭 시 상세화면으로 페이지이동이 되며 한줄평 총 공부시간 등을 볼 수 있어야 한다.
7. 글 수정기능이 필요하다. 글 수정 시 공부 시간과 한줄평을 수정할 수 있어야 한다.
8. 글 삭제기능이 필요하다.
9. 전체 입출력은 DB CRUD를 기본으로 한다.
10. 목록 부분에 필터를 만들어서 공부종류 별, 최신-오래된 순을 만들어 원하는 정보만 뽑아낼 수 있도록 한다.

차후 기능은 다 만든 후 업그레이드 하는 식으로 진행한다.

# DB 테이블 설계

Study_list

idx : PK NN INT
date : DATE
comment : TEXT
studyTime : INT
Subject : TEXT

# 디자인

일전에 만들었던 감정일기장을 base로 한다.
[감정일기장-정리](https://github.com/MyoungSeob-Pohang/daily-TIL/tree/main/Code-Review)
[감정일기장-코드](https://github.com/MyoungSeob-Pohang/React-Diary-Upgrade)

최초작성일 - 2022.02.13
버전 - V 1.0
작성자 - 심명섭