---
title: "2022 국제 대학생 창작 자동차 경진대회 (동상)"
date: 2022-11-04 09:00:00 +0900
categories: [Project, Competition]
tags: [자율주행, ROS, 센서퓨전, LiDAR, Camera, YOLOv4, 경진대회]
image:
  path: https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200
  alt: 자율주행 대회 이미지
pin: false
---

## 📋 대회 개요

| 항목 | 내용 |
|------|------|
| **대회명** | 2022 국제 대학생 창작 자동차 경진대회 |
| **일시** | 2022.11.04 |
| **주최** | 한국교통안전공단, 한국자동차안전학회 |
| **결과** | 🏆 **동상 수상** |

---

## 🎯 미션

> 사전 정보 없는 트랙에서 실시간으로 환경을 인지하고 **2회 연속 주행**하는 자율주행 시스템 구현

### 주행 전략

| 회차 | 목표 | 전략 |
|------|------|------|
| 1회차 | 탐색 주행 | 트랙 맵핑 + 안정적 완주 |
| 2회차 | 고속 주행 | 학습된 경로로 최적화 주행 |

---

## 🚗 시스템 구성

### 하드웨어

```
┌─────────────────────────────────────────────┐
│              ERP-42 플랫폼                  │
├─────────────────────────────────────────────┤
│  LiDAR    : Velodyne VLP-16 (16ch, 360°)   │
│  Camera   : Logitech C920 x2 (듀얼)         │
│  GPS/IMU  : ublox F9P + Xsens MTI-630      │
│  Compute  : Intel NUC                       │
└─────────────────────────────────────────────┘
```

### 소프트웨어 아키텍처

```
┌──────────────────────────────────────────────────┐
│                   ROS Melodic                    │
├────────────┬────────────┬────────────────────────┤
│   인지     │    판단    │        제어            │
│ Perception │  Planning  │       Control          │
├────────────┼────────────┼────────────────────────┤
│ LiDAR 처리 │ 경로 생성  │ Stanley Controller     │
│ Camera 처리│ 장애물 회피│ PID 속도 제어          │
│ 센서 퓨전  │            │                        │
└────────────┴────────────┴────────────────────────┘
        ↓              ↓                ↓
    /perception    /planning        /control
      Topic          Topic            Topic
```

---

## 🔧 담당 개발 내용

### 1. LiDAR-Camera 센서 퓨전

3D 포인트 클라우드와 카메라 RGB 이미지를 결합하여 객체 인식 정확도 향상

```python
# 3D → 2D 투영 변환
def project_lidar_to_camera(points_3d, extrinsic, intrinsic):
    # 외부 파라미터 변환 (LiDAR → Camera 좌표계)
    points_cam = extrinsic @ points_3d
    
    # 내부 파라미터 변환 (Camera → Image 좌표계)
    points_2d = intrinsic @ points_cam
    
    return points_2d / points_2d[2]  # 정규화
```

**캘리브레이션 과정**:
1. 체커보드 기반 3D-2D 좌표계 정렬
2. Multiple Camera(2대) 넓은 시야각 확보
3. 카메라별 LiDAR 데이터 분할 및 개별 캘리브레이션 후 병합

### 2. 하이브리드 인지 파이프라인

딥러닝 + 전통적 CV 기법 결합

```
[Camera Image]          [LiDAR Points]
      ↓                       ↓
  YOLOv4 Detection     Adaptive Clustering
      ↓                       ↓
  2D Bounding Box        3D Cluster
      ↓                       ↓
      └────────┬──────────────┘
               ↓
        Sensor Fusion
               ↓
      [3D Object Detection]
```

| 방식 | 역할 | 특징 |
|------|------|------|
| YOLOv4 | 라바콘 탐지 (2D) | 빠른 실시간 처리 |
| Clustering | 포인트 클러스터링 (3D) | 정확한 거리 정보 |
| Fusion | 결과 통합 | 상호 보완 |

---

## 🎬 결과 영상

### 센서 퓨전 시각화

{% include embed/youtube.html id='qqhyStWpxEU' %}

### 트랙 주행 영상

{% include embed/youtube.html id='8bqNV4dib7g' %}

---

## 📊 성과

### 정량적 성과

| 지표 | 결과 |
|------|------|
| 1회차 주행 | ✅ 완주 |
| 2회차 주행 | ✅ 완주 |
| 최종 순위 | 🏆 동상 |

### 정성적 성과

- ✅ 인지-판단-제어 **Full-Stack** 개발 경험
- ✅ ROS 기반 시스템 통합 역량 확보
- ✅ 실제 하드웨어에서의 디버깅 경험
- ✅ 팀 협업을 통한 모듈 간 인터페이스 설계 경험

---

## 🛠️ 기술 스택

```
Platform: ROS Melodic, Ubuntu 18.04
Languages: Python, C++
Perception: YOLOv4, PCL, OpenCV
Hardware: Velodyne VLP-16, Logitech C920
```

---

## 📚 배운 점

### 1. 시스템 통합의 중요성

> "각자가 만든 최고의 모듈도, 서로 단단하게 연결되지 않으면 전체 시스템은 실패한다"

인지 팀의 라바콘 위치 데이터에 작은 오차라도 있었다면, 경로 계획 모듈은 엉뚱한 길을 만들었을 것입니다.

### 2. 투명한 소통과 책임감

- 모듈의 현재 상태와 한계를 명확히 공유
- 약속된 인터페이스와 성능을 반드시 지켜내기
- 이것이 팀 성공에 기여하는 길

### 3. 실제 환경의 불확실성

시뮬레이션에서 완벽히 동작하던 코드도 실제 환경에서는 예상치 못한 문제 발생
- 조명 변화에 따른 카메라 인식률 저하
- GPS 신호 끊김
- 센서 노이즈

---

## 🔗 관련 링크

- [대회 공식 홈페이지](https://www.kotsa.or.kr/)
- [ERP-42 플랫폼](https://github.com/DGIST-ARTIV/ERP-42)
