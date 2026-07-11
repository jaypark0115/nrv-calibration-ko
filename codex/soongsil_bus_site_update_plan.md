# 숭실대 SoC/FPGA Bus 사이트 보강 계획

## 목표
- `jay-tech-notes` 사이트의 SoC/FPGA Bus 학습 노트를 공식 참고 링크 중심이 아니라, 사용자가 실제 수행한 숭실대 학부생연구인턴 자료 중심으로 다시 구성한다.
- 처음 보는 사람이 APB/AHB/AXI/AXI-to-APB Bridge의 차이를 이해하면서도, 각 개념이 어떤 Lab과 파형 검증으로 이어졌는지 볼 수 있게 한다.
- 기존 첫 페이지는 개념별 입구 역할만 유지하고, 상세 설명은 개별 페이지에서 다룬다.

## 수정 원칙
1. 참고자료 목록 제거
   - 각 페이지 하단의 Arm 공식 링크 목록을 삭제한다.
   - 대신 숭실대 수업자료, Lab 코드, 사용자가 찍은 파형/필기 사진, 최종 결과 로그를 근거로 설명한다.

2. 각 개념별 Lab 연결
   - APB: Lab2 APB register access, Lab3-1/3-2 memory-mapped register/SRAM, Lab4 interrupt/mask 확장.
   - AHB: Lab5 AHB register access, Lab6 AHB to SP-SRAM access.
   - AXI: Lab7 AXI slave interface, AW/W/B 및 AR/R channel handshake.
   - Bridge: Project #2 AXI-to-APB protocol conversion, Axi2Apb/ApbSlave/Top/TB, PASS 로그.

3. 이미지 반영
   - APB 페이지: Lab2 APB 파형, Lab3 APB-to-SRAM 파형.
   - AHB 페이지: AHB vs AXI 강의 현장 사진, Lab5 AHB 파형.
   - AXI 페이지: Lab7 AXI 파형.
   - Bridge 페이지: FSM 화이트보드 사진, 최종 write/read 파형.

4. 설명 방식
   - 용어 정의만 나열하지 않는다.
   - "왜 이 bus가 필요한지 -> 수업에서 어떤 개념을 배웠는지 -> Lab에서 어떤 회로/테스트벤치를 만들었는지 -> 파형에서 무엇을 확인했는지" 순서로 쓴다.

## 작업 순서
1. 숭실대 CV 폴더의 dossier, 시간순 기록, PDF 추출본, Lab RTL, Prj2.log, 이미지 자료를 재확인한다.
2. 사이트 assets에 필요한 이미지 파일을 복사한다.
3. `codex/soongsil_bus_source_analysis.md`와 본 계획 파일을 작성한다.
4. `04-apb-bus.html`, `05-ahb-bus.html`, `06-axi-bus.html`, `07-axi-to-apb-bridge.html`을 Lab 중심으로 수정한다.
5. `index.html` 카드 설명을 Lab 기반으로 조금 더 구체화한다.
6. 로컬 링크/이미지 참조 검사를 수행한다.
7. git commit 후 GitHub에 push한다.
