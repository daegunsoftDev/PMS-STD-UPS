# LiB-UPS PMS 명세서

> **문서 우선순위 (2026-11-16, 사용자 결정):** 이 명세서가 가장 위다. 코드 문서 중에서는 `UPSstd\PMS_STD.md`가 항상 우선하고, 그다음이 나머지 UPSstd md다.

| 항목 | 내용 |
|---|---|
| 판 | **v2.56 (초안)** |
| 대상 코드 | `C:\gil\UPS\UPSstd\PmsStd.sln` |
| 작성 | 에너지기술부 길범석 |

## 변경 이력

| 판 | 날짜 | 바뀐 요구사항 | 내용 |
|---|---|---|---|
| v0.1 | 2026-11-05 | 전체 | 초안. 인터페이스 제안서(2026-11-04) 내용을 옮김 |
| v0.2 | 2026-11-05 | CF-UPS-02·03, SQ-01·02·03, WR-SQ-01, O-06~09 | 설정 보존과 복구, 사고·고장 단계 판정, 복전 후 자동 운전, Fault 확인 명령 추가 |
| v0.3 | 2026-11-05 | RD-UPS-01·03, RD-BAT-01·02 | 운전 모드를 새 칸 대신 Status b11~b17 비트로 넣음(사용자 결정). RACK·BAT Status b5·b6에 기존 판정 포함 |
| v0.4 | 2026-11-05 | RD-BAT-01·02 | 랙 알람 워드를 모두 칸에 담음(Warning3 System Alarm, Fault7·8 Error Code 1·2). Status b5·b6은 칸만으로 판정 |
| v0.5 | 2026-11-05 | SQ-01, O-10 | 단계 판정 출력(대표 단계, 단계 목록, 원인 비트, 확인 대기, 통신장애) 정의. EMS 표시 칸은 미결 |
| v0.6 | 2026-11-05 | CF-UPS-02·03, SQ-03, O-11·12 | 설정 보존·복구를 H50~H56에 구현. H10·H57~H64는 스케일 확인 후(O-11). 켜짐 상태 복구는 4단계로 옮김 |
| v0.7 | 2026-11-05 | WR-UPS-01, WR-SQ-01, CF-UPS-03, O-13 | UPS 명령 1~9 구현(command/ups 구독 추가). Fault 확인 = 명령 10(잠정). 온도보상 켜짐 상태 복구 구현 |
| v0.8 | 2026-11-05 | WR-UPS-01, WR-SQ-01 | CommandType을 번호에서 비트로 바꿈(사용자 결정). command는 DeviceIndex·CommandType·Value 세 칸. Fault 확인 = b9 |
| v0.9 | 2026-11-05 | WR-BAT-01 | BAT CommandType도 비트로 (UPS와 같은 방식). 코드 반영은 확인 후 |
| v1.0 | 2026-11-05 | WR-UPS-01, WR-BAT-01, WR-SQ-01 | 비트 번호를 b1부터 (b0 비움). UPS b1~b9 명령, b10 Fault 확인. BAT b1~b6 |
| v1.1 | 2026-11-05 | DV-10~15 | 보도참고자료 기준 앞으로 추가할 장치 읽기(예정). 최대 범위는 기록만 |
| v1.2 | 2026-11-05 | DV-16 | 공조기 읽기 추가(예정) |
| v1.3 | 2026-11-05 | PF-01~04, TM-01, SQ-04, TS-01·02, DP-01 | 연구개발계획서 3차년도 과제를 요구사항으로 옮김(예정) |
| v1.4 | 2026-11-09 | DV-10·11·13·14 | 맵이 없어 Helios 기종으로 드라이버를 먼저 넣음(통신 꺼둠). 보호 판정 등록은 하지 않음 |
| v1.5 | 2026-11-09 | DV-10·11 | 온습도계는 RTU로 제어기에 직접 (EL6022 채널, 장치마다 채널 하나). IMD는 TCP |
| v1.6 | 2026-11-09 | RD-UPS-01·02, SQ-01 | UPS Warning4·Fault4 없앰(사용자 결정). STATUS 워드를 3번 칸 빈 반쪽에 넣음: Warning3 하위 = I51, Fault3 상위 = I50 OR I51. UPS 레코드 204 → 196 B |
| v1.7 | 2026-11-09 | DV-11 | IMD 기종을 LS IMD1000으로 (사용자 결정). MESSIS `FB_LsIMD1000` 기반 `FB_LsImd1000` 추가, `GVL_SITE.g_fbImd1` 교체. ISO685 드라이버는 남겨 둠 |
| v1.8 | 2026-11-09 | DV-11, RD-IMD-01 | LS 맵(`(MM-20210721) IMD1000 Modbus-Setting_v1_0_0.pdf`)으로 주소 확인: 절연저항은 IR 224~225 (MESSIS는 223~224로 한 칸 어긋남). 장비는 RS485 전용 → 게이트웨이 경유 TCP (사용자 결정). 인터페이스 `22. IMD.xlsx`대로 EMS 보고(토픽 `imd`) 추가, Status 배치는 사용자 결정 |
| v1.9 | 2026-11-09 | DV-10 | 온습도계 시험 장비 ITGuard IE-SX (TCP, 192.168.0.70, PC에서 읽어 확인 24.18 ℃·35.45 %). `FB_ItGuardIeSx` 추가, 토픽 `temperature_sensor`(인터페이스 07)에 실제 값 보고. location 2(BAT실) 잠정. RADIONODE는 다음 |
| v2.0 | 2026-11-10 | SQ-01·02, WR-SQ-01 | 단계 판정 결과를 이벤트 로그(pms_log → EMS)에 남김 (이벤트 클래스 PmsUps). 확인 전 Fault를 PERSISTENT에 둬 PMS 재시작 후에도 유지. 통신이 끊긴 장비의 Fault는 확인 명령으로 내리지 않음 |
| v2.1 | 2026-11-10 | RD-BAT-03 | BAT 질의 답변(2026-11-10) 반영 시작: MODULE 레코드 맨 끝에 ModuleVoltage 추가 (104 → 108 B). 나머지 답변은 7장 미결 표에 기록 |
| v2.2 | 2026-11-10 | DV-03 | BAT 질의 Q5 답변 반영: 8랙·9모듈·14셀 (GVL_PARAM, 셀은 14개까지만 읽음). RACK 레코드 30 → 24, MODULE 300 → 216. module 토픽 주기 대기 60 s·충방전 1 s (Q6) |
| v2.3 | 2026-11-10 | WR-BAT-01 | BAT 질의 Q3·Q1 반영: 투입 명령 = 40013 → 40014 → 40015 (FC06 이어 쓰기, 앞이 성공해야 다음), 개방 = 40013만, 비상정지는 이어 쓰기보다 우선. BMS 시각 동기화 HR 40005~40007 1시간마다 (통신 유효해지면 먼저 한 번). 실제 쓰기는 GVL_PARAM.UPS_CONTROL_ENABLED(지금 FALSE)를 켜야 함 |
| v2.4 | 2026-11-10 | SQ-01, WR-SQ-01 | 사용자 결정: 사고·고장 단계는 **EMS가 받은 비트로 직접 계산·표시**. 인터페이스 칸 추가 없음. PMS는 판정표만 제공하고, PMS 안 계산은 비교용으로 남기되 기록(pms_log)은 끔 (`GVL_PARAM.UPS_SAFETY_LOG_ENABLED`). Fault 확인은 EMS 안에서 처리 |
| v2.5 | 2026-11-10 | WR-BAT-01 | EMS 협의: BAT 명령 번호 방식 확정 (1·2 시스템 일괄, 3 Estop, 4 초기화, 11·12 랙 컨택터 Value = 랙 번호). 랙 개별 제어 `FB_BmsRackCommand` 추가 (BAT 질의 Q4) |
| v2.6 | 2026-11-10 | DV-17 (신규) | AC 회로 누전 감시: 운영 WYGF-D08/12/16NC 지락검출계전기, EL6022 RTU 직결, PMS 안에서만 읽음 (EMS 보고 없음). `FB_WoonyoungGfr`, `GVL_SITE.g_fbGfr1` |
| v2.7 | 2026-11-10 | DV-18 (신규) | 연기 감지: 가스디엔에이 TS-60 열연복합감지기의 연기 접점만 벡호프 DI 1에 연결 (온도 접점은 안 씀). 알람만 (조치 없음). `GVL_SITE.g_fbIo1`·`g_fbSmoke1`, 보호 칸 6 |
| v2.8 | 2026-11-10 | DV-19 (신규) | 연기·온도 감지: 가스디엔에이 ASD-57 열연복합 감지기를 EL6022 RTU(9600 고정)로 읽음. 연기 알람은 보호 칸 7에 알람만. `FB_GasdnaAsd57`, `GVL_SITE.g_fbAsd57`·`g_fbSmoke2` |
| v2.9 | 2026-11-10 | RD-FIRE-01 (신규) | 화재감지기 EMS 보고: 토픽 `fire_detector` (이름 잠정, EMS 확인 필요). 인터페이스 20. OffGas 양식 그대로 (DeviceIndex + Status b0 통신 이상·b1 알람·b2 고장, 8 B). 0 = TS-60 연기 접점, 1 = ASD-57 (연기 또는 온도 알람). 사용자 결정 |
| v2.10 | 2026-11-11 | RD (module 주기) | module 토픽 발행 주기를 대기 60 s·충방전 1 s에서 **무조건 1 s**로 바꿈 (모든 토픽 1 s). 사용자 결정 |
| v2.11 | 2026-11-12 | RD-UPS-01·03 | 인터페이스 `08.UPS.xlsx` 반영: Status를 5비트로 줄이고 상태 칸 9개(SystemOperationMode, OperationMode, UPS_ModeStatus, MCCB_Status, RectStatus, BBCStatus, InverterStatus, BypassStatus) 추가. UPS 레코드 196 → 228 B. 운전모드 2칸은 출처 미정이라 0 |
| v2.12 | 2026-11-12 | WR-CMD-01 (신규), WR-UPS-01 | 인터페이스 `100.Command.xlsx` 반영: command 레코드 앞에 SequenceNumber(중복 방지), 끝 Delay(ms, 기다렸다가 실행) → 16 → 20 B. UPS CommandType을 비트 b1~b10에서 **번호 1~26**(H100~H125)으로. Fault 확인 b10 없앰. 사용자 결정 |
| v2.13 | 2026-11-13 | WR-BAT-01, RD-BAT-01·02 | BMS 맵 v1.4 반영: 컨택터 상태 Input 30026(충전 b0·방전 b1)·30027(랙 b0~b9) 읽기(읽기 전용 확정). 1 닫기 / 2 열기 = 40013~40015 FC16 한 번에 [1,1,1] / [0,0,0] (순서 무관, 사용자 결정 2안). 5~10 컨택터 하나씩 제어 추가. 11·12는 실제 랙 상태(30027) 바탕. BAT Status b11·b12, RACK Status b10 컨택터 상태. 코어 컨택터 피드백(bContactorClosed) 실제 상태로 |
| v2.14 | 2026-11-16 | CF-UPS-01·02, O-11·14 | 간헐충전 H57~H61을 UPS에 씀. H58·H59는 H57이 1이면 전압(0.1 V), 2이면 SOC를 같은 레지스터에 씀 (세방 맵 Param 설명 칸 "SOC : 70 %"). EMS 4칸(StartVoltage·EndVoltage·StartSoc·EndSoc)은 그대로 두고 PLC가 H57로 골라 씀 (사용자 결정 1안). 전압 0.1 V는 UPS 보드 값으로 확인(H58 5100 = 510.0 V). SOC 단위는 미결 O-14. 문서 우선순위·대상 코드 경로 갱신 |
| v2.14a | 2026-11-16 | WR-UPS-01, O-06 | UPS 보드 시험: Auto ON(13, H112)이 정류부·BBC·인버터 일괄 기동임을 확인. 코드 변경 없음 |
| v2.14b | 2026-11-16 | CF-PRT-01 | config/protection 안 씀 (보류). 배터리 온도는 BMS Level → SQ-01, 배터리실 온·습도는 EMS가 temperature_sensor로 직접 판정. 코드 변경 없음 (echo만) |
| v2.15 | 2026-11-17 | WR-UPS-01, CF-UPS-03 | 고장·e-stop 재검사: 명령·설정 쓰기가 드라이버를 기다리는 동안에도 매 스캔 다시 보고, 생기면 시작 명령·설정 쓰기를 취소하거나 기다림. 드라이버(FB_SebangUps)도 고장·e-stop 입력으로 대기열의 시작 명령·설정 쓰기를 뺌. 외부 검토(GPT) 지적 2건 |
| v2.16 | 2026-11-17 | SQ-05 (신규), 4-1 머리말 | 사고·고장 대응 동작(C안, 2026-11-11 결정)을 명세로: 원인별 UPS·BMS 명령, 3초 확인·최대 3번 재시도, 전체 스위치 기본 OFF. 실패 알람의 EMS 칸은 이번에 넣지 않음. Level 3은 하나로 봄(3.1 따로 안 봄). E-stop 명령은 쓰지 않음 |
| v2.17 | 2026-11-18 | RD-BAT-03 | 인터페이스 `10. Module.xlsx`(2026-11-17) 반영: 인덱스 3칸 뒤 ModuleVolatge1~3(철자 원본대로), 그 뒤 온도·셀 전압. 레코드 104 → 116 B. ModuleVolatge1 = 랙 상세 D+15+m ×0.01 V, 2·3은 맵에 없어 0. 예전 "맨 끝 ModuleVoltage 108 B" 안은 폐기 (2026-11-17 작업, 11-18 검증) |
| v2.17a | 2026-11-18 | SQ-05, O-15 | 미결 O-15 추가: 입력 정전(I44)도 원인 b4로 묶여 충전 컨택터가 열리고, 복전 뒤 다시 닫는 주체가 없음. 해결 방안은 나중에 정함 (사용자 결정). 코드 변경 없음 |
| v2.18 | 2026-11-18 | SQ-05 | ESD(b12) 대응을 화재·Level 3과 나눔: 바이패스 절체를 빼고 인버터 정지(H105)를 추가 (바이패스로 부하를 다시 살리면 비상정지를 무효로 만듦). ESD 중 UPS 명령은 앞 명령 결과를 기다리지 않고 드라이버가 받는 대로 연달아 보냄 (인버터 → BBC → 정류부). 상태 확인은 유지. 주 대응은 UPS가 접점으로 하고 PMS는 백업 (사용자 결정) |
| v2.19 | 2026-11-18 | SQ-05 | 재시도를 "3초 확인·최대 3번 → 실패로 멈춤"에서 "될 때까지 2초마다 계속 보냄, 10초 넘게 안 되면 오류 표시, 그 뒤에도 계속"으로 바꿈 (데스틴 PCS 통신 방식: 응답 대기 1초 + 쉼 1초, 10초 지속 시 이상 표시, 표시 뒤에도 계속). 드라이버가 못 받은 명령은 기다리지 않고 다음 스캔(10 ms)에 다시. 순서가 있는 단계는 앞 동작이 되거나 오류가 켜지면 다음 동작으로 (사용자 결정) |
| v2.19a | 2026-11-18 | SQ-05 | 대응 오류(10초)를 EMS로 보내는 칸은 두지 않음. EMS가 단계·상태 비트로 직접 판단 (사용자 결정, 코드 변경 없음) |
| v2.20 | 2026-11-18 | RD-UPS-01·02, SQ-01 | UPS Warning·Fault 칸을 회사 eventMap(`인터페이스\001.UPS 경고장비트맵.xlsx`) 순서로 바꿈: 경고장 → Warning1 b0~29, 중고장 I44~I46 → Fault1 b0~29, I47~I51 → Fault2 b0~18. Warning2·3, Fault3은 0. 맵 비트 자리 그대로(v1.6)는 없앰. 단계 판정(SQ-01)이 읽는 비트도 같이 바꿈 (사용자 결정) |
| v2.21 | 2026-11-18 | SQ-01 | 사고·고장 단계·원인 발생/해소 기록을 다시 켬 (v2.4에서 끈 것). **PMS 이벤트 로그에만** 남기고 EMS(pms_log)로는 보내지 않음. 기록 항목은 v2.0 표 그대로 (원인 알람, 단계 바뀜, 확인 대기, 통신장애, Fault 확인). 대응 동작(SQ-05) 기록은 넣지 않음 (사용자 결정) |
| v2.22 | 2026-11-19 | SQ-01, SQ-05 | 사고·고장 대응 동작(SQ-05)의 명령·완료·10초 오류도 **PMS 이벤트 로그에만** 남김 (PMS가 UPS·BMS에 잘 썼는지 확인용). 처음 보낸 명령 1번, 완료 1번, 오류는 알람(되면 해소). 2초마다 다시 보내는 것은 남기지 않음. EMS로는 안 보냄 (사용자 결정, v2.21의 "대응 동작 기록 안 함"을 바꿈) |
| v2.23 | 2026-11-19 | O-15 | 입력 전원 이상(I44 10개)을 경보만 하는 안 → 사용자 지시로 되돌림 (커밋 4fb24d7). **적용 안 함** |
| v2.24·v2.25 | 2026-11-19 | SQ-05 | 2단계 BMS Minor로 연 충전 컨택터를 풀리고 30초 뒤 PMS가 다시 닫는 안 (v2.25에서 이상 흐름 연동) → 사용자 지시로 되돌림. **적용 안 함** |
| v2.26 | 2026-11-19 | SQ-05, O-15 | 결정: 2단계(BMS Minor)·3단계(BMS Major, 입력·정류부 중고장 포함) 모두 **복귀는 사람**. PMS는 자동으로 다시 닫거나 켜지 않음. 코드는 v2.22 그대로 (사용자 결정) |
| v2.27 | 2026-11-20 | SQ-01 | 통신장애 기록 문구에서 '짝'을 뺌: "UPS n 통신장애" / "BMS n 통신장애" - 끊긴 동안 사고·고장 판정은 마지막 값 유지 (사용자 요청, 이벤트 문구만 바꿈) |
| v2.28 | 2026-11-20 | RD-BAT-01·02 | 배터리 eventMap(`인터페이스\002.이솔루션 초안.xlsx`, BAT·RACK 시트) 배치로 바꿈: 69비트를 b0부터 빈칸 없이 (Warning1, Fault1~3). Module Status·Error Code 1·2·Self Test는 뺌 (사용자 결정). BAT Status b13 = 랙 컨택터 닫힘 (30027 ≠ 0, EMS 단선도 DC 컨택터용, 사용자 결정). 레코드 크기는 그대로 |
| v2.29 | 2026-11-20 | SQ-01, SQ-05, O-09 | RCD 1·2단계(I51 b12·b13)는 7쪽 표의 배터리 DC 전로 지락 1·2단계가 **아님** (2026-11-11 확인). 대응이 같아(BBC 정지) RCD 1단계 = 3단계, RCD 2단계 = 4단계는 그대로 두고 **이름만 RCD로** 바꿈 (원인 b7·b9, 이벤트 로그 문구). 진짜 지락 1·2단계와 DC 전로 단락은 신호 위치를 모름 → 칸만 둠 (사용자 결정) |
| v2.30 | 2026-11-20 | SQ-05, O-17 | 세방 맵 Alarm Flag의 비트별 동작(정류부·배터리컨버터·인버터 칸)도 모두 수행 (7쪽 표는 그중 중요한 것만 모은 것). 단계는 새로 만들지 않음. '인버터 정지 To Bypass' = 바이패스 절체 확인 뒤 인버터 정지(H105). MBS C/B Fail 대응 추가(단계 없음). 맵의 시간 지연: 배터리 과전류 10분, 출력 과전류 15분. '기동 안 됨'(입력 상회전, Pre Magnet Fail)은 명령 없음. 바이패스 이상 때 처리는 O-17로 남김 (사용자 결정) |
| v2.31 | 2026-11-20 | SQ-05, SQ-01, O-17, O-18 | **화재는 ESD처럼**: 바이패스 절체 안 함, 인버터·BBC·정류부 정지와 BMS 전부 열기를 기다리지 않고 연달아 (7쪽 표·맵의 화재 '바이패스 절체'를 바꿈, 사용자 결정). O-17 해결: 바이패스 이상(I49)이면 바이패스 절체를 보내지 않고 인버터도 끄지 않음(부하 유지), 이벤트 로그 알람 BypassBlocked. 바이패스 회로를 여는 통신 명령이 없어 화재·ESD 출력 완전 차단은 O-18 |
| v2.32 | 2026-11-23 | RD-BAT-01·02, O-16 | 배터리 eventMap v1 (`인터페이스\002.이솔루션 v1.xlsx`, 사용자 수정 2026-11-23) 반영: 126비트. System Alarm을 고장 → **경고**로, 뺐던 Module Status·Error Code 1·2·Self Test 1·2를 다시 넣음. 칸 Warning1, Fault1~4. 레코드 크기 그대로 |
| v2.33 | 2026-11-23 | WR-BAT-01 | BAT 명령 3 비상정지를 코어 경로 대신 **바로 쓰는 경로**(5~10과 같음)로: 코어(F_BatCmdAllowed)가 지령원의 EStop을 거절해 40012가 나가지 않던 문제 (시험 PLC에서 발견). 4 초기화도 코어가 거절함 → O-19 |
| v2.34 | 2026-11-23 | WR-BAT-01, O-19 | BAT 명령 4 초기화도 바로 쓰는 경로로 40031 Reset = 1 (원격 초기화 허용, 사용자 결정 O-19 A). 초기화는 BMS 비상차단만 풀고, 컨택터 재투입은 따로 명령 |
| v2.35 | 2026-11-23 | WR-BAT-01 | BMS 시각 동기 구현: v2.3에 정했지만 UPSstd로 옮길 때 빠져 있었음. 40005~40007을 FC16 한 번에, 통신이 붙으면 먼저 한 번, 그 뒤 1시간마다 |
| v2.36 | 2026-11-23 | SQ-05 | ESS 공통 코어 보호 규칙 1~5(PCS 고장·통신, BMS 고장·통신, 접속점 계측)를 **알람만**으로: 규칙 3 'BMS 고장'이 BMS 컨택터를 전부 열던 것(명세에 없던 동작)을 끔. 배터리 차단은 SQ-05만 (사용자 결정) |
| v2.37 | 2026-11-23 | RD-BAT-01·02 | 배터리 eventMap v2 (`인터페이스\002.이솔루션 v2.xlsx`, 사용자 결정): 칸 하나에 **워드 2개**(첫 워드 b0부터, 둘째 워드 b16부터), **각 워드 안에서는 정의된 비트만 이어서**. 시트 하나(BAT A~F, RACK H~M). Module Error b9·Module Status 경고 비트는 워드째로 Fault. 이솔루션 답변(Lock 2 b10~15, Fire b15) 이름 반영 |
| v2.38 | 2026-11-23 | SQ-01, WR-SQ-01 | PMS 안 단계 계산에서 **Fault(4~6)도 원인이 해소되면 바로 내림** (확인 전 유지·재시작 래치 없앰). 확인은 EMS가 하므로(v2.4) PMS에 확인 신호가 오지 않아, 붙잡은 Fault가 PMS 재시작 전까지 안 풀리던 문제. 이벤트 AckPending·FaultAck 안 씀. WR-SQ-01 폐기 (사용자 결정) |
| v2.39 | 2026-11-23 | CF-UPS-02 | 제어기(CX5130) 내장 1초 UPS로 전원 장애를 감지하면 60초를 기다리지 않고 바로 PERSISTENT 저장 (HELIOS와 같은 방식, `Tc2_SUPS` `FB_S_UPS_CX51x0` 상태 확인 모드). 사용자 결정 |
| v2.40 | 2026-11-23 | SQ-05 | 사고·고장 대응 스위치 `g_bUpsSafetyRespEnable` 기본값 FALSE → **TRUE** (사용자 결정: 끄는 것은 사람이 온라인으로). PERSISTENT라 이미 돌던 PLC는 저장된 값 그대로 |
| v2.41 | 2026-11-23 | RD-UPS-01 | PMS는 UPS 운전 모드를 나누지 않음(모드 하나). OperationMode = **1 자동 고정**, SystemOperationMode = 0 고정 (사용자 결정) |
| v2.42 | 2026-11-23 | (범위 밖) | 이 현장에는 PCS가 없음: ESS 공통 코어의 PCS(Plaspo)·계량기는 등록하지 않고(이미 그랬음), 운전 모드 5개(정출력·스케줄·피크컷·통합·스탠다드 스케줄)도 등록하지 않아 고를 수 없게 끔. 코드는 남겨 둠 (사용자 결정) |
| v2.43 | 2026-11-23 | RED-01~05 (신규), O-20 | 이중화 확정 (사용자 결정). 4-2장 추가. RED-01 구현: 설정 동기화에 UPS 설정 보관값·사고 대응 스위치 추가, 형식 번호 2, 받은 값으로 바꾸면 바로 저장. 나머지는 예정 (장비·업체 답 뒤) |
| v2.44 | 2026-11-23 | RED-02, O-20 | Standby는 장비에 접속하지 않음 (Active만 읽고 씀). O-20 없앰 (사용자 결정: 업체에 물을 것 없음) |
| v2.45 | 2026-11-23 | RED-02 | 대기 중인 PLC: 사고·고장 판정 마지막 값을 비움(넘겨받는 순간 옛 값으로 대응 명령 방지), 장비 통신장애 알람(CommLost, 보호 규칙 4) 안 띄움 (사용자 결정) |
| v2.46 | 2026-11-23 | DV-04 (신규) | 읽기 주기: UPS 1초·BMS 1초는 한 바퀴(요청 여러 개 연달아) 기준, 랙 상세 100 ms마다 한 조각 (사용자 결정) |
| v2.47 | 2026-11-23 | P-01~03, RD-UPS-02, RD-BAT-01·02, WR-UPS-01, O-10, DV-05·RD-FIRE-01·RD-TMP-01·RD-MQTT-01·RD-LOG-01·WR-GATE-01 (신규), 5장 | 빠지거나 낡은 곳 정리 (새 결정 없음, 코드·결정 그대로): 원칙을 C안·BAT 명령에 맞춤, eventMap 파일 이름, Auto OFF, O-10 해결, 현장 설정값·화재감지기·온습도·MQTT 공통·이벤트 로그·쓰기 관문 항목, 시험 환경 |
| v2.48 | 2026-11-24 | RD-BAT-01·02 | 배터리 eventMap: 경고 비트 5개(Module Error b9 Cell V Imbalance Warning, Module Status b0·2·4·6 UV·OV·OT·UT Warning)를 Fault에서 빼 **Warning2 b0~4**로 옮김. Fault1 b16~25 = Module Error b0~8·10, Fault2 b16~20 = Module Status b1·3·5·7·8 (사용자 수정, 공유 드라이브 구글 시트 '002.이솔루션' 2026-11-24). 레코드 크기 그대로 |
| v2.49 | 2026-11-25 | RED-01, SQ-05 | 이중화: 사고 대응 '된 동작' 기억을 EAP로 넘김. 넘겨받은 PLC가 사람이 다시 켠 장비를 다시 끄지 않게 (사용자 결정). 형식 번호 2 → 3, 동기화 변수 크기 바뀜 (XAE EAP 배열 크기 같이 바꿈). 판정 값은 계속 넘기지 않음 (v2.45) |
| v2.50 | 2026-11-25 | SQ-01, SQ-05, RD-UPS-01, O-09, O-18 | 세방 회신(2026-11-25) 반영. ① 배터리 DC 전로 지락 1·2단계 = STATUS2 b12·b13 (RCD 1·2단계): v2.29의 '지락 아님'(2026-11-11)을 바꿈, 이름을 '지락(RCD) 1·2단계'로. 단계·대응(3·4단계, BBC 정지)은 그대로 (사용자 결정). ② MCCB_Status b6 = 배터리 MCCB Trip (H11 OP-CODE b3, 1 = Trip). PMS는 MCCB를 Trip하지 않고 상태만 봄 (세방 회신, 사용자 결정). ③ O-18 해결: 바이패스 포함 출력 완전 차단은 UPS 자체 ESD 사양. PMS 백업 정지 명령(v2.18)은 바꾸지 않음. ④ DC Link 단락: 전용 비트 없음, 여러 알람으로 뜸 → 5단계로 볼지는 O-09에 남김 |
| v2.51 | 2026-11-25 | CF-UPS-01·02·03, RED-01, O-12 | 인터페이스 `101.Config Project` UPS_V1(구글 시트)에 2칸 추가: FloatChargeSetVoltage(H7 Batt 부동 충전 전압), ChargeSetCurrentLimit(H10 Batt 충전 전류, 예전 임시 이름 ChargeCurrentLimit). 둘 다 REAL_FIELD, DeviceId 바로 뒤 → config/ups 레코드 148 → 164 B. PMS가 쓰고 보관·복구 (보관값 12 → 14칸). 이중화 동기화 변수 480 → 512 B (남는 자리 둠), 형식 번호 3 → 4. O-12 해결 |
| v2.52 | 2026-11-25 | RED-02, O-20 | **두 PLC 모두 장비를 읽고, 쓰기는 Active만** (PMS_STD 코어 기본으로 돌아감, 사용자 결정). v2.44(대기 쪽은 접속 안 함)와 v2.45(대기 중 판정 비우기·통신장애 알람 끄기)를 없앰. 지금 RS-485 장비는 실제로 안 씀 (사용자 확인). O-20 다시 열림 (장비 TCP 접속 2개) |
| v2.53 | 2026-11-25 | SQ-01, O-09 | DC Link 단락은 따로 판정하지 않음: 세방은 비트만 띄우고 PMS는 뜬 비트마다 정해진 단계·대응을 수행 (사용자 결정). 알람 조합·DC Link 전압으로 5단계를 만들지 않음. 코드 변경 없음 |
| v2.54 | 2026-11-25 | SQ-05 | BMS Level 3(5단계)에 인버터 정지 추가: 7쪽 표 5단계 BMS Fault의 '인버터 정지/바이패스 절체'를 출력·인버터 중고장(v2.30)처럼 바이패스 절체 → 확인되면 인버터 정지(H105)로 수행. 예전에는 바이패스 절체만 해서 인버터가 기동 상태로 남았음 (사용자 지적) |
| v2.55 | 2026-11-25 | WR-CMD-01, WR-UPS-01, RD-UPS-01, RED-01 | (다른 AI 작업을 정리해 기록) 회사 EMS 시험 중 사용자 지시: ① command 레코드 버전 2 = DeviceIndex · CommandType · Value · Delay 16 B (SequenceNumber·중복 거르기 없앰, 헤더 포함 명령 1건 80 B, DeviceIndex 위치 0). 실제 EMS가 이 형식으로 보냄. 인터페이스 `100.Command.xlsx`에는 아직 SequenceNumber 줄이 남아 있음. ② `command/ups` 101 = OperationMode 0(수동), 102 = 1(자동), 시작 기본 1. 보고 칸만 바뀌고 장치 명령·사고 대응은 막지 않음 (v2.41 '1 고정'을 바꿈) |
| v2.56 | 2026-11-26 | RED-03 | 이중화를 켰는데 10초 넘게 역할을 못 정하면(Reading: Net ID 읽기 실패 또는 RedundancyTask 정지) 설정 오류 알람 (회사 EMS 시험 중 Reading에 멈춰 MQTT가 안 붙은 일, 사용자 지시) |

