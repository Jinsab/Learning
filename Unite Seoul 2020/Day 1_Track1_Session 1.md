# Unite Seoul 2020 Day 1 - Track1
## Unity Game Simulation, 게임의 본질에 집중하게 하다

## 목차

- 사용 배경
- 활용 목표
- 시작 전 알아두면 좋은 내용
- 개발 과정
- 결과 1 - 오류 탐지
- 결과 2 - 실패율 예측
- 후기
- 로드맵

## Unity Game Simulation 사용 배경

- 개발팀: 일주일에 20개 맵 업데이트
  - 월: 맵제작
  - 화: 테스트
  - 수: 선정 & 배치
  - 목: 밸런싱
  - 금: 밸런싱
- QA팀 : 새로운 퍼즐 기능 추가, 중요한 기능에서의 버그를 수정
  - 지금까지 라이브 된 모든 맵 (현재 800개)를 모두 테스트
-> 누군가 대신해준다면..

## 활용 목표

## 시작 전 알아두면 좋은 내용

### 모바일
- 시스템 제어 및 결과 이력 관리를 위한 툴 개발 필요
- 공간과 기기 관리 필요

### Unity Editor or 에뮬레이터
- 시스템 제어 및 결과 이력 관리를 위한 툴 개발 필요
- 공간과 기기 관리 필요
- 유니티 라이센스 필요

### 유니티 시뮬레이션(Unity Game Simulation)
- 시뮬레이션 시스템을 위한 개발보다는 오토플레이 머신과 분석을 위한 개발에 집중

![screenshot001](https://github.com/Jinsab/Learning/blob/main/Image/Day1%20Track1%20001.PNG "Track1 001 Screenshot")

- 선데이토즈 애니팡

남색: 오토플레이 머신 실패율
노란색: 유저 데이터, 플레이어 실패율

## 개요

유니티 시뮬레이션은 리눅스 빌드

- 시뮬레이션 횟수는 파라미터들의 조합만큼 실행
  - Input Parameter: int, float, string
  - Output Parameter: int
- 시뮬레이션에 실패하는 주요 원인
  - 타임아웃(60min): 한 인스턴스 당 최대 60분
  - 유니티 코드 오류(Exception)
  - 로직 오류(UX): 서버에 접근하는 코드가 존재할 시 각 시뮬레이션의 인스턴스마다 에셋번들을 다운받으려 하여 서버 공격으로 다가올 수 있음
- 실패할 경우 결괏값은 없으므로 주의
- 1번의 시뮬레이션 당 약 3min이 추가
  - 시뮬레이션 빌드 업로드, 결과 전송에 사용되는 시간
- Player Logs, Raw Result, Aggregate Result

## Unity Game Simulation 설정과 빌드

[1](https://unity.com/kr/products/unity-simulation)
[2](https://unity.com/kr/products/game-simulation)
[3](https://blogs.unity3d.com/kr/2020/03/24/optimize-your-game-balance-with-unity-game-simulation/)

시뮬레이션에 사용할 입력값 설정, 유니티 에디터에서 빌드와 업로드가 동시에 가능

## Unity Game Simulation Dashboard

시뮬레이션 정보 조회, 시뮬레이션 설정 및 결괏값

결괏값 분석을 위해서는 분석 툴 개발 필수

- 결괏값은 텍스트 데이터
- 시뮬레이션 결과 대시보드
  - 스테이지별 실패율 그래프
  - 재배치 발생 빈도 그래프
  - 시뮬레이션 시간 그래프
  - 에러 그래프
  - 실패율 예측 그래프

## 시뮬레이션 결과 분석

### 1. 오류 탐지

- 스테이지별 오류 발생 그래프
- 오류 발생 시 수집한 로그
- 오류가 발생한 상황

### 2. 실패율 예측

![screenshot002](https://github.com/Jinsab/Learning/blob/main/Image/Day1%20Track1%20002.PNG "Track1 002 Screenshot")

- 시뮬레이션 라이브 데이터 비교 그래프
- 입력값에 따른 시뮬레이션의 결과

### 3. 스테이지별 유저 경험 예측

![screenshot003](https://github.com/Jinsab/Learning/blob/main/Image/Day1%20Track1%20003.PNG "Track1 003 Screenshot")

- 아이템 사용에 따른 실패율은 사용할수록 줄어든다.

## Unity Game Simulation 후기

### 게임을 더 재미있게 만드는 데 집중

- 시뮬레이션을 위한 인프라 개발보다는 시뮬레이션 기능에 집중
- 퍼즐 게임의 레벨을 제작하는 데 소요되는 시간을 효율적으로 사용
- 충분한 사용 경험

## Unity Game Simulation 로드맵

### 머신 러닝

![screenshot004](https://github.com/Jinsab/Learning/blob/main/Image/Day1%20Track1%20004.PNG "Track1 004 Screenshot")

### 테스트 머신

- 단순하지만 반복적인 UX 테스트
  - 로그인 테스트
  - 아이템 구매 테스트
  - 메시지 함 확인 테스트
  - 이벤트 활성/비활성 테스트

## 라이센스

__모든 저작권은 Unite Seoul 2020 - 유니티 코리아에 있습니다.__
__
