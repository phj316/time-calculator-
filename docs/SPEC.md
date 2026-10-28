# SPEC.md — 막차연산알림 (Last Train Backward Calculator)

## 1. 개요 (Overview)
* **목적**: 현재 술자리 위치에서 집까지 귀가하기 위해 **"최대 몇 시 몇 분에 술집에서 일어나야 하는지(Notice Leave Time)"**를 거꾸로 역산하여 계산해 주는 귀가 도우미.
* **핵심 가치**: ODsay API 등의 경로 검색 결과(소요시간)를 바탕으로, 구간별 시간표 및 환승 도보 시간, 중간 종착열차(예: 구파발행) 제외 여부를 반영하여 **진짜 막차 타임라인을 거꾸로 계산**함.
* **MVP 범위**:
  1. **대중교통 범위**: 지하철 노선 전용 (버스는 추후 확장)
  2. **개인 계산**: 현재 위치 → 집 (도보 시간 + 여유 시간 포함)
  3. **그룹 공유**: 동일 술자리 내 사용자들의 "일어날 시각"을 오름차순으로 정렬하여 공유

---

## 2. 시간 및 단위 표준화 규칙 (Time Conventions)
* **시간 표현**: 자정을 넘어서는 시각(예: 00:30)은 날짜 계산 오류를 방지하기 위해 **"운행일 기준 누적 분(Minutes from Midnight/Base Day)"**으로 변환하여 저장 및 연산함.
  * 예: `23:12` -> $23 \times 60 + 12 = 1392$분
  * 예: `00:30` (다음 날) -> $24 \times 60 + 30 = 1470$분
* **요일 구분 (Calendar Type)**: 평일, 토요일, 공휴일/일요일 시간표가 상이하므로 해당 날짜의 시간표 구분을 필수 입력 파라미터로 받음.

---

## 3. 핵심 아키텍처 및 입력/출력 (I/O)

### 3.1. 입력 데이터 (Input)
1. **출발지 정보**: 현재 술자리 위치 좌표/주소 및 **출발역까지의 도보 시간** ($T_{walk\_origin}$, 기본값: 7분)
2. **사용자 환경설정**: **여유 시간** ($T_{buffer}$, 기본값: 5분)
3. **목적지 정보**: 사용자별 집 근처 도착역
4. **운행 정보**: 요일 구분 (`WEEKDAY`, `HOLIDAY`)

### 3.2. 출력 데이터 (Output)
```json
{
  "userId": "string",
  "userName": "string",
  "noticeLeaveTime": 1375, // 일어날 시각 (분 단위) -> Display: "22:55"
  "status": "OK | SOON | NOW | MISSED", // 상태
  "selectedRoute": {
    "originStation": "홍대입구",
    "destinationStation": "대화",
    "totalLegs": [
      {
        "line": "L2",
        "departureStation": "홍대입구",
        "arrivalStation": "을지로3가",
        "departureTime": "23:08",
        "arrivalTime": "23:35",
        "headsign": "성수방면"
      },
      {
        "line": "L3",
        "departureStation": "을지로3가",
        "arrivalStation": "대화",
        "departureTime": "23:40", // 환승도보 5분 반영
        "arrivalTime": "00:34",
        "headsign": "대화행"
      }
    ]
  },
  "excludedTrains": [
    {
      "line": "L3",
      "departureTime": "23:45",
      "headsign": "구파발행",
      "reason": "목적지(대화역) 미도달로 제외"
    }
  ],
  "alternativeRoutes": []
}