---

## 0. 원칙

| ID | 상태 | 요구사항 |
|---|---|---|
| P-01 | 확정 | 운전 명령(설정값 쓰기 + 시작·정지·충방전 모드)은 UPS에만 한다. |
| P-02 | 확정 | BMS에는 충·방전 운전 명령을 보내지 않는다. BMS에 쓰는 것은 EMS BAT 명령(컨택터·비상정지·초기화, WR-BAT-01), 사고·고장 대응의 컨택터 열기(SQ-05), 시각 동기뿐이다 (v2.47 정리). |
| P-03 | 확정 | 1차 보호(차단·정지)는 BMS·UPS가 스스로 한다. PMS는 사고·고장 단계를 판정하고, 백업 대응으로 UPS·BMS에 직접 명령한다 (C안 2026-11-11, SQ-05). 복귀는 사람이 한다. |
| P-04 | 확정 | 알람 칸에는 장비 맵 워드의 비트를 위치 그대로 담는다. |

## 1. 장치와 통신

| ID | 상태 | 요구사항 |
|---|---|---|
| DV-01 | 확정 | UPS: 세방전기, Modbus TCP, 맵 v3.2 (2026-10-22) |
| DV-02 | 확정 | BMS: 이솔루션, Modbus TCP, 맵 v1.3 (2026-10-23) |
| DV-03 | 확정 | 랙 8개, 랙당 모듈 9개, 모듈당 셀 14개 (BAT 질의 Q5 답변 2026-11-10) |
| DV-04 | 확정 | **읽기 주기 (v2.46, 사용자 결정): UPS 1초, BMS 1초, 랙 상세 100 ms.** UPS는 1초마다 4개 요청(입력 2·설정 2)을 연달아 읽어 전체 값이 1초마다 갱신. BMS는 1초마다 시스템·랙 아날로그·랙 상태 3개를 연달아 읽고, 그 사이 100 ms마다 랙 상세 한 조각(125 레지스터)을 읽음 (8랙 한 바퀴 약 3.2초). 장비 쓰기는 읽기보다 먼저. 예전에는 200 ms마다 요청 하나(UPS·BMS 한 바퀴 0.8초, 랙 상세 25.6초) |

### DV-05 현장 설정값 — 잠정 (v2.47)

사람이 현장에 맞춰 정하는 값입니다. 지금 값은 시험 기준이며 시운전 때 정합니다. 바꾸면 다시 빌드·다운로드합니다 (사고 대응 스위치만 온라인).

| 구분 | 값 | 지금 | 위치 |
|---|---|---|---|
| 태스크 주기 | PlcTask·MqttTask·FastTask·RedundancyTask | 각 10 ms (우선순위 20·30·2·1) | `PmsStd.tsproj` |
| 장비 읽기 | UPS·BMS 한 바퀴 / 랙 상세 한 조각 / 센서 | 1 s / 100 ms / 1 s (DV-04) | `GVL_SITE` 인스턴스 |
| 통신 끊김 판정 | 정상 응답 없음 | 10 s | `FB_DeviceBase.i_tCommTimeout` |
| 장비 주소 | IP·포트·국번, 사이트 번호, 브로커 주소·계정 | 시험값 (사이트 5) | `GVL_SITE` |
| UPS 설정 | 비교·다시 쓰기 주기 | 10 s | `FB_UpsConfigApply.i_tRecheck` |
| UPS 설정 상한 | 방전·펄스 전류 / 주기적 방전 시간 / 펄스 시간·횟수 / 간헐충전 전압·종료시간·재시작 기간 / SOC 단위 | 100 A (시험값) / 60분 / 60초·1000회 / 1000 V·1440분·365일 / 1 % (O-14) | `GVL_PARAM.UPS_CFG_*` |
| 사고 대응 | 대응 켜짐 / 다시 보내기 / 오류 판정 / 과전류 지연 | 켬 / 2 s / 10 s / 출력 15분·배터리 10분 (맵 값) | `GVL_PERSIST`, `FB_UpsSafetyResponse`, `FB_UpsSafetyStage` |
| 저장·기록 | PERSISTENT 주기 저장 / 켜진 뒤 기록 안 하는 시간 | 60 s / 15 s | `GVL_SITE.LOG_SET`, `FB_UpsSafetyLog` |
| MQTT | 발행 주기 / 브로커 끊김 알람 | 1 s / 30 s (Local) | `FB_MqttPublish`, `GVL_PERSIST.g_stBrokerWatch` |
| BMS 시각 동기 | 주기 | 1시간 | `FB_EsolutionBms.i_tTimeSync` |
| 이중화 | 사용·Net ID / EAP·접점 끊김 / 절체 지연 / 복귀 안정 / 넘겨받기 제한 / 쓰기 대기 / 시작 유예 | 끔 / 0.5 s·0.5 s / 0.5 s / 10 s / 3 s / 1 s / 3 s | `GVL_SITE.RED_SET` |
| 장비 쓰기 관문 | `DEVICE_WRITE`, `UPS_CONTROL_ENABLED` | 커밋 값 꺼짐 (WR-GATE-01) | `GVL_SITE`, `GVL_PARAM` |

