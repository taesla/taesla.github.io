---
title: "샘플링 기반 실시간 경로 생성 알고리즘 개발"
date: 2024-03-01 09:00:00 +0900
categories: [Project, Planning]
tags: [자율주행, 경로계획, Hybrid A*, C++, ROS, CUDA, Path Planning]
image:
  path: https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200
  alt: 자율주행 경로 계획 이미지
pin: false
---

## 📋 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **기간** | 2024.03 ~ 2024.11 |
| **발주기관** | (주)SUM |
| **역할** | 실무 책임자 |
| **참여인원** | 2명 |

---

## 🎯 목표

실시간으로 인지되는 **Occupancy Grid Map**을 기반으로, 자율주행차가 **동적/정적 장애물**을 모두 회피하면서 물리적으로 주행 가능한 경로를 생성하는 **Hybrid A*** 알고리즘 개발

---

## 🔍 문제 분석

### 기존 방식의 한계

기존 Voronoi Field 방식의 경로 탐색은 다음과 같은 문제가 있었습니다:

- ❌ 경로 탐색 루프 내 과도한 연산 부하
- ❌ 목표 실시간 성능(20Hz) 달성 불가
- ❌ 동적 객체(보행자) 회피 미지원

```
기존 처리 속도: 5~10Hz
목표 처리 속도: 16~20Hz (실시간)
```

---

## 💡 솔루션

### 1. 아키텍처 개선 - Boundary Map 전처리

복잡한 충돌 회피 계산을 경로 탐색과 분리하는 아키텍처를 독자적으로 설계했습니다.

```
[LiDAR Raw Data]
       ↓
[누적 연속 맵 생성]
       ↓
[베이지안 확률 필터 → 신뢰도 기반 맵]
       ↓
[라이다 방사형 탐지 → Boundary Map]
       ↓
[Hybrid A* 경로 탐색]
```

> 💡 **핵심 아이디어**: Hybrid A*가 경로 탐색에만 집중하도록 전처리 단계 분리
{: .prompt-tip }

### 2. Hybrid A* 알고리즘 구현

- 차량 동역학 모델(Bicycle Model) 고려
- 격자 기반 + 연속 공간 탐색의 장점 결합
- Reeds-Shepp 경로로 후진 주차 지원

### 3. 동적 객체 회피

```python
# 동적 객체 예측 (등속 모델)
def predict_pedestrian_path(current_pos, velocity, time_horizon):
    predicted_positions = []
    for t in range(time_horizon):
        future_pos = current_pos + velocity * t
        predicted_positions.append(future_pos)
    return predicted_positions
```

- LiDAR 기반 보행자 마커 필터링 및 위치 추적
- 등속 모델 기반 보행자 미래 경로 예측
- Hybrid A* 휴리스틱에 동적 객체 예측 경로 통합

### 4. C++ 포팅 및 최적화

Python 프로토타입을 C++로 직접 포팅하고 최적화:

```cpp
// 우선순위 큐를 사용한 효율적인 노드 탐색
std::priority_queue<Node, std::vector<Node>, NodeComparator> open_set;

// KD-트리를 통한 장애물 경계 최소거리 연산 최적화
KDTree obstacle_tree(boundary_points);
double min_distance = obstacle_tree.nearest_neighbor(query_point);
```

- `std::priority_queue`를 사용한 효율적인 노드 탐색
- KD-트리를 통한 장애물 경계 최소거리 연산 최적화
- CUDA 프로그래밍으로 연산 병목 구간 GPU 가속

---

## 📊 결과

### 성능 향상

| 지표 | 개선 전 | 개선 후 | 향상률 |
|------|---------|---------|--------|
| 처리 속도 | 5~10Hz | 16~20Hz | **2배** |
| 실시간성 | ❌ | ✅ | - |
| 동적 객체 회피 | ❌ | ✅ | - |

### 시나리오별 검증

- ✅ **도심 환경**: 동적 객체 회피 + 교차로 통과 성공
- ✅ **주차장 환경**: Reeds-Shepp 경로로 후진 주차 가능
- ✅ **고속 주행**: 20Hz 실시간 처리 검증 완료

---

## 🛠️ 기술 스택

```
Languages: C++, Python
Frameworks: ROS
Libraries: CUDA, OpenCV
Algorithms: Hybrid A*, Reeds-Shepp, KD-Tree
```

---

## 📚 배운 점

1. **아키텍처 설계의 중요성**: 알고리즘 자체의 최적화보다 전체 시스템 구조 개선이 더 큰 성능 향상을 가져옴
2. **C++ 최적화 역량**: STL 자료구조 선택과 메모리 관리가 실시간 성능에 미치는 영향
3. **실차 적용 관점**: 연산 속도도 안전과 직결되는 중요한 요소

---

## 🔗 관련 링크

- [Hybrid A* Reference](https://github.com/karlkurzer/path_planner)
- [Reeds-Shepp 경로 논문](https://projecteuclid.org/journals/pacific-journal-of-mathematics/volume-145/issue-2/Optimal-paths-for-a-car-that-goes-both-forwards-and/pjm/1102645450.full)
