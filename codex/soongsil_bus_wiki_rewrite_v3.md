# Soongsil SoC/FPGA bus wiki rewrite v3

## 수정 배경

이전 버전은 활동 흐름을 보여 주는 데는 괜찮았지만, 사용자가 지적한 것처럼 기술 위키로 보기에는 허술했다. 특히 “실제 bus가 무엇인지”, “APB/AHB/AXI가 서로 어떻게 다른지”, “RTL에서 어떤 조건식과 FSM으로 구현했는지”가 충분히 드러나지 않았다.

이번 수정은 사이트 글을 활동 일지형 문장보다 기술 설명형 문장으로 바꾸는 작업이다. 각 페이지를 다음 구조에 가깝게 재작성했다.

1. 개념 정의
2. 주요 신호와 역할
3. transfer sequence
4. Lab RTL 구현 방식
5. waveform/log 검증 포인트

## 반영한 코드/자료

- `LAB/Lab2/RTL/Src/Lab2_ApbIfBlk.v`
  - `PSEL/PENABLE/PWRITE/PADDR` 조건으로 write/read enable 생성
  - Lab2 register map: 입력 A, 입력 B, 출력 C
- `LAB/Lab3_1/RTL/Src/Lab3_1_ApbIfBlk.v`
  - APB access를 SP-SRAM access로 변환
  - `PADDR[3:2]` 기반 SRAM entry 선택
- `LAB/Lab3_2/RTL/Src/Lab3_2_ApbIfBlk.v`
  - `0x0000` start command, `0x0004` packet word size
  - InBuf/OutBuf 기반 endian conversion 흐름
- `LAB/Lab5/RTL/Src/Lab5_AhbIfBlk.v`
  - HREADYin 기준 address phase latch
  - `rHSEL/rHTRANS/rHWRITE/rHADDR` 기반 data phase access
- `LAB/교수 Lab6/RTL/Src/Lab6_AhbIfBlk.v`
  - `HADDR[31:4] == 28'h7000400` SRAM address range decode
  - zero-wait write, one-wait read, HREADY 제어
- `LAB/Lab7 교수/RTL/Src/Lab7_AxiIfBlk.v`
  - AW/W/B, AR/R handshake 기반 AXI slave register access
  - `0x7000_0000`, `0x7000_0004`, `0x7000_0008` register map
- `최종 결과/Axi2Apb.v`
  - write FSM: `xW_Idle`, `xW_AwReady`, `xW_WValid`, `xW_Setup`, `xW_Enable`, `xW_BValid`
  - read FSM: `xR_Idle`, `xR_ArReady`, `xR_Setup`, `xR_Enable`, `xR_RValid`
  - AW/W/AR READY pulse timing
- `최종 결과/ApbSlave.v`
  - 4-register APB target
  - PREADY 1-cycle pulse
  - `PADDR[5:2]` 기반 read mux/write commit
- `최종 결과/TbTop_Prj_Axi.v`
  - task 기반 AXI master model
  - AWVALID 2-cycle hold, WVALID 3-cycle hold, ARVALID 2-cycle hold
- `최종 결과/Prj2.log`
  - `PASS: 4REG write/read + timing OK`

## 페이지별 변경 내용

### APB

- 실제 bus와 bus protocol의 의미를 먼저 설명했다.
- APB를 주변장치 register 제어용 bus로 정의하고, PSEL/PENABLE/PREADY/PADDR/PWDATA/PRDATA 신호 표를 추가했다.
- APB transfer sequence를 idle, setup, enable, ready로 설명했다.
- Lab2/Lab3_1/Lab3_2/Lab4 RTL 구현 흐름을 추가했다.

### AHB

- AHB를 APB보다 빠른 system bus로 설명했다.
- address phase와 data phase가 분리되는 이유를 중심으로 재작성했다.
- HREADY, HRESP, HTRANS, HSEL 등 주요 신호 표를 추가했다.
- Lab5 address phase latch와 Lab6 SRAM one-wait read 구현을 코드 구조로 설명했다.

### AXI

- AXI를 5-channel bus로 정의하고, AW/W/B/AR/R channel 표를 추가했다.
- VALID/READY handshake 규칙을 별도 section으로 설명했다.
- Lab7 AXI slave register block의 write/read decode 흐름을 코드 형태로 설명했다.

### AXI-to-APB Bridge

- Bridge를 protocol conversion block으로 정의했다.
- 최종 프로젝트 사양과 block별 역할을 표로 정리했다.
- write FSM/read FSM을 실제 state 이름으로 설명했다.
- READY pulse와 APB slave의 PREADY/write commit/read mux 구현을 추가했다.
- testbench task와 최종 PASS 로그를 설명했다.

## 검증

- HTML local link/image/script/css reference 검사 통과.
- UTF-8 replacement character 0개 확인.
- 핵심 키워드 반영 확인: `실제 버스`, `PREADY`, `HREADY`, `VALID/READY`, `READY pulse`, `Axi2Apb`, `PASS: 4REG`.