## 2. 읽기 (PMS → EMS)

### RD-UPS-01 UPS 상태 칸 — 잠정 (v2.11, 인터페이스 `08.UPS.xlsx` 2026-11-12)

레코드 앞부분 10칸(UDINT): DeviceIndex, SystemOperationMode, OperationMode, Status, UPS_ModeStatus, MCCB_Status, RectStatus, BBCStatus, InverterStatus, BypassStatus. 맵 워드: I45 정류부, I46 BBC, I48 인버터, I50 STATUS1, I51 STATUS2.

| 칸 | 비트·값 | 채우는 값 |
|---|---|---|
| SystemOperationMode | enum 0 수동, 1 모드스케줄 | **0 고정** (v2.41: PMS는 모드를 나누지 않음) |
| OperationMode | enum 0 수동, 1 자동 | **UPS 명령 101→0, 102→1** (2026-11-25 사용자 지시). 시작 기본값 1. 장치 명령·사고 대응 허용 조건은 그대로 |
| Status | b0 통신에러 / b1 경고 있음 / b2 고장 있음 / b3 e-stop / b4 충전 중 | 통신 끊김 / Warning1~3 ≠ 0 / Fault1~3 ≠ 0 / I50 b0 / I50 b13 |
| UPS_ModeStatus | b0~b5 부동·균등·대기·주기적 방전·펄스 방전·간헐충전 | I50 BAT Mode 값 0~5를 비트로 나눔 |
| | b6 비상 방전 / b7 온도보상 / b8 ECO / b9 PRLT / b10 Droop / b11 VSM / b12 Regen | I45 b8 / I51 b2 / I48 b7 / I50 b12 / I50 b11 / I45 b15 / I45 b8 |
| MCCB_Status | b0 바이패스 M/C / b1 인버터 M/C / b2 입력 Pre M/C / b3 입력 M/C / b4 MBS C/B (1 = close) / b5 Out C/B (**1 = 개방**) / b6 배터리 MCCB Trip (**1 = Trip**, v2.50) | b0~b5: I50 b1~b6, b6: H11 OP-CODE b3 |
| RectStatus | b0 기동 / b1 정상 / b2 Regen | I45 b11 / b12 / b8 |
| BBCStatus | b0 기동 / b1 정상 | I46 b12 / b13 |
| InverterStatus | b0 기동 / b1 정상 / b2 인버터 출력 | I48 b6 / b14 / b13 |
| BypassStatus | b0 바이패스 출력 | I48 b12 |

- UPS_ModeStatus b6 비상 방전, b12 Regen, RectStatus b2는 모두 같은 맵 비트(I45 b8 Regen mode)입니다 (인터페이스 표 그대로).
- **배터리 MCCB (v2.50, 세방 회신 2026-11-25):** 배터리 MCCB 개폐 상태는 H11 OP-CODE b3 (Batt CB Trip)으로 봅니다. H11은 Read Only라 PMS는 쓰지 않고, UPS가 Trip한 결과만 MCCB_Status b6으로 EMS에 보냅니다. H11은 이미 읽는 설정값 묶음(H0~11)에 들어 있습니다.

**시험 기준**
- I46 b0 = 1 (배터리 저전압) → Warning1 b5 = 1 (0x20), Status b1 = 1, b2 = 0 (v2.20)
- I50 b0 = 1 (긴급정지) → Fault2 b13 = 1 (0x00002000), Status b2 = 1, b3 = 1 (v2.20)
- I50 = 0x3860, I45 b8 = 1, I48 b7 = 1, BAT Mode = 5, 온도보상 = 1 → Status = 0x10, UPS_ModeStatus = 0x17E0, MCCB_Status = 0x30
- (v2.50) 위 상태에 H11 = 0x0008 (Batt CB Trip) → MCCB_Status = 0x70. H11 = 0x0007 (b3 아님) → b6 = 0
- SystemOperationMode는 언제나 0. OperationMode는 시작 1, UPS 명령 101 → 0, 102 → 1 (v2.55)
- 통신 끊김 → Status = 1, 나머지 0

### RD-UPS-02 UPS Warning·Fault 배치 — 확정 (v2.20, 회사 eventMap)

회사 eventMap `인터페이스\001.UPS 경고장비트맵.xlsx`(시트 G2Power_V1)와 같은 자리로 보냅니다. EMS는 이 파일로 비트 이름을 띄웁니다. 공유 드라이브에는 같은 내용을 배터리 eventMap 양식(세방 맵 출처·설명·워드 열 추가)으로 바꾼 구글 시트 '001.UPS 경고장비트맵'이 있습니다 (2026-11-23, 줄 위치는 같음).

- 세방 맵 Alarm Flag의 경고장(노랑)·중고장(주황) 비트만 씁니다. 마스크 밖 비트(운전 상태·예약)는 버립니다.
- 채우는 순서: 맵 워드 순서(I44 → I51), 워드 안에서는 비트 번호 순서로 **b0부터 빈칸 없이** 채웁니다.

| 칸 | 내용 | 비트 |
|---|---|---|
| Warning1 | 경고장 30개: I44 입력 (b0~2) → I45 정류부 (b3~4) → I46 BBC (b5~7) → I47 출력 (b8~15) → I49 바이패스 (b16~25) → I51 STATUS2 (b26 DSP 통신 이상, b27 AC SPD, b28 DC SPD, b29 BMS Minor) | b0~b29 |
| Warning2·3 | 쓰지 않음 | 0 |
| Fault1 | 중고장 30개: I44 입력 (b0~12) → I45 정류부 (b13~21) → I46 BBC (b22~29) | b0~b29 |
| Fault2 | 중고장 19개: I47 출력 (b0~5) → I48 인버터 (b6~12) → I50 (b13 Emergency Stop, b14 MBS C/B Fail) → I51 (b15 BMS Major, b16 RCD 1단계, b17 RCD 2단계, b18 화재) | b0~b18 |
| Fault3 | 쓰지 않음 | 0 |

- 마스크(맵의 색)가 바뀌면 그 뒤 비트 자리가 모두 밀립니다. 그때는 eventMap 파일도 같이 고칩니다.
- 이전 배치(v1.6, 맵 비트 자리 그대로, `08-1.UPS 경고·고장.xlsx`)는 쓰지 않습니다.

**시험 기준**
- I47 = 0xFFFF → Warning1 = 0x0000FF00, Fault2 = 0x0000003F
- I51 b14 = 1 (화재) → Fault2 = 0x00040000, Warning1 = 0
- I48~I51 = 0xFFFF → Warning1 = 0x3FFF0000, Fault2 = 0x0007FFC0, 나머지 칸 0
- I44~I51 = 0xFFFF → Warning1 = 0x3FFFFFFF, Fault1 = 0x3FFFFFFF, Fault2 = 0x0007FFFF
- I51 b10 (BMS Minor) → Warning1 b29. I46 b9 (BBC PWM Fail) → Fault1 b27

### RD-UPS-03 UPS 운전 모드 — 확정

UPS_ModeStatus 칸으로 보냅니다 (v2.11, RD-UPS-01 표).

**시험 기준:** BAT Mode = 5 → UPS_ModeStatus b5 = 1, b0~b4 = 0

### RD-BAT-01 RACK Warning·Fault 배치 — 잠정 (v2.48, 배터리 eventMap v2 + Warning2)

배터리 eventMap 공유 드라이브 구글 시트 **'002.이솔루션'**(시트 '이솔루션 1.4v' 하나: BAT A~F열, RACK H~M열, 같은 줄. 2026-11-24 사용자 수정본이 기준. 로컬 `인터페이스\002.이솔루션.xlsx`는 그 내보내기본, 시험 도구가 읽음)과 같은 자리로 보냅니다. EMS는 이 파일로 비트 이름을 띄웁니다.

S = 랙 상태 영역 시작(랙 1 = I90, 랙마다 +9), D = 랙 상세 영역 시작(랙 1 = I500, 랙마다 +500)

- **칸 하나(32비트)에 BMS 워드 2개**: 첫 워드는 b0부터, 둘째 워드는 b16부터. **각 워드 안에서는 정의된 비트만 비트 번호 순서로 이어서** 채웁니다 (Reserved 뺌). 각 워드의 남는 자리는 항상 0. eventMap 양식: 출처 칸 = 원래 비트와 주소(예 `b10  [30513 + 500(n-1)]`), 비고 칸 = 워드 16줄을 합쳐 원래 맵 워드 이름.

| 칸 | b0~15 | b16~31 |
|---|---|---|
| Warning1 | S+2 System Warning Status (b0~9) | S+0 System Alarm (b0~8) |
| **Warning2 (v2.48)** | b0 = S+4 b9 Cell V Imbalance Warning, b1~4 = S+8 b0 UV·b2 OV·b4 OT·b6 UT Warning | (없음, 항상 0) |
| Fault1 | S+3 System Error Status (b0~13) | S+4 Module Error Status: b16~25 = 원래 b0~8·10 (b9 경고는 Warning2) |
| Fault2 | S+7 Fire Alarm: b0 연기 센서, **b1** = 원래 b15 화재·연기 감지기 | S+8 Module Status: b16~20 = 원래 b1·3·5·7·8 (UV·OV·OT·UT Error, AFE Error. 경고는 Warning2) |
| Fault3 | D+11 System Lock Status 1: b0~8 = 원래 b0·1·3·6~11 | D+12 System Lock Status 2: b16~29 = 원래 b0·1·4~15 |
| Fault4 | S+5 Error Code 1 (b0~15) | S+6 Error Code 2 (b0~7) |
| Fault5 | D+13 Self Test Status 1 (b0~8) | D+14 Self Test Status 2: b16~30 = 원래 b0~10·12~15 |
| Warning3~10, Fault6~10 | | 쓰지 않음 (항상 0) |

- **(v2.48, 사용자 결정)** Module Error b9(Cell V Imbalance Warning)와 Module Status 경고 비트(b0·2·4·6)는 Warning2에 둡니다. 이 비트만 켜지면 RACK Status b5(경고)만 켜지고 b6(고장)은 켜지지 않습니다. Warning2는 '워드 2개' 규칙의 예외로, 두 워드의 경고 비트를 b0부터 이어 붙입니다. (v2.37~v2.47에는 워드째로 Fault 칸에 두었음)
- 랙 상세(D+11~14)는 그 랙 상세를 받은 뒤부터 채웁니다 (랙 상세는 100 ms마다 한 조각, 8랙 한 바퀴 약 3.2초라 최대 그만큼 늦음, DV-04 v2.46).
- RACK Status b5 / b6 = 그 랙의 Warning / Fault 칸 중 하나라도 0이 아님.
- BMS Minor / Major 판단은 이 칸으로 하지 않고 UPS 비트로 합니다 (SQ-01).
- 이솔루션 답변 (2026-11-23): Lock 2 b10 CT1·CT2 전류 편차, b11 CT1 기준 전압 없음, b12 CT1·CT2 커넥터 체결 불량 (Current Sensor Fail), b13 Relay Off 명령 없이 Aux High, b14 RBMS ON 때 Aux Low 유지, b15 Relay Off 명령에도 Aux High 유지 (PRA Fail). Fire Alarm b15 = 화재·연기 감지기.
- 이전 배치: v0.4, v2.28(69비트), v2.32(126비트 빈칸 없이)는 쓰지 않습니다.

**시험 기준 (v2.37)**
- 랙 3의 S+3 = 0x2000 → RACK[3] Fault1 = 0x00002000, RACK[3] Status b6 = 1, 다른 랙 영향 없음
- 랙 1의 S+0 = 0xFFFF → Warning1 = 0x01FF0000, Status b5 = 1, b6 = 0. S+2 = 0xFFFF → Warning1 = 0x000003FF
- (v2.48) 랙 1의 S+3 = S+4 = 0xFFFF → Fault1 = 0x03FF3FFF, Warning2 = 0x00000001. S+7 = S+8 = 0xFFFF → Fault2 = 0x001F0003, Warning2 = 0x0000001E
- (v2.48) 랙 1의 S+4 = 0x0200만 → Fault1 = 0, Warning2 = 0x00000001, Status b5 = 1, b6 = 0. S+4 = 0x0400 (AFE) → Fault1 = 0x02000000 (b25)
- (v2.48) 랙 1의 S+8 = 0x0040 (UT Warning)만 → Warning2 = 0x00000010 (b4), Fault2 = 0, Status b5 = 1, b6 = 0. S+8 = 0x0080 (UT Error) → Fault2 = 0x00080000 (b19), Status b6 = 1
- 랙 1 상세 받음, D+11 = D+12 = 0xFFFF → Fault3 = 0x3FFF01FF. D+13 = D+14 = 0xFFFF → Fault5 = 0x7FFF01FF
- 랙 1 상세 받음, D+12 = 0x0400 (b10 CT1·CT2 전류 편차) → Fault3 = 0x01000000 (b24). S+7 = 0x8000 (화재) → Fault2 = 0x00000002 (b1)
- 랙 1의 S+5 = S+6 = 0xFFFF → Fault4 = 0x00FFFFFF
- 랙 2 상세 안 받음, D+11~14 = 0xFFFF → Fault3 = Fault5 = 0

### RD-BAT-02 BAT Warning·Fault·Status — 잠정

- BAT Warning1·Warning2 / Fault1~5 = 8개 랙의 같은 칸을 OR로 합친 값 (배치는 RD-BAT-01과 같음, 구글 시트 '002.이솔루션' A~F열, v2.48)
- BAT Status b6 / b7 = BAT Warning / Fault 중 하나라도 0이 아님
- BAT Status b9 Emergency = 어느 랙이든 System Lock(S+0 b8 또는 S+3 b13) = 1
- BAT b8 Balancing, b10 E-stop, RACK b8 Precharge, b9 Balancing = 항상 0 (미지원)
- **(v2.13)** BAT Status b11 = 충전 컨택터 붙음 (Input 30026 b0), b12 = 방전 컨택터 붙음 (30026 b1). RACK Status b10 = 그 랙 컨택터 붙음 (Input 30027 b(n−1)). 레코드 크기는 그대로
- **(v2.28)** BAT Status b13 = 랙 컨택터 닫힘: 30027 ≠ 0 (랙 하나라도 닫히면 1, 전부 열리면 0). EMS 단선도의 DC 컨택터 표시용. 어느 랙인지는 RACK Status b10

**시험 기준 (v2.13):** 30026 = 0x0001, 30027 = 0x0085 → BAT b11 = 1, b12 = 0, RACK 1·3·8 b10 = 1, RACK 2 b10 = 0

**시험 기준 (v2.28)**
- 30027 = 0x0085 → BAT b13 = 1. 30027 = 0x0000 → BAT b13 = 0. BMS 통신 없음 → BAT Status = 1 (b13 = 0)
- 랙 1 S+2 = 0x0001, 랙 2 S+2 = 0x0002 → BAT Warning1 = 0x0003

**시험 기준 (v2.37):** 랙 3 D+11 = 0x0400 (상세 받음) → BAT Fault3 = 0x00000080 (b7). 랙 4 S+6 = 0x0004 → BAT Fault4 = 0x00040000 (b18)

**시험 기준 (v2.48):** 랙 1 S+8 = 0x0001, 랙 2 S+4 = 0x0200 → BAT Warning2 = 0x00000003, BAT Status b6 = 1, b7 = 0

### RD-IMD-01 IMD 보고 — 잠정

토픽 `imd`, 레코드 `ST_ImdPublish_V1` 32 B (인터페이스 `22. IMD.xlsx` V1 그대로). 장치 LS IMD1000 (DV-11). 2026-11-09 사용자 결정: 저항이 높은 경보부터 Status에 배치.

| 필드 | 값 | IMD1000 출처 (프로토콜 주소) |
|---|---|---|
| DeviceIndex | 1부터 | |
| Status b0 | 통신에러 (나머지 칸 0) | 응답 10초 없음 |
| Status b1 Pre-Warning | Pre Alarm#3 동작 (초기값 100 kΩ) | IR 210 b3 |
| Status b2 Warning | Pre Alarm#1 OR #2 동작 (초기값 40·70 kΩ) | IR 210 b1·b2 |
| Status b3 Fault | 자가진단 오류(메모리·RTC·교정) OR Device 이상 | IR 209 b0~2, IR 211 b1 |
| Status b4 Alarm | Ins. Alarm 동작 (초기값 10 kΩ) | IR 210 b0 |
| Status b5 Internal test | Self Test 진행 중 | IR 215 = 1 |
| Status b6 External test | 버튼·DI Self Test 입력 | IR 206 b0·b1 |
| InsulationResistance | 절연저항 kΩ | IR 224~225 FLOAT32 |
| DeviceErrorLocation | 0 (Bender 항목) | 없음 |
| InsulationResistanceFaultLocation | 0 (Bender 항목) | 없음 |
| PreAlarmSettingValue | Pre Alarm#1 Pick-up kΩ | HR 46~47 UINT32 |
| MainAlarmSettingValue | Ins. Alarm Pick-up kΩ | HR 40~41 UINT32 |
| Fault1 | b0 System 연결 이상, b1 PE(접지) 연결 이상 | IR 211 b2, b0 |

- 설정 영역(HR)은 측정 10번에 한 번 읽는다. 못 읽었으면 설정값 칸은 0.
- 32비트 값의 워드 순서(앞 = 상위)는 맵에 적혀 있지 않다. MESSIS 현장값이 상위 워드를 224로 읽고 맞았으므로 앞 = 상위로 봄 (현장 확인).

시험 기준
- 통신 이상 → Status = 1, 나머지 0
- IR 210 = 0x0008 → Status = 0x02 / 0x0002 또는 0x0004 → 0x04 / 0x0F0F → 0x16 (Pick-up 비트 b8~11은 무시)
- IR 209 b1 또는 IR 211 b1 → Status b3. IR 211 = 0x0007 → Fault1 = 0x03
- IR 215 = 1, IR 206 b1 → Status 0x60. IR 215 = 2, IR 206 b2(Stand-By)만 → 0

### RD-BAT-03 MODULE 모듈 전압 — 잠정

