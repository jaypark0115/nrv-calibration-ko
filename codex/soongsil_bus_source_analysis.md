# 숭실대 SoC/FPGA Bus 자료 분석

## 확인한 원본 자료
- `codex/BUS_INTERN_MASTER_DOSSIER_SINGLE_FILE.txt`
- `codex/BUS_INTERN_시간순_작업전체_상세기록.txt`
- `codex/codex_all_pdf_extracts.txt`
- `codex/RAW_EVIDENCE_ALL_TEXT.txt`
- `수업자료/*.pdf`
- `LAB/Lab*/RTL/Src/*.v`, `LAB/Lab*/TestBench/TbTop/*.v`
- `파형 사진들/*.png`
- `필기 사진/**/*.jpg`
- `최종 결과/Axi2Apb.v`, `ApbSlave.v`, `Prj_Axi_Top.v`, `TbTop_Prj_Axi.v`, `Prj2.log`

## 수업자료에서 확인한 흐름
- 강의계획서 기준 과목은 이론과 실습을 함께 수행하는 동계 학부생연구인턴이며, 목표는 AXI/APB interface를 갖는 HW IP를 Verilog-HDL로 설계 및 검증하는 것이다.
- 12/24에는 APB와 Lab2가 연결된다. APB interface를 갖는 register 구현 및 검증이 핵심이다.
- 12/26에는 Lab3-1, Lab3-2, Lab4가 연결된다. APB 기반 memory-mapped register, SRAM access, endian conversion, interrupt/mask 확장으로 이어진다.
- 12/30~12/31에는 AHB bus topology와 Lab5가 연결된다. AHB register access와 address/data phase, HREADY/HRESP를 다룬다.
- 1/2에는 Lab6가 연결된다. AHB slave가 SP-SRAM에 접근하며 write zero-wait, read one-wait 구조를 확인한다.
- 1/5~1/6에는 AXI bus protocol과 Lab7이 연결된다. AXI channel 분리, VALID/READY handshake, burst read/write를 다룬다.
- 1/7~1/12에는 Project #2로 AXI-to-APB protocol conversion block을 구현한다.

## Lab별 실제 구현 근거
- Lab2: `Lab2_ApbIfBlk.v`에서 PSEL/PENABLE/PWRITE/PADDR 조건으로 register write/read enable을 만들고, `oPready`를 APB enable phase 조건으로 생성한다.
- Lab3-1/3-2: APB access를 SRAM 및 buffer 구조로 확장한다. memory-mapped register가 실제 memory 접근으로 이어지는 단계다.
- Lab4: APB 기반 interrupt enable, pending clear, mask enable 구조를 다룬다.
- Lab5: `Lab5_AhbIfBlk.v`에서 HSEL/HTRANS/HWRITE/HADDR를 address phase에서 latch하고, data phase에서 register write/read를 수행한다.
- Lab6: AHB slave가 SP-SRAM에 접근하며, synchronous SRAM read 특성 때문에 read에는 wait가 필요하다는 점을 다룬다.
- Lab7: `Lab7_AxiIfBlk.v`에서 AW/W/B, AR/R channel을 분리하고, VALID/READY handshake 기반으로 write/read burst를 처리한다.
- 최종 프로젝트: `Axi2Apb.v`에서 AXI write/read FSM을 분리하고 APB setup/enable phase로 변환한다. `ApbSlave.v`는 4개 register를 제공하고, `TbTop_Prj_Axi.v`는 task 기반 write/read 검증을 수행한다.

## 이미지 선별 기준
- `파형 사진들/lab2.png`: APB Lab2 register access 파형. APB 페이지에 사용.
- `파형 사진들/lab3_1.png`: APB-to-SRAM memory-mapped access 파형. APB 확장 실습 설명에 사용.
- `필기 사진/26.1.5/2.jpg`: AHB와 AXI 사용 조건을 비교한 강의 현장 사진. AHB 페이지에 사용.
- `파형 사진들/lab5_wtask.png`: AHB Lab5 register access 파형. AHB 페이지에 사용.
- `파형 사진들/lab7 파형.png`: AXI Lab7 channel handshake 파형. AXI 페이지에 사용.
- `필기 사진/26.1.8/1.jpg`: FSM과 timing diagram을 설명한 화이트보드 사진. Bridge 페이지의 FSM 설명에 사용.
- `최종 결과/write 시 파형.png`, `최종 결과/read 시 파형.png`: 최종 AXI-to-APB write/read 검증 파형. 기존 assets 복사본을 유지해 Bridge 페이지에 사용.

## 사이트 문서화 방향
- APB/AHB/AXI를 단순 약어 설명으로 끝내지 않고, 각각 어떤 Lab으로 구현했는지 함께 쓴다.
- 처음 보는 사람에게 필요한 비유는 유지하되, 문서의 핵심은 실제 구현과 검증에 둔다.
- 최종 Bridge 페이지는 이론 정리보다 `AXI 요청 수신 -> FSM -> APB 제어신호 생성 -> APB slave register access -> AXI response/read data 반환` 흐름을 중심으로 설명한다.
- 참고자료 링크 목록은 제거한다. 내부 근거는 계획/분석 md와 원본 cv 폴더 구조로 남긴다.
