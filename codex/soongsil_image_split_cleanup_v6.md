# Soongsil bus image split cleanup v6

## 작업 목적

숭실대 SoC bus 정리 페이지에 들어간 일부 PDF 추출 이미지가 한 장짜리 슬라이드 캡처처럼 보이거나, 제목/로고/잘린 표 조각이 함께 들어가 가독성이 떨어졌다. 이번 수정에서는 원본 PDF를 다시 확인하고, 필요한 회로/타이밍 정보만 보이도록 이미지를 더 작게 나누어 교체했다.

## 수정한 이미지

- `04-apb-bus.html`
  - 기존 `Lab3-1 APB to SP-SRAM` 한 장 이미지는 표 일부와 block diagram이 섞여 보였다.
  - `soongsil-pdf-apb-lab3-register-map.png`와 `soongsil-pdf-apb-lab3-block.png`로 분리했다.
- `05-ahb-bus.html`
  - 기존 `Lab6 AHB to SP-SRAM timing`은 두 timing diagram이 한 장에 들어가고 위쪽 제목 조각이 남았다.
  - `soongsil-pdf-ahb-lab6-sram-local-timing.png`와 `soongsil-pdf-ahb-lab6-bus-conversion-timing.png`로 분리했다.
- `06-axi-bus.html`
  - `AHB / AXI comparison` 표 이미지는 가로가 잘려서 페이지 안에서 읽기 어려웠다.
  - 본문 설명으로 충분하다고 판단해 해당 figure를 제거했다.
- `07-axi-to-apb-bridge.html`
  - 기존 bridge block 이미지는 오른쪽 APB slave/register 영역이 잘렸다.
  - `soongsil-pdf-bridge-block-main.png`로 전체 interface를 다시 넣고, `soongsil-pdf-bridge-apb-slave.png`로 register bank 부분을 보조 확대했다.

## 추가 정리

- `soongsil-pdf-ahb-soc-topology.png`와 `soongsil-pdf-axi-soc-topology.png`는 슬라이드 제목/로고가 보이지 않도록 다시 crop했다.
- crop 후 `view_image`로 실제 표시 상태를 확인했다.