- BAT 질의 Q6 답변: 모듈은 전압·온도를 표시, 셀 전압은 별도 창. 저장 주기는 총괄 결정 (업체는 대기 1분, 충방전 중 1초로 기록)
- **v2.17 (인터페이스 `10. Module.xlsx` 2026-11-17):** 칸 순서는 BatIndex, RackIndex, DeviceIndex → **ModuleVolatge1~3** (FLOAT32, V, `Volatge` 철자는 인터페이스 원본대로) → ModuleTemp1~3 → CellVoltage1~20. 레코드 104 → **116 B**. 온도·셀 전압 칸 위치가 12 B 뒤로 밀렸으므로 EMS 수신 코드도 바꿔야 한다
- ModuleVolatge2·3은 BMS 맵에 대응 값이 없어 0
- (폐기) 예전 안: 맨 끝에 `ModuleVoltage` 1칸, 104 → 108 B
- 출처: ModuleVolatge1 = 랙 상세 Module Voltage (D+15+m, m = 맵 Module 번호 0~, uint16 0.01 V. 첫 랙 첫 모듈은 30516). 상세가 유효하지 않은 랙은 0
- **발행 주기 (Q6 저장 주기):** module 토픽도 다른 토픽과 같이 **무조건 1초**마다 보냅니다 (대기·충방전 구분 없음, 2026-11-11 사용자 결정). EMS는 받은 것을 저장하므로 저장 주기도 1초가 됩니다. 이전 작업본의 대기 60초(`MODULE_PUB_IDLE_TICKS`)는 새 STD에 옮기지 않음

**시험 기준:** 랙 2 상세 D+15 = 5180 → 랙 2 첫 모듈 ModuleVolatge1 = 51.80 V, ModuleVolatge2·3 = 0. 랙 1 상세 무효 → 랙 1 모듈 ModuleVolatge1 = 0. 레코드 크기 116 B. 수신기에서 module 간격이 대기 중에도 1 s

### RD-FIRE-01 화재감지기 보고 — 잠정 (v2.9, v2.47 정리)

토픽 `fire_detector` (이름 잠정, EMS 확인). 인터페이스 `20. OffGas` 양식: DeviceIndex + Status 8 B.

| DeviceIndex | 장비 | Status |
|---|---|---|
| 1 | 가스디엔에이 TS-60 연기 접점 (EtherCAT DI 1) | b0 통신 이상(EtherCAT 단자 상태), b1 연기 알람, b2 고장(알 수 없어 0) |
| 2 | 가스디엔에이 ASD-57 (RTU) | b0 통신 이상, b1 연기 또는 온도 알람, b2 고장(0) |

**시험 기준:** TS-60 DI 1 = 1 → Status 0x02 / ASD-57 통신 끊김 → Status 0x01

### RD-TMP-01 온습도 보고 — 잠정 (v1.9, v2.47 정리)

토픽 `temperature_sensor` (인터페이스 `07. Temperature Sensor`). 칸: DeviceIndex, status, location, temperature(℃), humidity(%). 장비 ITGuard IE-SX (DV-10), location 2 = 배터리실 (잠정). EMS가 이 값으로 배터리실 온·습도를 직접 판정합니다 (CF-PRT-01).

**시험 기준:** 온도 24.18 ℃, 습도 35.45 % → temperature 24.18, humidity 35.45, location 2 / 통신 끊김 → status b0

### RD-MQTT-01 MQTT 공통 — 확정 (v2.47 정리)

- **보내는 주소:** `dgspms/ups/{사이트}/{토픽}`, 받은 설정 되돌려 주기 `dgspms/ups/{사이트}/config/{장치}`, 이벤트 로그 `dgspms/ups/{사이트}/pms_log` (이벤트가 생길 때 1건씩).
- **받는 주소:** `dgsems/ups/{사이트}/command/{장치}`, `dgsems/ups/{사이트}/config/{장치}`. 레코드의 DeviceIndex는 모든 토픽 **1부터** (2026-11-10).
- **형식:** 바이너리 (장치 헤더 40 B + 레코드, config는 헤더 64 B). JSON 아님.
- **발행 주기:** 모든 장치 토픽 **1초** (대기·충방전 구분 없음, 2026-11-11 결정).
- **보내는 토픽:** `ups`, `bat`, `rack`, `module`, `imd`, `fire_detector`, `temperature_sensor`, config echo. `pms` 토픽은 EMS가 쓰지 않습니다 (2026-11-23).
- **브로커:** Cloud·Local·Azure 세 칸 중 쓰는 것만 (`GVL_SITE.MQTT_SET`, 지금 Local). 고른 브로커 접속이 30초 넘게 끊기면 이벤트 로그 알람 '브로커 끊김' (`GVL_PERSIST.g_stBrokerWatch`).
- **이중화 (RED-02):** 동작 중인 PLC만 발행하고, 두 PLC 모두 구독합니다. 접속 이름(ClientId)에 -P / -S를 붙여 서로 끊지 않습니다.
- `pms_log`에는 PMS 코어 이벤트(RD-LOG-01)가 갑니다. 사고·고장 기록(PmsUps 클래스)은 보내지 않습니다 (SQ-01 v2.21).

**시험 기준:** 사이트 5 → `dgspms/ups/5/ups` 1초마다 / `dgsems/ups/5/command/ups` DeviceIndex 1 → UPS 1 명령, 0 → 버림

### RD-LOG-01 PMS 이벤트 로그 — 확정 (v2.47 정리)

TwinCAT 이벤트 로거(XAE Error List)에 남깁니다. 사고·고장 관련(PmsUps)은 SQ-01 표, 그 밖(PmsSystem)은 아래입니다.

| 종류 | 이벤트 |
|---|---|
| 알람 | 설정 저장 실패, 설정 오류로 장비 제어 중지, MQTT 브로커 끊김, 상대 PLC 끊김, 이중화 통신 한쪽 고장, 동기화 값 못 받음, 이중화 설정 오류, 보호 규칙 발생(규칙 1~7, 모두 알람만), UPS 설정 불일치(CF-UPS-03) |
| 메시지 | PMS 시작(마지막 동작 시각·꺼져 있던 시간), 운영 설정 바뀜·되돌림, 이중화 시작·절체·복귀·양보·이어짐·동기화로 맞춤 |

- ~~이중화 대기 중에는 장비 통신 끊김 알람(보호 규칙 4, PmsUps CommLost)을 띄우지 않습니다 (RED-02 v2.45).~~ v2.52: 대기 중에도 장비를 읽으므로 띄웁니다.

### RD-X-01 표기하지 않는 항목 — 확정

BMS 충방전 전력 제한값(I21~I24), 모듈별 Module Status, 랙 상세 6~10·13·14번

## 3. 쓰기 (EMS → PMS 명령)

### WR-CMD-01 command 레코드 공통 — 버전 2 (2026-11-25 사용자 지시)

command 레코드(pcs·bat·ups 공통, 16 B)는 **DeviceIndex · CommandType · Value · Delay** 순서입니다. CommandVersion은 2이며, 헤더 64 B와 명령 1건의 전체 길이는 80 B입니다.

- **SequenceNumber 없음:** 같은 명령을 다시 보내도 처리합니다. PLC 내부의 태스크 전달용 nSeq는 그대로 사용하며 통신 레코드에는 넣지 않습니다. 디스크의 `100.Command.xlsx` A4에는 옛 SequenceNumber 행이 남아 있으나, 사용자 지시와 실제 80 B 수신 형식을 따릅니다.
- **Delay (ms):** 이만큼 기다렸다가 실행합니다. 0이면 바로. 기다리는 동안 같은 장치에 새 명령이 오면 기다리던 명령은 버리고 새 명령을 따릅니다. 1시간(3 600 000 ms)을 넘으면 버립니다. 재시작하면 기다리던 명령은 실행하지 않습니다.
- **위치:** `FB_EmsCmdGate`가 PRG_Mqtt에서 명령 핸들러(FB_EmsCommand, UPS 칸)보다 먼저 처리합니다. `F_MqttDistribute`는 DeviceIndex를 레코드 맨 앞에서 읽습니다(`nIndexOffset = 0`).

**시험 기준:** 같은 명령 두 번 → 둘 다 처리 / 충전 컨택터 닫기 80 B → 장치 1·명령 7로 해석 / Delay 600 000 → 보관, 핸들러에 안 보임 / 기다리는 중 새 명령 → 새 명령 바로 / Delay 3 600 001 → 버림

### WR-UPS-01 UPS 명령 — 잠정 (2026-11-25 운전 모드 추가)

**보고 운전 모드 추가 (2026-11-25 사용자 지시):** `command/ups`의 CommandType 101은 OperationMode=0(수동), 102는 OperationMode=1(자동)으로 반영합니다. 값은 `MAIN.fbUpsCommand[n].q_nOperationMode`에 유지하며 시작 기본값은 1입니다. 장비 쓰기 허가가 필요하고, UPS 통신 상태와 무관한 PMS 보고 모드이므로 UPS 레지스터에는 쓰지 않습니다. 실제 장치 Auto ON/OFF인 13·14와 별개이며, 장치 명령·보호 동작을 이 모드로 차단하지 않습니다.

**CommandType은 번호 1~26**이고 Value는 0입니다. 번호 n이면 세방 맵 H(99+n)에 0x55를 씁니다.

| 번호 | 명령 | 맵 | 번호 | 명령 | 맵 |
|---|---|---|---|---|---|
| 1 | 리셋 | H100 | 14 | Auto OFF | H113 |
| 2 | 버저 정지 | H101 | 15 | 비상 방전 시작 | H114 |
| 3 | 정류부 ON | H102 | 16 | 주기적 방전 시작 | H115 |
| 4 | 정류부 OFF | H103 | 17 | 펄스 방전 시작 | H116 |
| 5 | 인버터 ON | H104 | 18 | 비상·주기적·펄스 방전 취소 | H117 |
| 6 | 인버터 OFF | H105 | 19 | 간헐충전 시작 | H118 |
| 7 | BBC ON | H106 | 20 | 간헐충전 취소 | H119 |
| 8 | BBC OFF | H107 | 21 | 온도보상충전 시작 | H120 |
| 9 | 인버터로 절체 | H108 | 22 | 온도보상충전 취소 | H121 |
| 10 | 바이패스로 절체 | H109 | 23 | ECO 시작 | H122 |
| 11 | 부동충전 | H110 | 24 | ECO 취소 | H123 |
| 12 | 균등충전 | H111 | 25 | PRLT 시작 | H124 |
| 13 | Auto ON | H112 | 26 | PRLT 취소 | H125 |

- **일괄 기동 (2026-11-16 UPS 보드 시험):** 13 Auto ON 하나로 정류부·BBC·인버터가 모두 기동한다. 3·5·7(정류부·인버터·BBC ON)을 하나씩 보낼 필요가 없다.
- **다시 확인 (v2.15):** 명령을 받을 때뿐 아니라 드라이버가 바빠 기다리는 동안에도 매 스캔 다시 본다. 기다리는 중에 고장·e-stop이 생기면 켜기·시작 명령은 취소(거부로 셈)하고, 끄기·취소·리셋은 그대로 보낸다. 드라이버 대기열에 이미 들어간 켜기·시작 명령과 설정 쓰기도 드라이버가 뺀다.
- **거부 조건:** 통신이상이면 모두 거부. 고장(Status b2)이나 e-stop(b3) 중이면 켜기·시작(3, 5, 7, 9, 11, 12, 13, 15, 16, 17, 19, 21, 23, 25)만 거부하고 끄기·취소·리셋은 보냅니다. 1~26 밖은 모르는 명령으로 셉니다.
- **보관값:** 19·20이면 간헐충전, 21·22이면 온도보상 켜짐 상태를 저장합니다(CF-UPS-03).
- **Fault 확인:** EMS가 합니다(v2.4). 예전 b10은 없앴습니다.
- **결과 확인 (v2.47 정리, 2026-11-23 결정):** PMS는 명령(예: 14 Auto OFF)을 UPS에 쓰기만 하고, 그 뒤 UPS 동작(바이패스로 넘어가는지 등)은 판단하지 않습니다. 결과는 상태 칸(BypassStatus, InverterStatus, MCCB_Status)으로 EMS에 보고합니다.
- **경로:** EMS `command/ups`(DeviceIndex 1~3) → PRG_Mqtt(FB_EmsCmdGate → GVL_IF.g_astUpsCmd) → MAIN.fbUpsCommand(PlcTask) → UPS 드라이버. 실제 쓰기는 `UPS_CONTROL_ENABLED`와 쓰기 허가가 모두 켜져야 나갑니다.

**시험 기준**
- 18 → H117 요청 1회
- e-stop 중 19 → 거부. 이어서 18 → 허용
- 1, 10, 26 → H100, H109, H125
- 27, 0 → 요청 없음, 모르는 명령으로 셈
- (v2.15) 13 Auto ON을 드라이버가 바빠 기다리는 중 e-stop 발생 → 보내지 않음, 거부 1
- (v2.15) 18 방전 취소를 기다리는 중 고장 발생 → 그대로 보냄 (H117)

### WR-SQ-01 Fault 단계 확인 명령 — 폐기 (v2.38: PMS 안에서 Fault를 붙잡지 않으므로 확인할 것이 없음. 확인은 EMS 안에서 처리, v2.4)

**v2.38 폐기:** PMS 안에서 Fault를 붙잡지 않으므로 이 명령은 쓰지 않습니다. 코드에도 없습니다. 아래는 예전 내용으로 기록만 남깁니다.

v2.4부터 단계 표시와 확인은 EMS가 합니다. 아래는 PMS 안 비교용 계산에만 해당하며, EMS가 b10을 보내지 않으면 쓰이지 않습니다.


운영자가 EMS에서 Fault 단계(4~6)를 확인하는 명령입니다. 원인이 이미 해소된 Fault 표시만 내립니다. UPS command CommandType b10(0x400)으로 받습니다. 원인이 남아 있으면 표시를 유지합니다. 장비에는 아무것도 쓰지 않습니다.

**시험 기준**
- 4단계 원인 해소 후 확인 명령 → 단계 0
- 4단계 원인이 남은 상태에서 확인 명령 → 단계 4 유지

### WR-BAT-01 BAT 명령 — 잠정 (v2.13, BMS 맵 v1.4)

command 레코드는 WR-CMD-01 버전 2 (DeviceIndex · CommandType · Value · Delay)이고, **CommandType은 번호**입니다 (인터페이스 `100.Command.xlsx` BAT). 이전 판의 비트 방식 안은 버림.

| CommandType | 명령 | PMS 처리 (BMS 레지스터) |
|---|---|---|
| 1 | 닫기 (일괄) | 코어 경로 → 40013~40015 = [1, 1, 1] **FC16 한 번** (2026-11-12 이솔루션: 순서 무관) |
| 2 | 열기 (일괄) | 코어 경로 → 40013~40015 = [0, 0, 0] FC16 한 번 (2026-11-13 사용자 결정 2안) |
| 3 | Estop | **(v2.33) `FB_BmsContactorCommand` → 40012 Emergency Shutdown = 1** (코어 순서 거치지 않고 바로) |
| 4 | 초기화 | **(v2.34) `FB_BmsContactorCommand` → 40031 Reset System = 1** (코어 순서 거치지 않고 바로). BMS 비상차단 해제만, 컨택터는 다시 붙이지 않음 |
| 11 | 랙 컨택터 닫기 | `FB_BmsRackCommand` → 40001 랙 비트에서 그 랙만 켬 |
| 12 | 랙 컨택터 열기 | `FB_BmsRackCommand` → 40001 랙 비트에서 그 랙만 끔 |
| 5 / 6 | 시스템 연결 / 차단 | `FB_BmsContactorCommand` → 40013 Rack Contactor Control = 1 / 0 (랙 8개 한꺼번에) |
| 7 / 8 | 충전 컨택터 닫기 / 열기 | `FB_BmsContactorCommand` → 40014 = 1 / 0 |
| 9 / 10 | 방전 컨택터 닫기 / 열기 | `FB_BmsContactorCommand` → 40015 = 1 / 0 |

- **11·12의 Value = 랙 번호 1~8** (정수). 예: 1번 BAT 6번 랙 닫기 = DeviceIndex 1, CommandType 11, Value 6 (DeviceIndex는 모든 command·config 토픽 1부터, 2026-11-10).
- 랙 선택표(비트 n-1 = 랙 n)는 PERSISTENT(`GVL_PERSIST.g_awBmsRackMask`), 저장값이 없으면 설치된 랙 전부. 명령마다 표 전체를 다시 씀.
- **(v2.13)** BMS 데이터가 유효하면 실제 랙 컨택터 상태(Input 30027)를 바탕으로 그 랙 비트만 바꿔 씀. 40013으로 전 랙이 붙은 뒤 옛 표로 다른 랙을 떼는 일을 막음. 데이터가 없으면 기억한 표.
- **40013과 40001:** 40013 = 1이면 40001과 상관없이 랙 8개가 다 붙음(그래서 맵 v1.4에서 이름이 System → Rack Contactor Control). 40001은 랙 하나씩 (2026-11-13 사용자 설명).
- **컨택터 상태 (Input, 읽기 전용):** 30026 System Contactor State b0 충전·b1 방전, 30027 Rack Contactor State b0~b9 랙 1~10. 맵의 RW 표기는 틀림(사용자 확정). PMS는 System 데이터를 30001~30027(27칸)로 읽음.
- **코어 피드백:** 컨택터 붙음 = 설치 랙 중 하나라도 붙음 AND (충전 또는 방전 컨택터 붙음). 그래서 하나라도 붙어 있으면 2 열기가 나가고, 열기 확인은 다 떨어져야 됨. 예전에는 피드백이 늘 FALSE라 2 열기가 BMS로 나가지 않았음.
- 3·4·5~10·11·12는 코어 순서(유닛 정지 확인)를 거치지 않고 바로 씀 (3은 v2.33, 4는 v2.34부터). BMS 비상정지 해제(4 초기화)는 EMS 원격으로도 함 (v2.34 사용자 결정). 컨택터 재투입은 1·5~10 등 따로 명령.
- **거절:** 랙 번호 범위 밖·정수 아님, 마지막 남은 랙 열기(전체 열기는 2번). 드라이버가 바쁘면 5초 동안 다시 시도.
- 실제 쓰기는 `GVL_PARAM.UPS_CONTROL_ENABLED`와 쓰기 허가(설정 정상, 이중화 Active)가 있어야 함.
- 충·방전 명령은 BMS로 보내지 않음.

