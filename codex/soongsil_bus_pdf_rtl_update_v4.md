# 숭실대 SoC/FPGA Bus 사이트 보강 기록 v4

작성일: 2026-07-12

## 목적

이전 버전은 APB, AHB, AXI, AXI-to-APB Bridge의 흐름은 잡았지만, 처음 보는 사람이 보기에는 강의자료의 블록도/타이밍도와 실제 Verilog RTL 구현이 충분히 붙어 있지 않았다.
이번 수정은 사용자의 피드백에 맞춰 단순 활동 기록이 아니라 실제로 공부한 내용을 설명하는 위키형 글에 가깝게 보강하는 것을 목표로 했다.

## 확인한 원본 자료

- `숭실대 학부인턴 bus cv/수업자료/3. ..._03일차.pdf`
  - Lab2 APB register map, block diagram, APB timing diagram 확인
- `숭실대 학부인턴 bus cv/수업자료/4. ..._04일차.pdf`
  - Lab3-1 APB to SP-SRAM, Lab3-2 command/register/buffer 구조 확인
- `숭실대 학부인턴 bus cv/수업자료/6. ..._06~07일차.pdf`
  - SoC bus topology, APB/AHB/AXI protocol summary, Lab5 AHB block/timing 확인
- `숭실대 학부인턴 bus cv/수업자료/7. ..._08일차.pdf`
  - Lab6 AHB to SP-SRAM one-wait read timing 확인
- `숭실대 학부인턴 bus cv/수업자료/8. ..._09일차.pdf`
  - AXI topology, AHB vs AXI 비교, independent channel 설명 확인
- `숭실대 학부인턴 bus cv/수업자료/9. ..._10일차.pdf`
  - Lab7 AXI slave block/timing 확인
- `숭실대 학부인턴 bus cv/수업자료/10. ..._11일차.pdf`
  - AXI-to-APB project block diagram, write timing, read timing, FSM 확인
- `숭실대 학부인턴 bus cv/LAB/**/RTL/Src/*.v`
  - Lab2 APB register decode
  - Lab3-1 APB to SP-SRAM address conversion
  - Lab3-2 command register, InBuf/OutBuf decode
  - Lab5 AHB address phase latch
  - Lab6 AHB one-wait SRAM read
  - Lab7 AXI slave handshake and register access
- `숭실대 학부인턴 bus cv/최종 결과/*.v`
  - `Axi2Apb.v`: write/read FSM, READY pulse, APB output logic
  - `ApbSlave.v`: 4-register APB target, PREADY pulse, read mux/write commit
  - `TbTop_Prj_Axi.v`: AXI master task, write/readback verification
  - `Prj2.log`: `PASS: 4REG write/read + timing OK`

## 추가한 이미지 asset

강의 PDF에서 필요한 슬라이드를 PNG로 렌더링해 `site/assets`에 추가했다.

- `soongsil-pdf-apb-lab2-block.png`
- `soongsil-pdf-apb-lab2-timing.png`
- `soongsil-pdf-apb-lab3-sram.png`
- `soongsil-pdf-apb-lab3-sram-timing.png`
- `soongsil-pdf-ahb-soc-topology.png`
- `soongsil-pdf-bus-protocol-summary.png`
- `soongsil-pdf-ahb-lab5-block.png`
- `soongsil-pdf-ahb-lab5-timing.png`
- `soongsil-pdf-ahb-lab6-sram-timing.png`
- `soongsil-pdf-axi-soc-topology.png`
- `soongsil-pdf-ahb-vs-axi-table.png`
- `soongsil-pdf-axi-independent-channels.png`
- `soongsil-pdf-axi-lab7-block.png`
- `soongsil-pdf-axi-lab7-timing.png`
- `soongsil-pdf-bridge-block.png`
- `soongsil-pdf-bridge-write-timing.png`
- `soongsil-pdf-bridge-read-timing.png`
- `soongsil-pdf-bridge-fsm.png`

## 페이지별 반영 내용

### `04-soongsil-axi-bridge.html`

- 첫 페이지에 RTL을 읽는 기본 기준을 추가했다.
- 조합논리, 순차논리, handshake, FSM을 짧게 정의해 뒤의 APB/AHB/AXI/Bridge 글을 읽기 전에 필요한 관점을 먼저 제시했다.

### `04-apb-bus.html`

- Lab2 APB block diagram과 setup/enable timing diagram을 추가했다.
- Lab2 실제 코드의 `wWrEnInA`, `wWrEnInB`, `oPready` 조건을 코드 조각으로 넣었다.
- Lab3-1 APB to SP-SRAM 구조와 timing conversion 슬라이드를 추가했다.
- Lab3-1의 SRAM 주소 변환, chip select, write/read 조건을 실제 코드 기반으로 설명했다.
- Lab3-2의 start command, packet word size, InBuf/OutBuf decode 코드를 추가했다.

### `05-ahb-bus.html`

- SoC bus topology와 APB/AHB/AXI protocol summary 슬라이드를 추가했다.
- Lab5 AHB register block diagram과 timing diagram을 추가했다.
- Lab5의 address phase latch와 register decode를 실제 코드 형태로 보강했다.
- Lab6 AHB to SP-SRAM timing diagram을 추가했다.
- Lab6의 zero-wait write, one-wait read, `HREADY` 제어 코드를 실제 구현 흐름에 맞춰 정리했다.

### `06-axi-bus.html`

- AXI bus topology, AHB vs AXI 비교, independent channel 슬라이드를 추가했다.
- Lab7 AXI register block diagram과 timing diagram을 추가했다.
- `wAwFire`, `wWFire`, `wBFire`, `wArFire`, `wRFire`처럼 VALID/READY handshake가 실제 RTL에서 어떻게 fire 조건으로 정의되는지 추가했다.
- Lab7의 write/read register access 코드를 더 직접적으로 넣었다.

### `07-axi-to-apb-bridge.html`

- AXI-to-APB project block diagram, write timing, read timing, FSM 슬라이드를 추가했다.
- `Axi2Apb.v`의 write FSM/read FSM state와 next-state 코드를 실제 코드 조각으로 교체했다.
- READY pulse 생성 조건을 pseudo-code가 아니라 실제 `wAwReadyPulse`, `wWReadyPulse`, `wArReadyPulse` 형태로 보강했다.
- APB output logic, `ApbSlave.v`의 PREADY/write commit/read mux, `TbTop_Prj_Axi.v`의 write/read task 사용 흐름을 추가했다.

## 주의한 점

- 강의자료와 실습 코드가 약간 다르게 표현되는 부분은 무리하게 하나로 맞추지 않았다.
- 예를 들어 APB Lab RTL에서는 내부 enable을 `PENABLE=0`인 setup 조건에서 만들고, `PREADY`는 `PSEL & PENABLE`에서 올리는 형태가 보인다. 이 차이는 숨기지 않고 “주소 해석 시점”과 “transfer 완료 시점”을 나누어 설명했다.
- 코드는 전체를 붙이지 않고, 처음 보는 사람이 bus RTL 구조를 이해하는 데 필요한 부분만 잘라 넣었다.
