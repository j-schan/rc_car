# HSI — Hardware Software Interface (핀 정의서)

- 프로젝트: `rc_car` / MCU: STM32F446RET6 (LQFP64) / 보드: NUCLEO-F446RE
- 작성일: 2026-07-05 · CubeMX 설정과 동기화 유지할 것

---

## Phase 1 — 활성 (현재 사용 중)

| 핀 | 신호 | AF/모드 | 방향 | 보드 라벨 | 연결 대상 | 설명 |
|----|------|---------|:---:|:---------:|-----------|------|
| PA6 | TIM3_CH1 | PWM 1kHz | 출력 | D12 (CN5) | L298N ENA | 모터L 속도 (듀티 0~999) |
| PA7 | TIM3_CH2 | PWM 1kHz | 출력 | D11 (CN5) | L298N ENB | 모터R 속도 |
| PB0 | IN1 | GPIO_Output | 출력 | A3 (CN8) | L298N IN1 | 모터L 방향 |
| PB1 | IN2 | GPIO_Output | 출력 | CN10 (D7 맞은편) | L298N IN2 | 모터L 방향 |
| PB2 | IN3 | GPIO_Output | 출력 | CN10 (D8 맞은편) | L298N IN3 | 모터R 방향 |
| PB4 | IN4 | GPIO_Output | 출력 | D5 (CN9) | L298N IN4 | 모터R 방향 |
| PA0 | TIM2_CH1 | PWM 50Hz | 출력 | A0 (CN8) | 서보 신호선 | 조향 (1100~1900µs, 중립 1500) |
| PA9 | USART1_TX | AF7 | 출력 | D8 (CN5) | HC-05 **RXD** | BT 송신 (★엇갈림) |
| PA10 | USART1_RX | AF7 | 입력 | D2 (CN9) | HC-05 **TXD** | BT 수신, 인터럽트 |
| PA2 | USART2_TX | AF7 | 출력 | (ST-Link 내부) | PC 가상COM | 디버그 115200 |
| PA3 | USART2_RX | AF7 | 입력 | (ST-Link 내부) | PC 가상COM | 디버그 |
| PA5 | LD2 | GPIO_Output | 출력 | D13 | 보드 LED | 상태 표시 |
| PC13 | B1 | EXTI | 입력 | — | 보드 버튼 | 사용자 버튼 |
| PA13/PA14 | SWDIO/SWCLK | SWD | — | — | ST-Link | 디버거 (예약, 사용 금지) |

## Phase 2 — 예약 (엔코더·센서)

| 핀 | 신호 | 모드 | 연결 대상 | 설명 |
|----|------|------|-----------|------|
| PB6 / PB7 | TIM4_CH1/CH2 | Encoder Mode | 모터L 엔코더 A/B | 1320 cnt/rev |
| PC6 / PC7 | TIM8_CH1/CH2 | Encoder Mode | 모터R 엔코더 A/B | |
| PB8 / PB9 | I2C1 SCL/SDA | I2C | IMU·OLED 등 | 공유 버스 |
| PC0~PC2 | GPIO_Output | — | 초음파 Trig ×3 | 전방/좌/우 |
| PC3~PC5 | EXTI | — | 초음파 Echo ×3 | 펄스폭 측정 |

## Phase 3+ — 예약 (컴패니언·확장)

| 핀 | 신호 | 연결 대상 | 설명 |
|----|------|-----------|------|
| PB10 / PB11 | USART3 TX/RX | 라즈베리파이 5 | cmd_vel/odom, ROS2 링크 ★ |
| PC10 / PC11 | UART4 TX/RX | LiDAR/GPS 예비 | |
| PA1 | TIM2_CH2 | 예비 서보 | 카메라 짐벌 등 |
| PB12~PB15 | SPI2 | nRF24/SD 등 예비 | |
| **PA11 / PA12** | **CAN1 RX/TX (AF9)** | SN65HVD230 트랜시버 | 미니 SDV 버스, 500kbps |

## 자유 예비 핀
PA15 · PB5 · PC8 · PC9 · PD2 (라인센서 어레이 / 추가 GPIO)

---

## 전원·GND 인터페이스

| 넷 | 값 | 공급원 | 소비처 |
|----|-----|--------|--------|
| VMOT | 11.1V | 3S 18650 | L298N → 모터 ×2 |
| VSERVO | 7.4V | 2S 18650 | LD-1501MG |
| 5V | 5V | USB(개발) / L298N 5V출력(주행) | 보드, HC-05 |
| 3V3 | 3.3V | 보드 레귤레이터 | 엔코더(P2), I2C 센서 |
| **GND** | 0V | — | **전체 공통 (단일 접지)** — 3S−, 2S−, L298N, 서보, HC-05, 보드 |

## 변경 이력
| 날짜 | 내용 |
|------|------|
| 2026-07-05 | 최초 작성 (P1 배선 직전 기준) |
