---
title: 자율주행 시스템 아키텍처 개요
date: 2026-01-15 13:00:00 +0900
categories: [Project]
tags: [자율주행, 아키텍처, perception, planning, control]
---

## 🚗 자율주행 시스템 구성

자율주행 시스템은 크게 **인지(Perception)**, **판단(Planning)**, **제어(Control)** 세 가지 모듈로 구성됩니다.

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Perception │───▶│  Planning   │───▶│   Control   │
│  (인지)      │    │  (판단)      │    │   (제어)     │
└─────────────┘    └─────────────┘    └─────────────┘
       ▲                                      │
       │                                      ▼
   [센서]                              [차량 액추에이터]
```

---

## 1. 인지 (Perception)

센서 데이터를 기반으로 주변 환경을 이해합니다.

### 주요 센서
- **Camera**: 객체 인식, 차선 인식
- **LiDAR**: 3D 포인트 클라우드, 거리 측정
- **Radar**: 속도 측정, 악천후 대응

### 주요 태스크
- Object Detection
- Object Tracking
- Lane Detection
- Sensor Fusion

---

## 2. 판단 (Planning)

인지된 정보를 바탕으로 경로를 계획합니다.

### 계층 구조
1. **Route Planning** - 출발지에서 목적지까지의 전역 경로
2. **Behavior Planning** - 차선 변경, 추월 등 행동 결정
3. **Motion Planning** - 구체적인 궤적 생성

---

## 3. 제어 (Control)

계획된 궤적을 추종하도록 차량을 제어합니다.

### 주요 알고리즘
- **PID Control**
- **MPC (Model Predictive Control)**
- **Stanley Controller**
- **Pure Pursuit**

---

다음 포스트에서는 각 모듈에 대해 더 자세히 다루겠습니다.