**시험 기준 (`FB_Test_BmsRackCommand`)**
- 저장값 없음 → 선택표 0x00FF (8랙)
- 랙 3 열기 → 쓰기 0x00FB, 드라이버가 받은 뒤 선택표 0x00FB
- 랙 9·랙 0·마지막 랙 열기 → BadRequest, 쓰기 없음
- 드라이버 Disabled → 결과 Disabled로 끝, Busy → 계속 요청
- command/bat 11, Value 5 → BMS 1 랙 5 닫기 / Value 2.5 → 랙 0(거절)
- (v2.13) 기억한 표 0x00FB, 실제 상태 0x03FF에서 랙 5 열기 → 쓰기 0x00EF (설치 랙만, 랙 3 유지)

**시험 기준 (`FB_Test_BmsContactorCommand`, v2.13)**
- 5 → 40013 닫기, 6 → 40013 열기, 8 → 40014 열기, 9 → 40015 닫기
- (v2.33) 3 → 40012 비상정지 (EmergencyShutdown, 켜기). command/bat 3 → astBatCtl 번호 3, 코어 명령(astBat)은 그대로
- (v2.34) 4 → 40031 초기화 (ResetSystem, 1). command/bat 4 → astBatCtl 번호 4, 코어 명령(astBat)은 그대로

