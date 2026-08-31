# Robot Arm Control System

ESP32 기반 다관절 로봇팔 제어 프로젝트입니다.

PCA9685 PWM 드라이버를 이용해 여러 개의 서보모터를 제어하고,
각 관절의 보정값과 동작 범위를 설정하여 안정적인 로봇팔 동작을 구현했습니다.

단순 관절 제어부터 XYZ 좌표 기반 제어, 부드러운 궤적 생성,
Wi-Fi 기반 웹 제어 및 중앙 서버 연동까지 단계적으로 구현했습니다.

---

## 주요 기능

- 다축 서보모터 제어
- 서보 중립 위치 및 조립 오차 보정
- 관절별 각도 제한 및 안전 범위 설정
- 개별 관절 각도 제어
- XYZ 좌표 기반 로봇팔 제어
- 사다리꼴 속도 프로파일 기반 부드러운 동작
- ESP32 Access Point 기반 웹 제어
- Wi-Fi STA 모드 기반 중앙 서버 연동
- 로봇팔 ID를 이용한 다중 로봇팔 확장 구조

---

## Hardware

- ESP32
- PCA9685 16-Channel PWM Driver
- Servo Motors
  - Base
  - Shoulder
  - Elbow
  - Wrist Rotation
  - Wrist Pitch
  - Gripper
- External Servo Power Supply

---

## Software

- MicroPython
- Thonny
- Python
- Wi-Fi
- Socket Communication
- HTTP / Web Interface

---

## System Structure

User / Web Interface
        ↓
ESP32 Wi-Fi Communication
        ↓
Motion / Position Command
        ↓
Joint Angle Calculation
        ↓
Servo Calibration & Limit
        ↓
PCA9685 PWM Driver
        ↓
6-Axis Servo Robot Arm

---

## Servo Control

로봇팔은 총 6개의 서보 축으로 구성되어 있습니다.

| Joint | Function |
|------|------|
| Base | 로봇팔 좌우 회전 |
| Shoulder | 어깨 관절 |
| Elbow | 팔꿈치 관절 |
| Wrist Rotation | 손목 회전 |
| Wrist Pitch | 손목 상하 |
| Gripper | 물체 파지 |

각 서보모터의 조립 오차를 보정하기 위해 개별 offset 값을 적용하였으며,
최소/최대 각도를 제한하여 기구적인 충돌 및 과도한 회전을 방지했습니다.

---

## Motion Control

### 1. Neutral Position Calibration

각 서보모터의 중립 위치를 설정하고 조립 상태에 따라 offset을 적용하여
로봇팔의 기준 자세를 보정했습니다.

### 2. Joint Angle Control

각 관절의 목표 각도를 입력하여 개별적으로 서보모터를 제어할 수 있도록 구현했습니다.

### 3. XYZ Position Control

단순 관절각 제어를 넘어 목표 XYZ 좌표를 기반으로 로봇팔의 자세를 결정하도록
제어 구조를 확장했습니다.

### 4. Smooth Motion Control

관절을 목표 위치로 즉시 이동시키는 방식에서 발생하는 급격한 움직임을 줄이기 위해
사다리꼴 속도 프로파일(Trapezoidal Motion Profile)을 적용했습니다.

이를 통해 가속-등속-감속 형태의 부드러운 관절 움직임을 구현했습니다.

---

## Wi-Fi Control

### AP Mode

ESP32가 직접 Wi-Fi Access Point를 생성하도록 구성하여
별도의 공유기 없이 스마트폰 또는 PC에서 로봇팔에 접속할 수 있도록 구현했습니다.

웹 인터페이스를 통해 로봇팔의 각 관절 및 동작 명령을 전달할 수 있습니다.

### STA Mode

ESP32를 기존 Wi-Fi 네트워크에 접속시키는 STA 모드도 구현했습니다.

중앙 서버의 IP와 Port에 로봇팔을 등록하고,
각 로봇팔에 고유한 Robot ID를 부여하는 구조를 사용했습니다.

이를 통해 여러 로봇팔을 하나의 중앙 서버에서 제어할 수 있도록 확장 가능한 구조를 구현했습니다.

---

## Development Process

프로젝트는 기능을 단계적으로 확장하는 방식으로 진행했습니다.

1. Servo Neutral Position 설정
2. Servo Calibration
3. Joint Angle Test
4. Potentiometer 기반 제어
5. XYZ Coordinate Control
6. Smooth Motion Control
7. ESP32 AP Web Control
8. Wi-Fi STA / Central Server Communication

---

## What I Learned

- ESP32 기반 임베디드 시스템 제어
- I2C 통신을 이용한 PCA9685 제어
- PWM 기반 서보모터 제어
- 다관절 로봇팔 좌표 및 관절 제어
- Servo Calibration과 기구 오차 보정
- Motion Profile 기반 속도 및 가속도 제어
- ESP32 Wi-Fi AP / STA 통신
- Socket 기반 네트워크 통신
- 하드웨어와 소프트웨어를 결합한 시스템 설계
