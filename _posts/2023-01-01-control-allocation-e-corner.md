---
title: "e-코너 모듈 차량 최적 제어 분배 알고리즘 개발"
date: 2023-01-01 09:00:00 +0900
categories: [Project, Control]
tags: [자율주행, 제어, Control Allocation, Vehicle Dynamics, MATLAB, Simulink, CarSim]
image:
  path: https://images.unsplash.com/photo-1549317661-bd32c8ce0db2?w=1200
  alt: 차량 제어 시스템 이미지
pin: false
---

## 📋 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **기간** | 2022.12 ~ 2023.11 |
| **발주기관** | 현대엔지비 |
| **역할** | 제어 분배(Control Allocation) 로직 담당 |
| **참여인원** | 4명 |

---

## 🎯 목표

상위 제어기(Planner)의 목표 거동(가속도, Yaw Rate)을 **e-코너 모듈의 8개 액추에이터**에 **최적으로 분배**하는 하위 제어기 개발

---

## 🔍 시스템 구성

### e-코너 모듈 (4WIS/4WID/4WIB)

```
        ┌─────────────────────────────────┐
        │         e-Corner Module         │
        │                                 │
   FL ──┼── 조향 + 구동/제동              │
   FR ──┼── 조향 + 구동/제동              │
   RL ──┼── 조향 + 구동/제동              │
   RR ──┼── 조향 + 구동/제동              │
        │                                 │
        │   총 8개 독립 액추에이터        │
        └─────────────────────────────────┘
```

- **4WIS**: 4륜 독립 조향 (4-Wheel Independent Steering)
- **4WID**: 4륜 독립 구동 (4-Wheel Independent Drive)
- **4WIB**: 4륜 독립 제동 (4-Wheel Independent Braking)

### 주행 모드

| 모드 | 설명 | 활용 |
|------|------|------|
| Normal | 일반 조향 | 고속 주행 |
| Crab | 횡방향 이동 | 좁은 공간 |
| Pivot | 제자리 회전 | 주차 |

---

## 💡 알고리즘 개발

### 차량 동역학 모델링

평면 운동 방정식을 상태 방정식으로 모델링:

$$
\dot{x} = Ax + Bu
$$

여기서:
- $x$: 상태 벡터 (속도, Yaw Rate 등)
- $u$: 입력 벡터 (각 휠의 조향각, 구동력)

### 제어 분배 문제

상위 제어기 명령 $v$를 8개 액추에이터 입력 $u$로 분배:

$$
v = Bu
$$

> ⚠️ **과적합 시스템**: 입력(8개) > 출력(3개) → 무한히 많은 해 존재
{: .prompt-warning }

---

## 🔧 두 가지 접근 방식

### 1. FEM (Fixed Efficiency Matrix)

**개념**: 입력-출력 관계를 고정 행렬로 표현

```matlab
% Pseudo-inverse 방식
u = B' * inv(B * B') * v
```

**특징**:
- ✅ 연산 속도 매우 빠름 (0.009초/스텝)
- ✅ 구현 단순
- ❌ 타이어 횡력 영향 무시
- ❌ 한계 상황 성능 제한

### 2. DEM (Dynamic Efficiency Matrix)

**개념**: 타이어 마찰 특성을 실시간 반영

```matlab
% 마찰 타원 제약 조건
sqrt((Fx/Fx_max)^2 + (Fy/Fy_max)^2) <= 1
```

**특징**:
- ✅ 타이어 종/횡력 결합(마찰 타원) 실시간 반영
- ✅ Magic Formula + Combined Slip 모델 적용
- ✅ 코너 등 한계상황 성능 우수
- ⚠️ FEM 대비 약간 느림 (0.01초/스텝)

---

## 📊 시뮬레이션 검증

### 테스트 환경

```
Platform: MATLAB/Simulink + CarSim
Scenarios: DLC, 곡선/코너/직선 주행
Metrics: Yaw Rate 오차, Vx/Ax 오차
```

### 결과 비교

| 지표 | FEM | DEM | 개선율 |
|------|-----|-----|--------|
| 연산 속도 | 0.009초 | 0.01초 | - |
| 코너 Yaw Rate 오차 | 기준 | -35% | **35% 감소** |
| 한계 주행 안정성 | 보통 | 우수 | - |

### DLC (Double Lane Change) 테스트

```
     ┌───────────────────────────────────┐
     │    ╱─────╲                       │
     │   ╱       ╲                      │
─────┼──╱         ╲──────────────────────
     │ START      END                   │
     └───────────────────────────────────┘
```

- FEM: 경로 추종 가능, 미세한 흔들림
- DEM: 부드러운 경로 추종, 안정적인 거동

---

## 🧮 핵심 수식

### 타이어 마찰 타원

$$
\left(\frac{F_x}{F_{x,max}}\right)^2 + \left(\frac{F_y}{F_{y,max}}\right)^2 \leq 1
$$

### Magic Formula (Pacejka)

$$
F = D \sin(C \arctan(Bx - E(Bx - \arctan(Bx))))
$$

---

## 🛠️ 기술 스택

```
Tools: MATLAB, Simulink, CarSim
Concepts: Vehicle Dynamics, Control Allocation
Models: Bicycle Model, Magic Formula, Combined Slip
```

---

## 📚 배운 점

1. **차량 동역학 이해**: 타이어 물리적 한계가 제어 성능에 미치는 영향
2. **Trade-off 분석**: 연산 속도 vs 제어 정밀도의 균형점 찾기
3. **시뮬레이션 검증**: MATLAB/Simulink + CarSim 연동 환경 구축 역량

---

## 🔗 관련 발표

- **2023 한국자동차안전학회 추계학술대회**
  - "4륜 독립 조향/구동/제동 차량을 위한 타이어 마찰 특성을 고려한 연산 효율적 제어 분배 알고리즘"