**BMS 시각 동기 (v2.3 정함, v2.35 구현):** PMS 현지 시각(FB_LocalClock)을 BMS Holding 40005~40007에 **FC16 한 번**으로 씁니다. 값은 바이트마다 숫자 그대로 (40005 = 연도 끝 두 자리·월, 40006 = 일·시, 40007 = 분·초. 예: 2026-11-23 10:05:09 → 16#1A0B, 16#170A, 16#0509). BMS 통신이 정상이고 PMS 시각이 유효해지면 먼저 한 번, 그 뒤 1시간마다(`i_tTimeSync`, T#0S = 끔). 쓰기 관문(UPS_CONTROL_ENABLED, 쓰기 허가)을 따르고, 쓰지 못하면 다음 스캔에 다시.
- 11 → BadRequest, 쓰기 없음. 같은 번호 다시 → 쓰기 없음
- 드라이버 Busy → 계속 요청, Disabled → 결과 Disabled로 끝
- command/bat 7 → astBatCtl 번호 7, 모르는 명령 아님 / 13 → 모르는 명령

### WR-GATE-01 장비 쓰기 관문 — 확정 (v2.47 정리)

PMS가 장비(UPS·BMS)에 쓰는 모든 것(EMS 명령, 설정 쓰기·복구, 사고 대응, BMS 시각 동기)은 아래가 **모두** 켜져야 나갑니다.

| 관문 | 뜻 | 바꾸는 법 |
|---|---|---|
| `GVL_PARAM.UPS_CONTROL_ENABLED` | UPS·BMS 쓰기 전체 | 코드 상수 (다시 다운로드) |
| `GVL_SITE.DEVICE_WRITE` | 장비 쓰기 전체 | 코드 상수 (다시 다운로드) |
| 설정 정상 | 설정 점검(`FB_ConfigCheck`)·UPS 설정 점검 통과 | 자동 |
| 이중화 쓰기 권한 | 이중화를 쓰면 동작 중인 PLC만 (RED-02) | 자동 |
| (사고 대응만) `GVL_PERSIST.g_bUpsSafetyRespEnable` | 사고·고장 대응 켜짐 (기본 켬, v2.40) | 운영자 (온라인) |

- 커밋 값은 `UPS_CONTROL_ENABLED`, `DEVICE_WRITE` 모두 FALSE입니다. 현장에 처음 붙일 때 끄고 시작해, 읽기를 확인한 뒤 켭니다. 시험 PLC 작업본에서만 TRUE로 둡니다.

**시험 기준:** 관문 하나라도 FALSE → UPS·BMS 쓰기 없음, 읽기·보고는 그대로

## 4. 설정 (EMS → PMS)

### CF-UPS-01 UPS 설정 — 잠정

| 항목 | 레지스터 | 단위 (스케일) |
|---|---|---|
| FloatChargeSetVoltage (v2.51) | H7 Batt 부동 충전 전압 | V (0.1) |
| ChargeSetCurrentLimit (v2.51, 예전 임시 이름 ChargeCurrentLimit) | H10 Batt 충전 전류 (제한값) | A (0.1) |
| TrickleChargeSetParameter | H57 | 0 Disable / 1 전압 / 2 SOC (v2.14 적용) |
| TrickleChargeSetStartVoltage / EndVoltage | H58 / H59 (H57 = 1일 때) | V (0.1) (v2.14 적용) |
| TrickleChargeSetStartSoc / EndSoc | H58 / H59 (H57 = 2일 때) | % (단위 O-14, 기본 1 %) (v2.14 적용) |
| TrickleChargeSetEndTime (이름 변경, 지금 EMS 칸 이름은 StartTime) | H60 간헐충전 종료시간 | 분 (v2.14 적용) |
| TrickleChargeSetReStartDay | H61 | 일 (v2.14 적용) |
| PeriodicTrickleDischargeSetCurrent / Time | H50 / H51 | A (0.1) / 분 |
| Pulse… Current / Time / Cycle | H52~H56 | A (0.1) / 초 / 회 |
| TempComp 기준전압·셀 수·기울기 | H62 / H63 / H64 | V / 셀 / mV/℃ |

- **레코드 순서 (v2.51):** DeviceId → FloatChargeSetVoltage → ChargeSetCurrentLimit → TrickleChargeSetParameter … (구글 시트 UPS_V1 순서). 두 칸 모두 REAL_FIELD(값 REAL + is_available UDINT, 8 B)라 UPS 한 대 레코드가 148 → 164 B로 늡니다 (EMS에 알릴 것).
- **H7·H10 쓰기 (v2.51):** 값 × 10을 씁니다. 범위: H7 0 초과 `UPS_CFG_MAX_VOLTAGE_V`(1000 V) 이하, H10 0 초과 `UPS_CFG_MAX_CURRENT_A`(100 A) 이하. 벗어나면 그 UPS의 이번 설정 전체를 거부합니다. 다른 칸과 같이 부동충전(BAT Mode 0)일 때만 쓰고, 보관·복구(CF-UPS-02·03)도 같습니다. 쓰는 순서는 H50~H61 다음 H7 → H10입니다.

**간헐충전 쓰기 규칙 (v2.14)**
- UPS는 H58·H59 한 쌍을 시작·종료 값으로 씁니다. H57이 1이면 전압, 2이면 SOC입니다. EMS 칸은 4개(전압 2, SOC 2) 그대로 두고 PLC가 고릅니다.
- 기준은 받은 TrickleChargeSetParameter를 씁니다. 이번에 없으면 보관값(CF-UPS-02)의 H57, 그것도 없으면 UPS에서 읽은 H57을 씁니다.
- 기준 1: StartVoltage·EndVoltage × 10 → H58·H59. 기준 2: StartSoc·EndSoc × SOC 단위(`GVL_PARAM.UPS_CFG_SOC_SCALE`, 기본 1) → H58·H59. 기준 0: H58·H59는 쓰지 않습니다.
- 기준을 1이나 2로 보낼 때는 그 기준의 시작·종료를 함께 보내야 합니다. 빠지면 설정 전체를 거부합니다 (다른 기준의 옛 값이 남아 뜻이 바뀌는 것을 막음).
- 시작 ≥ 종료, 전압 0 이하 또는 `UPS_CFG_MAX_VOLTAGE_V`(1000 V) 초과, SOC 0 이하 또는 100 초과, H60 0 또는 `UPS_CFG_MAX_TRICKLE_MINUTES`(1440 분) 초과, H61 0 또는 365 일 초과면 그 UPS의 이번 설정 전체를 거부합니다.
- 쓰는 순서는 H57 → H58 → H59 → H60 → H61입니다. 부동충전(BAT Mode 0)일 때만 씁니다 (기존 규칙).

**시험 기준**
- (v2.51) ChargeSetCurrentLimit = 10.0 수신 → H10 = 100. FloatChargeSetVoltage = 545.0 → H7 = 5450
- (v2.51) FloatChargeSetVoltage = 0 또는 1000 초과, ChargeSetCurrentLimit = 0 또는 100 초과 → 거부 (BadConfig), 쓰기 없음
- 기준 1, 시작 510.0 V, 종료 530.0 V → H57 = 1, H58 = 5100, H59 = 5300
- 기준 2, 시작 70 %, 종료 80 % → H57 = 2, H58 = 70, H59 = 80
- 기준 없이 SOC 70·80만 받음, 보관값 H57 = 2 → H58 = 70, H59 = 80. 이때 같이 온 전압 칸은 쓰지 않음
- 기준 2인데 StartSoc만 받음 → 거부 (BadConfig), 쓰기 없음
- 기준 1, 시작 530.0 V ≥ 종료 510.0 V → 거부
- 기준 0 → H57 = 0만 쓰고 H58·H59는 쓰지 않음
- H60 = 120, H61 = 30 수신 → H60 = 120, H61 = 30

### CF-PRT-01 보호 기준 — 보류 (2026-11-16: 안 씀)

**결정 (2026-11-16, 사용자):** PMS는 `config/protection` 값을 판정에 쓰지 않습니다. 받고 되돌려 주기(echo)만 합니다.
- 배터리 온도(BatWarrning·BatFault): BMS가 직접 판정하는 Level 1(Minor)·2(Major)·3과 같은 기준(SafetyLevel 41·43 ℃)을 베낀 것이라, 사고·고장 단계(SQ-01)로 대체합니다. PMS가 같은 판정을 한 번 더 하면 기준이 둘이 되어 어긋날 수 있습니다.
- 배터리실 온·습도: 넘었을 때 PMS가 할 동작(차단·팬 등)이 없고 알람만이면 되므로, EMS가 `temperature_sensor` 토픽 값(온습도계, location 2 = BAT실)으로 직접 판정합니다.
- PMS가 배터리실 온·습도로 동작(충전 정지, 공조·팬 켜기 등)을 해야 하게 되면 그때 다시 살립니다 (아래는 그때를 위한 옛 내용).
- BAT Warning10·Fault10은 0으로 둡니다.

| 항목 | 기본값 | 비교 대상 | 결과 위치 |
|---|---|---|---|
| BatWarrning / BatFault | 41 / 43 ℃ | BMS I12 Cell Temperature Max | BAT Warning10 b0 / Fault10 b0 |
| BatRoomTempWarrning / Fault | 현장 기준 | UPS I43 온도6 | BAT Warning10 b1 / Fault10 b1 |
| BatRoomHum… | — | 측정원 없음 | 보류 |

입력 범위: 경고 < 고장, 고장 ≤ 43 ℃. 범위를 벗어나는 값은 거부하고 이전 값을 유지합니다.

**시험 기준:** BatWarrning 41, 셀 최고 온도 41.5 ℃ → BAT Warning10 b0 = 1

### CF-UPS-02 설정 보존 — 확정

- EMS에서 받은 UPS 설정값과 기능 켜짐 상태(간헐충전, 온도보상)를 PMS 유지 변수(GVL_PERSIST)에 보관합니다.
- 저장은 FB_PersistSave로 합니다. 바뀌면 1초 뒤 바로 저장하고, 60초마다 주기 저장합니다.
- **전원 장애 저장 (v2.39, 사용자 결정):** 제어기(CX5130)의 내장 1초 UPS가 전원 장애를 알리면(`FB_S_UPS_CX51x0`, `eSUPS_CheckPowerStatus`, 라이브러리 `Tc2_SUPS`) 바로 저장합니다. HELIOS `FB_PmsStatus`와 같은 방식입니다. 1초 UPS가 버티는 동안 저장을 마칩니다. 윈도우 종료는 하지 않습니다(상태 확인 모드). 내장 UPS가 없는 제어기(로컬 런타임 등)에서는 신호가 안 들어올 뿐 동작은 같습니다.

**시험 기준:** EMS 설정을 받으면 보관값에 남고, 같은 스캔에 바로 저장 요청이 나갑니다(H50 = 12.0 A → 120).
- (v2.39) 제어기 전원을 끊음 → 전원 장애 감지 → 바로 저장 요청 (저장 횟수 +1). 다시 켜면 끊기 직전 값으로 시작 (예: 사고 대응 스위치를 바꾸고 60초 안에 전원을 끊어도 바뀐 값이 남음). 실제 CX5130에서만 확인 가능

**구현 범위 (v0.6, v2.14 갱신):** H50~H61(주기적·펄스 방전, 간헐충전)입니다. 보관값 배열이 7칸에서 12칸으로 늘어 PLC에 처음 올릴 때 예전 보관값이 지워질 수 있습니다 (EMS가 설정을 한 번 다시 보내면 됨). H62~H64(온도보상)는 아직 넣지 않습니다 (O-11). **v2.51:** H7 부동 충전 전압·H10 충전 전류를 넣어 보관값이 12 → 14칸(칸 12 = H7, 13 = H10)이 됩니다. 이때도 PLC에 처음 올리면 예전 보관값이 지워질 수 있습니다 (EMS가 설정을 한 번 다시 보내면 됨).

### CF-UPS-03 설정 복구 — 확정

- PMS는 UPS 설정 레지스터를 주기적으로 읽어 보관값과 비교합니다. 다르면 보관값을 다시 씁니다. UPS 재기동, PMS 재시작, 통신 복구 후에도 설정이 맞춰지는 것이 목적입니다.
- 켜져 있어야 하는 기능이 꺼져 있으면 시작 명령을 다시 보냅니다. 온도보상은 STATUS2 b2(Status b17)로 판단합니다(v0.7 구현). 간헐충전은 UPS 상태로 켜짐을 확인할 칸이 없어 복구하지 않고, 켜짐 보관값만 남깁니다.
- UPS가 고장(Status b6)이거나 e-stop(b7)인 동안에는 다시 쓰지 않습니다. v2.15: 쓰기 요청을 기다리거나 되읽는 중에 고장·e-stop이 생겨도 쓰지 않고, 실패로 세지 않고 풀릴 때까지 기다립니다.
- 다시 쓰기를 3회 연속 실패하면 "설정 불일치" 알람을 띄우고 반복을 멈춥니다.

**시험 기준**
- UPS H50 읽은 값 100, 보관값 120 → H50에 120 쓰기 1회. 일치한 뒤에는 더 쓰지 않음
- (4단계로 옮김) 온도보상 보관값 ON, STATUS2 b2 = 0, 고장 없음 → H120에 0x55 쓰기 1회. 켜짐 상태는 시작 명령(4단계)이 정하므로 함께 구현합니다
- e-stop 중이거나 UPS 고장(Status b6) 중이면 → 쓰기 없음
- 3회 연속 실패 → 설정 불일치 알람 = 1, 다시 쓰기 멈춤(새 설정을 받으면 풀림)
- (v2.15) 쓰기를 기다리는 중(드라이버 바쁨) 고장 발생 → 쓰기 없음, 불일치 알람 없음. 고장이 풀리면 1회 씀

## 4-1. 사고·고장 대응

**C안 (2026-11-11 결정, v2.16 명세화):** PMS가 단계를 판정하고(SQ-01), 7쪽 표의 UPS·BMS 동작을 직접 명령합니다(SQ-05). 대응 전체는 스위치로 켜고 끕니다(기본 OFF). 꺼져 있으면 단계 판정과 표시만 합니다.

### SQ-01 단계 판정 — 잠정

| 단계 | 상태표시 | 조건 (맵) |
|---|---|---|
| 0 | 정상 | 아래 조건 없음 |
| 1 | Warning | AC·DC SPD (I51 b8·b9), UPS 경고장 (나머지 경고장 비트) |
| 2 | Warning | BMS Minor (I51 b10) |
| 3 | Warning | BMS Major (I51 b11), 입력·정류부·출력·인버터 중고장 (I44·I45, I47, I48), 배터리 DC 전로 지락 1단계 = RCD 1단계 (I51 b12, v2.50 세방 회신) |
| 4 | Fault | 배터리 컨버터 중고장 (I46), 배터리 DC 전로 지락 2단계 = RCD 2단계 (I51 b13, v2.50). DC 전로 단락은 신호 위치를 몰라 칸만 둠 (O-09) |
| 5 | Fault | BMS Level 3 (BAT Status b9), 화재 (I51 b14) |
| 6 | Fault | ESD (I50 b0) |

**EMS가 보는 칸 (v2.20, RD-UPS-02 eventMap 자리):**

| 원인 | EMS 칸·비트 |
|---|---|
| AC·DC SPD | Warning1 b27·b28 |
| UPS 경고장 | Warning1 b0~b26 |
| BMS Minor | Warning1 b29 |
| BMS Major | Fault2 b15 |
| 입력·정류부 중고장 | Fault1 b0~b21 |
| 출력 중고장 | Fault2 b0~b5 |
| 인버터 중고장 | Fault2 b6~b12 |
| 지락(RCD) 1단계 / 2단계 | Fault2 b16 / b17 |
| 배터리 컨버터 중고장 | Fault1 b22~b29 |
| BMS Level 3 | BAT Status b9 |
| 화재 | Fault2 b18 |
| ESD | Fault2 b13 |
| (단계 없음) MBS C/B Fail | Fault2 b14 |

- 여러 조건이 동시에 걸리면 **높은 단계 순서로 모두** 보여 줍니다. 대표 단계는 가장 높은 단계입니다.
- **복귀 (v2.38):** PMS 안 계산에서는 Warning(1~3)과 Fault(4~6) 모두 원인이 해소되면 바로 내립니다. Fault를 운영자 확인까지 붙잡는 일은 EMS가 합니다(v2.4). PMS에는 확인 신호가 오지 않으므로 PMS가 붙잡으면 재시작 전까지 풀리지 않기 때문입니다 (사용자 결정 2026-11-23). 예전의 확인 전 Fault 유지와 재시작 래치(`g_anUpsSafetyLatched`, v2.0)는 없앴습니다.
- **기록 (v2.0, v2.21에서 다시 켬):** 단계·원인의 발생과 해소를 **PMS 이벤트 로그(TwinCAT 이벤트 로거, XAE Error List)에만** 남깁니다. **EMS(pms_log 토픽)로는 보내지 않습니다** (v2.21 사용자 결정. 로거 리스너 `FB_MqttLoggerListener`가 `PmsUps` 클래스를 받지 않음). EMS는 v2.4대로 비트로 단계를 직접 계산합니다. 이벤트 클래스 `PmsUps`, `FB_UpsSafetyLog` (UPS n ↔ BMS n 짝마다 하나). 대응 동작(SQ-05)의 명령·완료·10초 오류도 같은 클래스로 남깁니다 (v2.22, 아래 표 RespCmd·RespDone·RespFail).

  | 이벤트 | 종류 | 문구 |
  |---|---|---|
  | CauseActive | 알람 (원인마다 발생·해소) | UPS n 사고·고장 k단계: 원인. 심각도 1~3 Warning, 4~5 Error, 6 Critical |
  | StageChange | 메시지 | UPS n 사고·고장 단계 a → b |
  | CommLost | 알람 | UPS n 통신장애 / BMS n 통신장애 - 끊긴 동안 사고·고장 판정은 마지막 값 유지 (한 번이라도 통신이 됐던 장비만, v2.27) |
  | RespCmd (v2.22) | 메시지 | UPS n 대응: 동작(명령) 명령 보냄. 그 동작을 처음 보낼 때 1번 (2초마다 다시 보내는 것은 남기지 않음) |
  | RespDone (v2.22) | 메시지 | UPS n 대응: 동작(명령) 완료 (보낸 횟수 n / 이미 그 상태였음) |
  | RespFail (v2.22) | 알람 (Error) | UPS n 대응 오류: 동작(명령) 10초 넘게 안 됨, 계속 보내는 중. 되면 해소 |
  | BypassBlocked (v2.31) | 알람 (Error) | UPS n 대응: 바이패스 이상으로 바이패스 절체 안 함 - 인버터 유지 (바이패스가 정상이 되면 절체). 해소되면 해소 |

  PMS가 켜진 뒤 15초 동안은 기록하지 않습니다 (통신이 붙기 전 값으로 기록하지 않기 위해).
- **통신장애:** 단계와 별도 상태로 띄웁니다. UPS나 BMS 통신이 끊긴 동안에는 그 장비 조건으로 단계를 내리지 않습니다.
- **맵에 없는 조건:** DC 전로 단락, Off-gas, 소화설비는 칸만 둡니다. 배터리 DC 전로 지락 1·2단계는 RCD 1·2단계입니다 (v2.50).
- **DC Link 단락 (세방 회신 2026-11-25):** 전용 비트가 없고 배터리 저전압, 배터리 과전류·과전류 정지, 출력 R·S·T 저전압·과전류·과전류 정지, 입력 휴즈 단선 정지 알람이 뜹니다. **따로 판정하지 않습니다 (v2.53, 사용자 결정).** 세방은 비트만 띄우고 PMS는 뜬 비트마다 정해진 단계·대응을 수행합니다 (1·3·4단계: BBC·정류부 정지, 인버터 바이패스 절체 뒤 정지, 충전 컨택터 열기). 알람이 매번 함께 뜨지 않으므로 조합이나 DC Link 전압으로 7쪽 표의 5단계를 만들지 않습니다.
- **출력 (FB_UpsSafetyStage, UPS n ↔ BMS n 짝마다 하나):**

  | 출력 | 뜻 |
  |---|---|
  | 대표 단계 (0~6) | 걸린 단계 중 가장 높은 것 |
  | 단계 목록 (WORD) | bit n = n단계. 높은 순서로 모두 보여 줄 때 사용 |
  | 원인 비트 (UDINT) | b0 SPD, b1 UPS 경고장, b2 BMS Minor, b3 BMS Major, b4 입력·정류부 중고장, b5 출력 중고장, b6 인버터 중고장, b7 지락(RCD) 1단계, b8 BBC 중고장, b9 지락(RCD) 2단계, b10 BMS Level 3, b11 화재, b12 ESD |
  | UPS·BAT 통신장애 (BOOL) | 단계와 별도 상태 |

- **EMS 표시 (v2.4 결정):** 단계는 **EMS가 `ups`·`bat` 비트로 직접 계산해 표시**합니다. 이 표(조건과 비트)가 EMS에 주는 판정표입니다. PMS 안 계산(`FB_UpsSafetyStage`)은 비교용으로만 남깁니다.
- **지락(RCD) (v2.50, 세방 회신 2026-11-25):** 7쪽 표의 배터리 DC 전로 지락 1·2단계는 STATUS2 b12·b13 (Building Alarm 5·6, RCD 1·2단계)입니다. v2.29의 '지락 아님'(2026-11-11 확인)을 바꿉니다. 단계·대응(1단계 = 3단계, 2단계 = 4단계, BBC 정지)은 그대로입니다.
- **확인 필요:** MBS C/B Fail(I50 b7)은 7쪽 표에 없어서 아직 단계를 정하지 않았습니다.

**시험 기준**
- I51 b10 = 1 → 대표 단계 2, 목록 [2]
- I51 b10 = 1이고 I48 b1 = 1 (인버터 이상 정지) → 대표 단계 3, 목록 [3, 2]
- I46 b9 = 1 (BBC PWM Fail) → 단계 4. 그 비트가 0이 되면 단계 0 (v2.38)
- 입력 정전 (I44 b6 = 1) → 단계 3. 0이 되면 단계 0
- UPS 통신 끊김 → 통신장애 = 1. 마지막 단계 유지
- I51 b8 (AC SPD)만 → 단계 1, 원인은 SPD만 (UPS 경고장 아님)
- I51 b12 (지락 1단계, RCD1) → 3, 해소되면 0. I51 b13 (지락 2단계, RCD2) → 4, 해소되면 0 (v2.38)
- I51 b14 (화재) → 5
- 4단계 중 UPS 통신 끊김 → 4 유지 (마지막 값). 통신 복구 뒤 원인이 없으면 0 (v2.38)
- BMS Level 3 후 BMS 통신 끊김 → BAT 통신장애 = 1, 단계 5 유지

### SQ-02 정전 처리 — 확정

- 입력 정전(I44 b6 Input Phase Loss)은 3단계 Warning으로 알리고 기록을 남깁니다. 기록은 "UPS n 사고·고장 3단계: 입력·정류부 중고장" 알람 (v2.0).
- 복전 후 UPS는 auto boot로 자동 재기동합니다(UPS 설정, O-06).

### SQ-03 PMS 재시작 후 운전 — 확정

- PMS가 다시 켜지면 Manual로 시작하지 않습니다. 저장된 값(CF-UPS-02)을 읽어 자동으로 운전을 이어 갑니다.
- 헬리오스 기본 정책인 `GVL_SITE` `bKeepOnRestart = FALSE`(재시작 시 Manual)와 반대입니다.

**시험 기준:** 저장된 설정이 있는 상태에서 PMS 재시작 → 운영자 조작 없이 CF-UPS-03 복구가 실행됩니다.

**구현 (v0.6):** UPS 설정은 보관값(GVL_PERSIST)을 기준으로 10초마다 비교해 맞추므로, 재시작 후 자동으로 이어집니다. `bKeepOnRestart`는 PCS 유닛 운전 모드용이라 바꾸지 않았습니다(이 현장에는 PCS 없음).

### SQ-05 사고·고장 대응 동작 — 잠정 (v2.16, 재시도 v2.19)

**스위치:** `GVL_PERSIST.g_bUpsSafetyRespEnable` (기본 TRUE (v2.40), PMS를 다시 켜도 유지. 이미 돌던 PLC는 저장된 값 그대로). 장비 쓰기 관문(`DEVICE_WRITE`, `UPS_CONTROL_ENABLED`, 쓰기 허가)도 모두 켜져야 명령이 나갑니다.

**원인 → 동작 (7쪽 표).** 원인은 SQ-01의 지금 걸린 원인(`q_nActiveCause`)입니다. 여러 원인이면 동작을 합칩니다.

| 원인 (SQ-01 원인 비트) | 단계 | 동작 |
|---|---|---|
| b2 BMS Minor | 2 | 충전 컨택터 열기 |
| b3 BMS Major, b7 지락(RCD) 1단계 | 3 | BBC 정지 |
| b4 입력·정류부 중고장 | 3 | 정류부 정지 + 충전 컨택터 열기 |
| b5 출력 중고장, b6 인버터 중고장 | 3 | 바이패스 절체 → 확인되면 인버터 정지 (v2.30). 출력 과전류 R/S/T(I47 b8~10)는 15분 계속될 때 |
| b8 BBC 중고장, b9 지락(RCD) 2단계 | 4 | BBC 정지 |
| b10 BMS Level 3 | 5 | 바이패스 절체 → **확인되면 인버터 정지 (v2.54)**, BBC 정지, 정류부 정지, BMS 컨택터 전부 열기 |
| b11 화재 | 5 | **ESD와 같음 (v2.31)**: 인버터 정지, BBC 정지, 정류부 정지, BMS 컨택터 전부 열기. 바이패스 절체 안 함 |
| b12 ESD | 6 | 인버터 정지, BBC 정지, 정류부 정지, BMS 컨택터 전부 열기. **바이패스 절체는 하지 않음** (v2.18). 다른 원인이 바이패스 절체를 요구해도 ESD가 걸려 있으면 하지 않음 |
| b0 SPD, b1 UPS 경고장 | 1 | 없음 (경보만) |

**세방 맵 Alarm Flag 비트별 동작 (v2.30, 사용자 결정).** 7쪽 표는 중요한 것만 모은 것이고, 맵 Alarm Flag의 정류부·배터리컨버터·인버터 칸에 적힌 동작도 모두 수행합니다. **단계는 새로 만들지 않습니다** (SQ-01 그대로). 대응에는 단계용 원인(`q_nActiveCause`)과 따로 만든 대응 원인(`q_nRespCause`)을 씁니다. 위 표의 동작과 합칩니다.

| 맵 동작 | 맵 비트 | PMS 동작 |
|---|---|---|
| 정류부 정지 | I44·I45 중고장 (아래 '기동 안 됨' 2개 빼고) | 정류부 정지 (+ 7쪽 표의 충전 컨택터 열기) |
| 기동 안 됨 | I44 b7 입력 상회전, I45 b9 Pre Magnet Fail | **명령 없음** (이미 켜지지 않는 상태). 3단계 알람은 그대로 |
| BBC 정지 | I46 중고장 (b4 빼고), I51 b11~13 (BMS Major, 지락(RCD) 1·2) | BBC 정지 |
| 10분 후 정지 | I46 b4 배터리 과전류 | **10분 계속되면** BBC 정지 |
| 인버터 정지 To Bypass | I47 b11~13 출력 과전류 Trip, I48 인버터 중고장 전부, I50 b7 MBS C/B Fail | 바이패스 절체 → **확인되면** 인버터 정지 |
| (화재) 정류부·BBC 정지, 인버터 정지 To Bypass | I51 b14 화재 | **v2.31: ESD와 같음** (바이패스 절체 안 함, 맵과 다름, 사용자 결정) |
| 15분 후 인버터 정지 To Bypass | I47 b8~10 출력 과전류 R/S/T | **15분 계속되면** 바이패스 절체 → 확인되면 인버터 정지 |
| 정류부·BBC·인버터 정지, 출력 차단 | I50 b0 ESD | 그대로 (v2.18). 출력 차단은 하드웨어 |
| 충전 정지 / 방전 가능 | I51 b10 BMS Minor | 충전 컨택터 열기 (그대로) |
| (경고장, 노랑) | 나머지 | 없음 (맵: 모두 정상 운전) |

- **인버터 정지는 바이패스 절체가 확인(BypassStatus b0 = 1)된 뒤에만** 보냅니다. 절체가 안 되면 인버터를 끄지 않습니다(부하 유지). 한 번 시작하면 원인이 사라질 때까지 유지합니다 (사람이 바이패스↔인버터를 바꿔도 다시 싸우지 않음). ESD는 이 규칙 대신 v2.18 규칙.
- **MBS C/B Fail (I50 b7):** 단계 없음(SQ-01 그대로), 대응만 합니다 (`q_nRespCause` b13).
- **시간 지연:** 그 비트가 계속 켜져 있어야 합니다. 중간에 꺼지면 처음부터 다시 셉니다. 대응 원인만 늦추고 단계·알람은 바로 뜹니다. UPS 통신이 끊긴 동안은 마지막 값을 유지하고 시간은 계속 흐릅니다 (비트가 켜진 채 복구되면 그 시간까지 셈).
- **바이패스 이상 (I49, 맵 'To Bypass 절체 안 됨')** 때 처리는 남겨 둡니다 (O-17). 지금은 절체를 계속 보내고 10초 오류, 인버터는 절체 확인 전이라 끄지 않습니다.

**동작 → 명령과 확인 신호**

| 동작 | 명령 | 됐다고 보는 조건 |
|---|---|---|
| 충전 컨택터 열기 | BMS 40014 = 0 | 30026 b0 = 0 (컨택터 상태를 읽고 있을 때) |
| BMS 컨택터 전부 열기 | BMS 40013~40015 = 0 (FC16, BAT 2와 같음) | 30026 = 0 그리고 30027 = 0 |
| 바이패스 절체 | UPS H109 To Bypass | BypassStatus b0 (Load On Bypass) = 1 |
| BBC 정지 | UPS H107 BBC OFF | BBCStatus b0 (BBC Mode) = 0 |
| 정류부 정지 | UPS H103 정류부 OFF | RectStatus b0 (Rect Mode) = 0 |
| 인버터 정지 (v2.18 ESD, v2.30 To Bypass 뒤) | UPS H105 인버터 OFF | InverterStatus b0 (인버터 기동, I48 b6) = 0 |

**확인·재시도**
- 먼저 확인해서 이미 그 상태면 명령하지 않습니다 (UPS가 스스로 바이패스로 넘어간 경우 등).
- **계속 보내기 (v2.19, 데스틴 PCS 통신 방식):** 명령 → **2초**(응답 대기 1초 + 쉼 1초) 뒤에도 안 됐으면 다시 명령. **될 때까지 멈추지 않고** 보냅니다.
  - **10초** 넘게 안 되면 그 동작에 오류를 띄웁니다(`q_wFailed`). 오류가 떠도 계속 보내고, 되면 오류는 꺼집니다. 오류를 EMS로 보내는 칸은 두지 않습니다 (PLC 안에서만, v2.19a 사용자 결정). EMS가 단계와 상태 비트로 직접 판단해 띄웁니다. 예: 4단계인데 `BBCStatus` b0 = 1이 10초 넘게 계속 → 'BBC 정지 안 됨'. PMS는 그동안 계속 보냅니다.
  - 드라이버가 못 받은 명령(바쁨·통신 끊김)은 2초를 기다리지 않고 다음 스캔(10 ms)에 다시 시도합니다. 보낸 횟수(`q_anTry`)는 드라이버가 받은 것만 셉니다.
  - 시간 값: `i_tResend` 2초, `i_tFail` 10초 (FB 입력 기본값).
- 된 동작은 원인이 사라졌다가 다시 생길 때까지 다시 하지 않습니다. 사람이 현장에서 다시 켠 것을 PMS가 되돌려 끄지 않기 위해서입니다.
- **(v2.49) 이중화 절체 때도 이 기억을 이어받습니다** (RED-01). 넘겨받은 PLC는 상대가 이미 한 동작을 '됨'으로 두고 다시 보내지 않습니다.
- UPS 명령은 스캔마다 하나씩, 흐름도 순서대로 보냅니다: 바이패스 절체 → 인버터 정지(v2.30, 절체 확인 뒤) → BBC 정지 → 정류부 정지. **앞 동작이 되거나 오류(10초)가 뜨면** 다음 동작을 보냅니다. 오류가 뜬 앞 동작도 계속 보냅니다. BMS는 '전부 열기'가 필요하면 '충전 컨택터 열기'는 따로 하지 않습니다.
- 스위치를 끄거나 쓰기 관문이 닫히면 진행 중인 동작을 모두 멈추고 처음 상태로 돌립니다.
- **ESD (v2.18):** ESD는 UPS가 접점(I50 b0)으로 직접 멈추고, PMS는 백업입니다. 확인은 그대로 해서 UPS가 이미 끈 것은 보내지 않습니다.
- **화재 (v2.31, 사용자 결정):** 화재는 ESD 상황과 같다고 봅니다. ESD와 똑같이 바이패스 절체를 하지 않고(다른 원인이 요구해도), 인버터 → BBC → 정류부를 앞 동작을 기다리지 않고 연달아 보내고, BMS 컨택터를 전부 엽니다. 단계는 5 그대로.
- **바이패스 이상 (v2.31, O-17 해결):** UPS 경고 I49(바이패스 저·고전압, 저·고주파, 정전, 상회전 = Warning1 b16~25)가 하나라도 있고 아직 바이패스가 아니면, 바이패스 절체(H109)를 보내지 않습니다(맵: 'To Bypass 절체 안 됨'). 인버터 정지 To Bypass도 절체가 확인되지 않으니 하지 않습니다(부하 유지). 이 동안 이벤트 로그 알람 `BypassBlocked`를 띄웁니다. 바이패스가 정상으로 돌아오면 그때 절체 → 인버터 정지. 이미 바이패스로 운전 중이면 해당 없음.
- **바이패스 회로 차단 (O-18):** UPS에는 바이패스를 끊는 통신 명령이 없습니다(H108 인버터로 절체, H109 바이패스로 절체뿐). 화재·ESD 때 바이패스까지 여는 출력 완전 차단은 하드웨어(EPO 접점 등)가 필요합니다. → **v2.50 (세방 회신 2026-11-25):** UMS는 ESD를 제어하지 않고 상태만 봅니다. 바이패스 포함 출력 차단 여부는 UPS 자체 ESD 동작 사양입니다. PMS 백업 정지 명령(v2.18)은 바꾸지 않았습니다.
  - 앞 동작이 되거나 오류가 뜨기를 기다리지 않고, 드라이버가 받아 주는 대로 다음 명령을 보냅니다. 순서는 인버터 → BBC → 정류부입니다.
  - 각 동작은 위와 같이 2초마다 계속 보내고, 10초 넘으면 오류를 띄웁니다.

**기록 (v2.22):** 동작마다 처음 보낸 명령, 완료, 10초 오류(알람)를 PMS 이벤트 로그에만 남깁니다 (SQ-01 기록 표 RespCmd·RespDone·RespFail). 2초마다 다시 보내는 것은 남기지 않습니다. 동작 이름에 명령을 함께 적습니다 (예: "BBC 정지(H107)", "충전 컨택터 열기(BMS 40014=0)"). EMS로는 보내지 않습니다.

**ESS 공통 코어 보호 규칙 (v2.36, 사용자 결정):** `GVL_PERSIST.g_astRuleCfg` 1~5번(PCS 고장, PCS 통신 끊김, BMS 고장, BMS 통신 끊김, 접속점 계측 무효)은 **조치 없음, 알람만**입니다. 예전에는 3번 'BMS 고장'(BMS 드라이버 bFault: System Error·Module Error·Error Code·화재 중 하나)이 BMS 컨택터를 전부 열었습니다(40013~15 = 0). 배터리를 UPS에서 떼는 일은 이 SQ-05만 합니다. PERSISTENT 값이라 이미 돌던 PLC에는 새 기본값이 들어가지 않으니, 온라인으로 1~5번 조치를 None으로 바꾸거나 보관값을 초기화해야 합니다.

**하지 않는 것:** 복귀(다시 켜기)는 사람이 합니다. 2단계(BMS Minor)·3단계(BMS Major 등)로 연 충전 컨택터도 원인이 풀린 뒤 사람이 닫습니다 (v2.26 사용자 결정). ESD 때 바이패스 절체는 하지 않습니다 (v2.18). MCCB Trip은 명령이 없어(세방 3차 Q8) 하지 않습니다. UPS가 Trip한 결과만 MCCB_Status b6으로 봅니다 (v2.50). 바이패스까지 끊는 출력 완전 차단(6단계)은 통신 명령이 없어 ESD 하드웨어(사람) 몫입니다. BMS E-stop(40012)은 쓰지 않습니다. Level 3은 3.1/3.2를 나누지 않습니다.

**시험 기준**
- 스위치 OFF, BMS Minor → 명령 없음
- 스위치 ON, BMS Minor, 충전 컨택터 닫힘 → BMS 40014 = 0 요청 1회. 2초 안에 30026 b0 = 0 → 성공, 더 보내지 않음
- 인버터 중고장인데 이미 Load On Bypass = 1, 인버터도 꺼짐 → 명령 없이 성공 (v2.30: 인버터가 켜져 있으면 H105)
- BMS Major, BBC Mode가 계속 1 → H107을 2초마다 계속 보냄. 10초 뒤 오류 표시, 그 뒤에도 계속. BBC Mode = 0이 되면 성공, 오류 꺼짐, 더 보내지 않음 (v2.19)
- ~~화재, 바이패스가 확인되지 않음 → 바이패스 오류가 뜬 뒤 H107 → H103이 나감. H109도 계속 보냄 (v2.19)~~ v2.31에서 화재는 H109 없음
- 화재 (v2.31) → H105, H107, H103을 기다리지 않고 연달아 (H109 없음), BMS 컨택터 전부 열기 1회 (충전만 열기는 따로 없음)
- 성공 뒤 사람이 BBC를 다시 켜도 원인이 그대로면 다시 끄지 않음. 원인이 사라졌다가 다시 생기면 다시 함
- ESD, 인버터·BBC·정류부 모두 켜짐, 상태 변화 없음 → 3스캔 안에 H105 → H107 → H103 (앞 결과를 기다리지 않음). H109 없음. BMS 전부 열기 1회 (v2.18)
- ESD와 화재가 함께 → H109 없음 (v2.18)
- 기록 (v2.22): BMS Major, BBC Mode 1 → "BBC 정지(H107) 명령 보냄" 1번 (다시 보내도 더 안 남김). 10초 넘으면 대응 오류 알람. BBC Mode = 0 → "완료" 1번, 오류 알람 해소
- 기록 (v2.22): 이미 Load On Bypass = 1 → 명령 기록 없이 "바이패스 절체(H109) 완료 (이미 그 상태였음)"
- ESD, UPS가 이미 인버터·BBC·정류부를 껐음 → UPS 명령 없이 성공 (v2.18)
- ESD, 드라이버가 바쁨 → 횟수 0 유지, 받으면 그때 1회 (v2.18). v2.19부터 ESD가 아니어도 같음
- (v2.30) 인버터 중고장, 인버터 켜짐, 바이패스 아직 → H109만 (H105 없음). 바이패스 확인 → H105. 인버터 꺼짐 → 완료 (바이패스·인버터 정지 둘 다 됨)
- (v2.30) 인버터 중고장, 바이패스가 끝내 확인 안 됨 → 10초 뒤 바이패스 오류, H105는 보내지 않음
- (v2.31) 화재와 인버터 중고장이 함께 → H109 없음, H105 → H107 → H103
- (v2.31) 바이패스 이상(Warning1 b16) + 인버터 중고장, 인버터 켜짐 → H109·H105 없음, BypassBlocked = 1. 바이패스 이상 해소 → H109 → 확인 → H105, BypassBlocked = 0
- (v2.31) 바이패스 이상 + 이미 Load On Bypass → BypassBlocked = 0 (해당 없음)
- (v2.31) 바이패스 이상 + ESD → BypassBlocked = 0 (ESD는 원래 절체 안 함)
- (v2.30) MBS C/B Fail (I50 b7, Fault2 b14)만 → 단계 0, 대응 원인 b13, H109 → 바이패스 확인 → H105
- (v2.30) 입력 상회전 (I44 b7, Fault1 b7)만 → 단계 3, 대응 원인에 b4 없음 → 명령 없음. Pre Magnet Fail (I45 b9, Fault1 b18)도 같음
- (v2.30) 출력 과전류 R (I47 b8, Fault2 b0) → 15분 전에는 대응 원인 b5 없음, 15분 계속 → b5 (H109). 출력 과전류 Trip R (I47 b11, Fault2 b3) → 바로 b5
- (v2.30) 배터리 과전류 (I46 b4, Fault1 b23) → 10분 전에는 대응 원인 b8 없음, 10분 계속 → b8 (H107). 배터리 종지전압 정지 (Fault1 b22) → 바로 b8
- (v2.30) 인버터 정지 완료 뒤 사람이 인버터를 다시 켬, 원인 그대로 → 다시 끄지 않음. 원인이 사라졌다가 다시 생기면 다시 함. 스위치를 끄면 해제
- (v2.30) 출력 중고장 대응 원인 b5 → H109
- (v2.54) BMS Level 3, 인버터·BBC·정류부 켜짐, 바이패스 아직 → H109만 (H105 없음), BMS 전부 열기 필요. 바이패스 확인 → H105

## 4-2. 이중화 (v2.43, 2026-11-23 확정)

PMS 제어기 2대(수집장치, 3차년도 구입)를 **프라이머리·세컨더리**로 씁니다. 평소에는 프라이머리가 동작하고(코어 상태 Active), 세컨더리는 대기합니다(Standby). 프라이머리가 죽으면 세컨더리가 넘겨받아 동작하고, 프라이머리가 돌아오면 다시 넘겨줍니다(코어 Handover). 아래 '동작 중인 쪽' = 지금 Active인 PLC입니다. 코어 이중화(`10_Core/90_Redundancy`, `PRG_Redundancy`)를 씁니다. **장비가 오기 전까지 `GVL_SITE.RED_SET.bUse = FALSE`** 그대로 둡니다.

### RED-01 설정 동기화 — 잠정 (v2.49)

- 동작 중인 쪽이 운영 설정을 매 스캔 EAP 변수 하나(`ST_RedSyncCfg`)로 보내고, 대기 중인 쪽은 상대가 동작 중이고 통신이 정상일 때 **다른 값만** 덮어씁니다 (코어 `FB_RedSync`).
- **UPS 항목 추가 (v2.43):** UPS 설정 보관값(`g_astUpsDesired`: EMS 설정 12칸(v2.51부터 14칸), 온도보상·간헐충전 켜짐)과 사고 대응 스위치(`g_bUpsSafetyRespEnable`). 형식 번호(`nProtoVer`) 1 → 2: 두 PLC 프로그램이 다르면 받은 값을 낡은 것으로 보고 쓰지 않습니다.
- 받은 값으로 설정을 바꾸면 바로 PERSISTENT 저장을 요청합니다 (CF-UPS-02와 같은 '바로 저장').
- BMS 랙 선택표(`g_awBmsRackMask`)는 동기화하지 않습니다. 랙 명령은 BMS의 실제 랙 컨택터 상태(Input 30027)를 바탕으로 쓰기 때문입니다 (WR-BAT-01 v2.13).
- 모듈 설정 묶음(`g_stModuleSet`)은 코어 규칙대로 동기화하지 않습니다 (HMI가 두 PLC에 씀).
- **사고 대응 '된 동작' 기억 (v2.49, 사용자 결정):** 동작 중인 쪽이 UPS마다 된 동작 비트(`FB_UpsSafetyResponse.q_wDone`: b0 충전 컨택터 열기, b1 BMS 컨택터 전부 열기, b2 바이패스 절체, b3 BBC 정지, b4 정류부 정지, b5 인버터 정지)를 함께 보냅니다. 대기 중인 쪽은 상대가 동작 중이고 통신이 정상일 때 받은 값을 기억해 둡니다 (설정이 아니므로 저장·동기화 횟수에 넣지 않음).
  - 이 PLC가 동작 중이 되는 순간(절체·복귀) 기억한 비트를 이어받습니다. 그 동작의 원인이 새로 읽은 값으로 다시 잡히면 '됨'으로 두고 명령을 보내지 않습니다. 인버터 정지(b5)를 이어받으면 바이패스 확인을 기다리지 않습니다.
  - 이어받은 비트는 장비 쓰기가 허가되고 UPS를 읽기 시작한 뒤 3초 동안 원인이 안 잡히면 버립니다. 어떤 경우든 동작 중이 된 뒤 30초가 지나면 버립니다 (옛 기억이 나중에 새 고장을 막지 않게).
  - 판정 값(SQ-01)은 계속 넘기지 않습니다 (v2.45). 과전류 지연 타이머(배터리 10분, 출력 15분)도 넘기지 않습니다 (절체 뒤 처음부터 셈).
  - 형식 번호 2 → 3. 동기화 변수 크기가 바뀌므로 XAE의 EAP 변수(RedSyncCfg, Publisher·Subscriber)의 크기도 같이 바꿉니다.
- **크기 고정 (v2.51):** UPS 보관값이 14칸으로 늘어 동기화 변수가 480 B를 넘습니다. 동기화 변수를 **512 B로 고정**하고 끝에 남는 자리(`abSpare`)를 둡니다. 다음에 칸이 늘어도 512 B 안이면 XAE EAP 변수를 다시 바꾸지 않습니다. 이번에는 XAE의 RedSyncCfg(Publisher·Subscriber)를 `ARRAY [0..511] OF BYTE`로 한 번 바꾸고 두 PLC에 같이 올립니다. 형식 번호 3 → 4.

**시험 기준 (시험 PLC 2대 필요):** 프라이머리(동작 중) 보관값 H50 = 120, 세컨더리(대기) = 100 → 세컨더리 120으로 바뀌고 동기화 횟수 +1, 바로 저장 요청 / 같은 값 → 덮어쓰지 않음 / 프라이머리 스위치 FALSE → 세컨더리도 FALSE / 상대가 형식 번호 1(옛 프로그램) → 받은 값 낡음, 덮어쓰지 않음

**시험 기준 (v2.49, 된 동작 이어받기)**
- 프라이머리: 인버터 중고장(원인 b6) → 바이패스 절체 됨 → 사람이 인버터로 되돌림(원인 그대로). 프라이머리 정지 → 세컨더리가 넘겨받음 → 바이패스 절체 명령을 보내지 않음 (q_wDone b2 = 1)
- 이어받은 비트가 있어도 넘겨받은 뒤 원인이 없음(3초) → 버림. 그 뒤 원인이 생기면 → 명령을 보냄
- 이어받을 기억이 없음(상대가 아무것도 안 함) → 지금처럼 원인대로 명령

### RED-02 대기 중인 쪽 장비 읽기 — 확정 (v2.52, 사용자 결정. v2.44·v2.45를 바꿈)

- **두 PLC 모두 장비를 읽습니다. 장비에 쓰는 것은 동작 중(Active)인 쪽만입니다.** PMS_STD 코어의 기본 방식으로 돌아갑니다. 대기 중인 쪽도 늘 최신 값을 갖고 있어 절체 순간 통신 공백(예전 1~2초 통신에러)이 없습니다.
- 대상: UPS, BMS, IMD, 온습도계 2종, 원격 I/O, 지락계전기, ASD-57 전부. 지금 RS-485(RTU) 장비는 실제로 쓰지 않습니다 (2026-11-25 사용자 확인). RS-485 장비를 두 PLC가 같은 줄로 읽게 되면 마스터가 둘이 되므로 그때 다시 정합니다.
- 쓰기 막기: UPS·BMS 드라이버의 쓰기 허가, UPS 설정·명령·사고 대응은 모두 `bWriteEnable`(설정 정상 AND `g_bRedWrite` AND `DEVICE_WRITE`)에 묶여 있습니다. `g_bRedWrite`는 Active가 된 뒤 쓰기 대기(1초)가 지나야 켜집니다.
- **대기 중에도 사고·고장 판정(SQ-01)을 합니다.** 표시·기록만 하고 명령은 쓰기 막기로 나가지 않습니다. v2.45의 '대기 중 판정 비우기'는 없앱니다. 넘겨받으면 이미 읽던 값으로 바로 대응하고, 상대가 이미 한 동작은 v2.49 기억으로 다시 보내지 않습니다.
- **대기 중에도 장비 통신장애 알람**(PmsUps CommLost, 보호 규칙 4 'BMS 통신 끊김')을 띄웁니다. 자기 접속이 끊긴 것이기 때문입니다. v2.45의 '대기 중 알람 끄기'는 없앱니다.
- 장비가 같은 포트에 TCP 접속 2개를 동시에 받아야 합니다 (O-20). 시뮬레이터는 받습니다.

**시험 기준:** bUse = TRUE, 대기 중 → 설치된 장비 i_bEnable = 선언값 그대로(읽음), UPS·BMS 쓰기 없음 / 대기 중 인버터 고장 → 3단계 판정·표시, 명령 없음 / 넘겨받음 → 쓰기 대기 1초 뒤 대응 명령 (상대가 이미 한 동작은 보내지 않음) / 대기 중 UPS 통신 끊김 → CommLost 알람

### RED-03 상대 감시·절체 — 확정 (코어)

- 두 길로 봅니다: 네트워크 하트비트(EAP)와 하드와이어 접점(EL1004 DI·EL2004 DO, ch1 살아 있음, ch2 동작 중). 두 PLC 배선은 서로 엇갈립니다 (A EL2004 ch1·2 → B EL1004 ch1·2, 반대도).
- **접점은 신호 두 개(살아 있음, Active)만 나릅니다. 설정 값은 접점으로 보낼 수 없고 네트워크(EAP, RED-01)로 보냅니다.**
- 두 길이 모두 끊기면 대기 중인 쪽(평소 세컨더리)이 동작합니다. 둘 다 동작 중으로 보이면 세컨더리가 물러납니다 (프라이머리 우선). 장비가 오면 XAE에서 EtherCAT 단자를 스캔하고 링크합니다.

- **역할 못 정함 알람 (v2.56):** 이중화를 쓰는데(`RED_SET.bUse`) 역할이 10초 넘게 Reading이면 설정 점검(`FB_ConfigCheck`, PlcTask)이 설정 오류 "RED_SET: 10초 넘게 역할을 못 정함"을 띄우고 이벤트 로그에 남깁니다. Reading 동안에는 MQTT가 접속하지 않고(ClientId의 -P/-S를 못 정함) 장비에 쓰지도 않으므로, 원인(Net ID 읽기 `PRG_Redundancy.fbGetNetId` 오류, RedundancyTask 정지)을 바로 알 수 있게 합니다. RedundancyTask가 멈춰도 알람이 나오도록 PlcTask에서 봅니다.

**시험 기준 (v2.56):** bUse = TRUE, 역할 Reading 9초 → 알람 없음 / 10초 넘음 → 설정 오류 1건 / 역할이 Primary가 되면 바로 없어짐 / bUse = FALSE → 알람 없음

### RED-04 대기 중 동작 점검 — 예정

- 대기 중인 쪽에서 UPS 설정 다시 쓰기(CF-UPS-03), UPS·BAT 명령, 사고 대응(SQ-05)이 쓰기 막힘 때문에 실패·거부·알람으로 세지지 않는지, 절체 직후 새로 동작하는 쪽이 이어서 하는지 확인합니다.

### RED-05 접점·MQTT — 예정

- 연기 접점(TS-60)과 나중에 넣을 Minor·Major·ESD 접점은 두 PLC에 모두 배선합니다.
- 두 PLC 모두 EMS 명령을 구독합니다. 버전 2에는 SequenceNumber가 없으므로, 절체 직후 중복 명령을 방지할 별도 정책은 아직 정하지 않았습니다.

## 5. 시험 환경

| 항목 | 내용 |
|---|---|
| 시험 묶음 (TcUnit) | `tests\` 아래 ReadMapping(보고 매핑), Command(UPS·BAT 명령), ConfigRecover(설정 복구), Safety(사고·고장 단계·대응·기록), AuxDevice(보조 장비). 입력은 맵 원시값을 직접 넣어 결과 확인 |
| 사고·고장 시뮬레이터 | `tests\sil\safety_stage_gui.py`: UPS(포트 2323)·BMS(1502) 가짜 장비. 단계별 고장 비트 넣기, PMS 명령을 받아 상태 바꾸기, 기대 동작과 비교, BMS 악화 자동(2 → 3 → 5단계, 7초씩) |
| 시험 EMS 화면 | `tests\mqtt\ems_test` (http://127.0.0.1:8080): MQTT로 받은 ups·bat·rack·module 보기, EMS 방식 단계 계산, UPS 설정·UPS 명령·BAT 명령 보내기 |
| 검증 | Claude: 정적 검사·사본 빌드·시험 빌드 (`명세작업\검증기준.md`). 사람: 로컬 런타임·시험 PLC 실행 (2026-11-16 결정) |

## 5-1. 앞으로 추가할 장치 읽기 — 예정

출처: LiB-UPS 통합안전관리시스템 보도참고자료(2026-11-05, 대건소프트). 이 자료는 과제의 **최대 범위**를 보여 주는 것이고, 지금 원칙(P-01~P-03)은 그대로 둡니다. 우선 **장비 읽기만** 추가합니다.

| ID | 장치 | 연결 | 쓰임 | 필요한 것 |
|---|---|---|---|---|
| DV-10 | 온습도계 | Modbus RTU (EL6022) | 배터리실 온습도 → CF-PRT-01 습도 항목의 측정원 | 장비 맵 |
| DV-11 | IMD (절연 감시) | Modbus TCP | 절연·지락 → SQ-01 지락 조건 보강 | 장비 맵 |
| DV-12 | Off-gas 감지기 | 미정 | SQ-01 4단계 Off-gas 조건 | 장비 맵 |
| DV-13 | 화재감지기 | 접점 | SQ-01 5단계 화재 조건 | 입력 점 |
| DV-14 | 연기감지기 | RS-485 | SQ-01 5단계 조건 | 장비 맵 |
| DV-15 | Minor·Major·ESD 접점 | 접점 입력 | BMS Minor·Major·ESD를 PMS가 직접 받음 (지금은 UPS STATUS2로만 읽음) | 입력 점 |
| DV-16 | 공조기 (공조·냉방) | 미정 | 운전 상태·설정 온도·실내 온도·고장 읽기. 배터리실 온도 관리 상태 확인 | 장비 맵 |

**선행 구현 (v1.4, 2026-11-09):** 맵이 없어 Helios에서 쓰던 기종으로 읽기 드라이버를 먼저 넣었다. 모두 보조 장비(`I_AuxDevice`, `GVL_SITE.g_aipAux`)이고 `i_bEnable` FALSE(통신 안 함)다. 실제 기종이 정해지면 드라이버를 바꾸거나 주소만 고친다.

| ID | 드라이버 | Helios 기반 | 점 | 남은 것 |
|---|---|---|---|---|
| DV-10 | `FB_AutonicsThd` (오토닉스 THD) | `FB_ThdAutonics_Chain` | AI 2n-1 온도, 2n 습도, DI n 센서 통신 이상 | 기종 확정, EL6022 채널 I/O 연결 |
| DV-10 | `FB_ItGuardIeSx` (ITGuard IE-SX, TCP) — v1.9 | MESSIS `FB_IE_SX` | AI 1 온도, 2 습도 (IR 1·2 ×0.01), DI 1 프로브 값 이상(32767). 보고 토픽 `temperature_sensor` | location 확정 |
| DV-11 | `FB_LsImd1000` (LS IMD1000, RS485-TCP 게이트웨이) — v1.8 | MESSIS `FB_LsIMD1000` + LS 맵 | AI 1 절연저항, 2·3 경보 설정값. DI 1 경고, 2 절연 경보(Ins. Alarm), 3 시험 중, 4 사전경고, 5 기기 고장, 6 계통·7 PE 연결 이상. EMS 보고는 RD-IMD-01 | 게이트웨이 IP·국번, 32비트 워드 순서, 경보 릴레이를 DI로도 받을지 |
| DV-13·14 | `FB_RootechAcuraIo` (루텍 ACURA3700 원격 I/O) | `FB_ACURA3700` | DI 1..8 접점, AI 1..6 | IP, 감지기 결선(Helios는 DI로 화재(열)·연기·가스). 연기감지기가 RS-485 장비면 별도 드라이버 |
| DV-12 | 없음 | 없음 (Helios에도 Off-gas 장비 드라이버 없음, 가스는 접점만) | | 장비 맵 |
| DV-17 | `FB_WoonyoungGfr` (운영 WYGF-D08/12/16NC 지락검출계전기, RTU EL6022) — v2.6 | 없음 (신규, 맵 `프로토콜 맵 v3\지락검출 계전기.pdf`) | AI n 회로 n 누설전류 A, DI n 회로 n 지락, DI 17 시험 모드. **AC 회로만** (ZCT 방식, 배터리 DC 전로 지락은 IMD) | 감시할 AC 회로·회로 수. **2026-11-10 현장 시험:** 통신·IR 0 상태(278, 시험 모드 b8)는 확인, Circuit Test 때 IR 1 지락 비트는 0 (실제 누설 시험 또는 제조사 확인 필요). 보고 `보고\2026-11-10_v2.6.md` |
| DV-18 | 드라이버 없음: `FB_AuxIo` `g_fbIo1` (벡호프 EL1xxx DI) + `FB_RuleBit` `g_fbSmoke1` — v2.7 | 매뉴얼 `프로토콜 맵 v3\TS-60_매뉴얼.pdf` (가스디엔에이 TS-60, 통신 없음, 릴레이 A접점 2개) | DI 1 = 연기 접점 (흰 NO·초록 COM, 경보 때 닫힘). 결선: 빨강 +24V, 검정 GND, 노랑·파랑 온도 접점(안 씀). 보호 칸 6: 조치 None = **알람만** (사고·고장 단계는 EMS) | XAE에서 `g_fbIo1._abDi[1]`을 DI 단자 채널에 링크. 연기 시험(시험 스프레이)으로 DI·알람 확인. 온도 경보 설정 DIP (매뉴얼 출하값 50℃/65℃ 서로 다름) |
| DV-19 | `FB_GasdnaAsd57` (가스디엔에이 ASD-57, RTU EL6022) — v2.8 | 없음 (신규, 맵 `프로토콜 맵 v3\ASD-5x(모드 버스 프로토콜)_ver1.1(22_0922).pdf`, 결선 `ASD-57_매뉴얼.pdf`) | FC3 HR 0~8. AI 1 연기1 %, 2 연기2, 3 온도 ℃. DI 1 연기 알람, 2 온도 알람 (HR 3: 1 연기, 2 온도). 설정(알람값·딜레이·국번)은 읽기만. 결선: 빨강 +24V, 검정 0V, 초록 FG, 흰 TX+, 파랑 TX-, 주황·갈색·노랑 릴레이(안 씀) | EL6022 채널 9600·8N1·half duplex (지락계전기 19200과 다른 채널). 2026-11-10 PC 프로그램으로 HR 읽힘 확인 (온도 25.2 ℃, 연기 시험 때 HR 3 = 1). PLC에서도 HR 0~8 읽힘 확인. 보고 `보고\2026-11-10_v2.8.md` |

- 보호 판정(`FB_RuleBit`·`FB_RuleValue` 등록)은 아직 하지 않았다. 등록 예는 `GVL_SITE` "보조 장비·규칙 예" 주석에 있다. 등록하면 `RULE_COUNT`와 `GVL_PERSIST.g_astRuleCfg`(PERSISTENT) 크기가 바뀐다.
- EMS 보고(MQTT)는 인터페이스를 정한 뒤 한다. IMD는 v1.8에 넣음 (RD-IMD-01).

**최대 범위 (지금은 하지 않음):** 보호동작 미수행 시 PMS 백업 제어(UPS·BMS 명령), 연동 설비(공조·냉방·소화) **제어**(공조기는 읽기만 DV-16), AI 이상탐지 결과를 단계 판정 입력으로 받기, 열화상 카메라(AI 서버 쪽).

**자료와 다른 점:** 자료에는 배터리 세트당 10랙으로 되어 있습니다. 도면 기준 판단은 8랙이며, BAT 질의 Q1로 확인 중입니다(DV-03).

## 5-2. 2026년(3차년도) 과제 요구 — 연구개발계획서에서 옮김

출처: 연구개발계획서(3세부, 대건소프트 3차년도 개발 목표·내용·성능지표). PMS(PLC)에서 할 것만 요구사항으로 두고, 서버 쪽 일은 아래 표에 적어 둡니다.

| ID | 상태 | 요구사항 | 근거 (계획서) |
|---|---|---|---|
| PF-01 | 예정 | **데이터 갱신 ≤ 1초**: UPS·BMS 값이 바뀌고 EMS(MQTT)에 반영되기까지 1초 이하 | 성능지표 14 (3차년도 ≤ 1 s, 공인 시험성적서) |
| PF-02 | 예정 | **데이터 취득 ≤ 1초**: 장비 읽기 주기 1초 이하. 랙 상세처럼 나눠 읽는 데이터도 포함해 확인 | 성능지표 15 |
| PF-03 | 예정 | **데이터 취득률 ≥ 99.8 %**: 읽기 성공 횟수와 실패 횟수를 세어 취득률을 계산·보고 | 성능지표 16 |
| PF-04 | 예정 | 통합안전관리 프로그램 신뢰도 ≥ 90 % (다음 해 ≥ 95 %) | 성능지표 17 (연차 칸 확인 필요) |
| TM-01 | 예정 | **시간 동기화**: PLC 시계를 GPS(또는 NTP) 기준에 맞추고, MQTT 헤더 TimeStamp가 그 시계를 따름 | 데이터 처리 모듈 "GPS 시간동기화 장치로 시간 동기화" |
| SQ-04 | 예정 | **동작 시퀀스 정의·편집**: 단계 판정 조건과 단계별 동작을 사용자 설정으로 바꿀 수 있게 함. PLC(고정 판정)와 서버(REST 사고 판단·편집) 중 어디서 할지 정해야 함 | 동작 시퀀스 개발 "사용자 설정 입력에 따른 알고리즘 편집", "시퀀스 수정·편집" |
| TS-01 | 예정 | **재현성 시험 — 가상 데이터 입력**: CSV 등 정상·비정상·비상 UPS·BMS 가상 데이터를 PMS에 넣어 실제 장비 없이 운전·판정을 재현 | 재현성 시험 "CSV 가상 데이터 입력·운영 모듈" |
| TS-02 | 예정 | **시퀀스 단위·연계 시험**: 결함 유형·심각도별 시퀀스 단위 시험과 영역 간 연계 시험 (TcUnit 32개가 출발점) | 재현성 시험 "유효성 검증" |
| DP-01 | 예정 | **실증 설치**: 제주글로벌연구센터 사고 모의 시뮬레이터와 연계해 설치·시운전, 과제 종료까지 운영 | 실증설비 구축방안 (3차년도) |

**대건소프트 서버 쪽 일 (PMS 범위 밖, 참고)**

| 과제 | 내용 |
|---|---|
| 데이터 처리 모듈 | 전처리(누락 보완·이상치 제거·형식 변환), 실시간 스트림, 고장·이벤트·통계 배치, 정형·비정형 이상 감지, 초·분 단위와 이벤트 저장 |
| 메모리 기반 DB·DB 인터페이스 | MongoDB capped / time-series collection, 샤딩, RAID, 인덱싱·쿼리 최적화, RESTful API |
| 사고 판단 인터페이스 | REST API 기반 정상·비정상·비상 사고 판단 인터페이스 (SQ-04와 나눌 범위 확인) |

## 6. 범위 밖

- **PCS·계량기·운전 모드 (v2.42, 사용자 결정):** 이 현장에는 PCS가 들어가지 않습니다. ESS 공통 코어의 PCS(Plaspo)·계량기는 등록하지 않아 통신하지 않고, 운전 모드 5개(정출력·스케줄·피크컷·통합·스탠다드 스케줄)도 등록하지 않아 HMI에서 골라도 거절됩니다(BadSlot). 코드(FB와 설정)는 지우지 않고 남겨 둡니다.
- 실제 장비 통신 타이밍과 연결 확인 (현장 확인)
- EMS 화면과 백엔드

## 7. 미결 사항

| 번호 | 내용 | 관련 요구사항 | 기다리는 답 |
|---|---|---|---|
| O-01 | 랙·모듈 수 → **답변 (2026-11-10): 8랙, 9모듈, 14셀.** v2.2에 반영 | DV-03 | 해결 |
| O-02 | 랙 상세에서 Lock 1·2만 표기하는지 | RD-BAT-01 | BAT Q5 |
| O-03 | Status b8~b10 추가 여부 | RD-UPS-01 | 차장님 / UPS Q5 |
| O-04 | BAT 명령 레지스터 연결 → **답변 (2026-11-10):** System Contactor(40013)가 랙 DC 컨택터 전체를 동작. 충전·방전 컨택터(40014·40015)는 평소 동시 투입, 충전 차단 때만 충전 컨택터 개방. 랙 개별·일괄 제어 모두 구현 권장. BMS 시각 동기화(HR 40005~40007) 1시간 또는 하루 1회 권장. Lock 2 b10~15 의미는 업체 확인 중(Q2). **v2.3: 투입 3개·시각 동기화 코드 반영** | WR-BAT-01 | EMS 명령 비트 협의(랙 개별·일괄), Q2 |
| O-05 | 재시작 후 설정값을 다시 써야 하는지 (CF-UPS-03으로 코드에서 보장, 답은 참고용) | CF-UPS-03 | UPS Q1 |
| O-06 | Auto ON(H112)이 복전 후 자동 재기동인지, 시운전 때 켜 두면 되는지 → **일부 확인 (2026-11-16, UPS 보드 시험):** Auto ON(명령 13)을 보내면 정류부·BBC·인버터가 한 번에 기동한다. 복전 후 자동 재기동 여부는 아직 모름 | SQ-02 | 세방전기 (3차 Q9) |
| O-07 | PMS 제어기 전원이 UPS 출력에서 나오는지 | SQ-03 | 현장 배선 |
| O-09 | **일부 해결 (2026-11-25, v2.50, 세방 회신):** 배터리 DC 전로 지락 1·2단계 = RCD 1·2단계 (I51 b12·b13). DC Link 단락은 전용 비트 없음 → 따로 판정 안 하고 비트별 단계로 대응 (v2.53). 남은 것: DC 전로 단락 신호 위치, MBS C/B Fail의 단계 | SQ-01 | 세방전기 / 사용자 |
| O-10 | ~~사고·고장 단계를 EMS에 보낼 칸과 Fault 확인 명령 번호~~ → **해결: 단계는 EMS가 비트로 직접 계산(v2.4), 확인도 EMS(v2.38, WR-SQ-01 폐기). 보낼 칸·명령 없음** | SQ-01, WR-SQ-01 | 해결 |
| O-11 | UPS 설정 레지스터 스케일: 맵 기본값(H10 100 "0.1A", H58 504 "0.1V", H62 2.22 등)이 원시값인지 공학값인지 → **일부 확인 (2026-11-16, UPS 보드):** 맵 기본값은 공학값이고 레지스터는 ×10 (H1 3800 = 380.0 V, H58 5100 = 510.0 V). H62는 222 = 2.22 V라 0.01 V로 보임(맵 표기 0.1V와 다름). H50·H52·H53은 맵 "0.1A"인데 보드 화면은 "kW"로 표시 | CF-UPS-01·03 | 세방전기 (3차 Q10, H50 단위) |
| O-12 | EMS 설정 구조체(config/ups)에 충전 전류(H10) 칸 추가 → **해결 (2026-11-25, v2.51): ChargeSetCurrentLimit(H10)·FloatChargeSetVoltage(H7) 추가 (구글 시트 101.Config Project UPS_V1)** | CF-UPS-01 | 해결 |
| O-13 | EMS가 command/ups 레코드의 DeviceIndex를 1~3으로 보내는지 (config/ups와 같다고 가정) | WR-UPS-01 | EMS 협의 |
| O-14 | 간헐충전 SOC 기준(H57 = 2)일 때 H58·H59 SOC 단위 (1 % 또는 0.1 %). 맵 설명은 "SOC : 70 %". 보드 화면에서 SOC로 바꿔 읽어 확인 예정. 지금 기본 1 % (`UPS_CFG_SOC_SCALE`) | CF-UPS-01 | UPS 보드 확인 / 세방전기 |
| O-08 | UPS가 BMS를 차단한 결과(컨택터 실제 상태)를 확인할 항목이 있는지 | SQ-01 | 이솔루션 (다시 물을지 결정 필요) |
| O-15 | 정전 뒤 충전 컨택터 재투입: 입력 정전(I44 b6)과 다른 입력 이상 비트(I44)가 원인 b4(입력·정류부 중고장)에 들어가서, SQ-05가 충전 컨택터를 연다. 정전 중에는 방전 컨택터가 따로 있어 문제가 없지만, 복전 뒤 PMS가 다시 닫지 않는다(복귀는 사람). 정전이 날 때마다 사람이 닫을 때까지 충전되지 않는다. 안: A 지금 그대로(사람이 닫음), B 정전으로 연 것만 복전 후 PMS가 다시 닫음, C 정전(I44)은 경보만, 충전 차단은 정류부 고장(I45)일 때만 | SQ-05, SQ-02 | 해결 (2026-11-19, v2.26): A 지금 그대로. 정전 등으로 연 충전 컨택터도 사람이 닫음. 2단계 Minor도 같음 |
| O-16 | 배터리 eventMap 확인 → **일부 정함 (2026-11-23, v2.32)**: 사용자 v1 파일에서 System Alarm = 경고, Module Status·Error Code·Self Test 다시 넣음. 이솔루션 최종 확인은 남음 | RD-BAT-01·02 | 이솔루션 (Q3 Lock 2 b10~15·Q4 Fire b15 답변 받음 2026-11-23) |
| O-17 | 바이패스 이상(I49)일 때 '인버터 정지 To Bypass' 처리 → **해결 (2026-11-20, v2.31): 절체 안 보내고 알림, 인버터 유지. 화재는 ESD처럼** | SQ-05 | 해결 |
| O-18 | 화재·ESD 때 바이패스까지 여는 출력 완전 차단 수단 → **해결 (2026-11-25, v2.50, 세방 회신): UMS는 ESD를 제어하지 않고 상태만 봄. 바이패스 포함 출력 차단은 UPS 자체 ESD 사양** | SQ-05 | 해결 |
| O-19 | BAT 명령 4 초기화(40031 Reset) → **해결 (2026-11-23, v2.34): A 원격 초기화 허용**, 바로 쓰는 경로 | WR-BAT-01 | 해결 |
| O-20 | 이중화: UPS·BMS가 같은 포트에 Modbus TCP 접속 2개를 동시에 받는지 → **다시 열림 (2026-11-25, v2.52: 두 PLC 모두 읽음)**. 시뮬레이터는 받음. (v2.44에서 Standby 접속 안 함으로 닫았던 것) | RED-02 | 세방전기 / 이솔루션 |
