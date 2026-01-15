---
layout: page
title: Portfolio
icon: fas fa-briefcase
order: 2
permalink: /portfolio/
---

## 🚗 프로젝트 타임라인

자율주행 시스템의 **판단(Planning)**, **제어(Control)**, **시스템 통합(Integration)** 분야에서 수행한 주요 프로젝트입니다.

---

### 2024

#### 🛣️ 샘플링 기반 실시간 경로 생성 알고리즘 개발
**기간**: 2024.03 ~ 2024.11  
**발주**: (주)SUM  
**역할**: 실무 책임자

동적/정적 장애물을 실시간으로 회피하는 Hybrid A* 기반 경로 계획 알고리즘 개발

- Boundary Map 전처리 모듈 독자 설계로 아키텍처 개선
- Python → C++ 포팅 및 CUDA 최적화
- **성과**: 처리 속도 2배 향상 (5~10Hz → 16~20Hz)

`C++` `Python` `ROS` `CUDA` `Hybrid A*`

[상세 보기 →](/posts/path-planning-hybrid-astar/)

---

### 2023

#### ⚙️ e-코너 모듈 차량 제어 분배 알고리즘 개발
**기간**: 2022.12 ~ 2023.11  
**발주**: 현대엔지비  
**역할**: 제어 분배(Control Allocation) 로직 담당

4륜 독립 조향/구동/제동(4WIS/4WID/4WIB) 차량의 최적 제어 분배 알고리즘 개발

- FEM(Fixed Efficiency Matrix) 방식: 연산 효율성 중점
- DEM(Dynamic Efficiency Matrix) 방식: 타이어 마찰 한계 고려
- MATLAB/Simulink + CarSim 연동 시뮬레이션 검증
- **성과**: 코너 주행 시 Yaw Rate 오차 35% 감소

`MATLAB` `Simulink` `CarSim` `Vehicle Dynamics`

[상세 보기 →](/posts/control-allocation-e-corner/)

---

### 2022

#### 🏆 2022 국제 대학생 창작 자동차 경진대회 (동상)
**일시**: 2022.11.04  
**주최**: 한국교통안전공단, 한국자동차안전학회  
**역할**: 인지-판단 파이프라인 개발

사전 정보 없는 트랙에서 실시간 자율주행 시스템 구현

- LiDAR-Camera 센서 퓨전 및 캘리브레이션
- YOLOv4 기반 객체 탐지 + 포인트 클라우드 클러스터링
- ROS 기반 인지-판단-제어 시스템 통합
- **성과**: 동상 수상, 2회 연속 트랙 완주

`ROS` `Python` `C++` `YOLOv4` `LiDAR` `Camera`

[상세 보기 →](/posts/autonomous-driving-competition/)

---

## 📚 연구 실적

### 논문

| 연도 | 제목 | 게재지 | 구분 |
|------|------|--------|------|
| 2024 | A Tube-Based Model Predictive Control for Path Tracking of Autonomous Articulated Vehicle | Actuators, Vol. 13, 164 | SCIE (1저자) |
| 2025 | 후륜 조향을 활용한 멀미 저감을 위한 차량의 요 방향 모션 제어 | 석사 졸업논문 | - |

### 학술대회 발표

| 연도 | 제목 | 학회 |
|------|------|------|
| 2024 | 후륜 조향을 활용한 멀미 저감을 위한 요 모션 제어 기술 연구 | 한국자동차공학회 (KSAE) 추계학술대회 |
| 2023 | 4륜 독립 조향/구동/제동 차량을 위한 타이어 마찰 특성을 고려한 연산 효율적 제어 분배 알고리즘 | 한국자동차안전학회 추계학술대회 |

---

## 🛠️ 기술 스택

### Languages
![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat-square&logo=mathworks&logoColor=white)

### Frameworks & Tools
![ROS](https://img.shields.io/badge/ROS-22314E?style=flat-square&logo=ros&logoColor=white)
![Simulink](https://img.shields.io/badge/Simulink-0076A8?style=flat-square&logo=mathworks&logoColor=white)
![CarSim](https://img.shields.io/badge/CarSim-FF6B35?style=flat-square)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)

### Domains
`Path Planning` `Motion Control` `Sensor Fusion` `Vehicle Dynamics` `SLAM`
