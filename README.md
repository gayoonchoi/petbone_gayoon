# petbone_gayoon

# 🐶 슬개지킴이 (Patella Guardian)

반려견의 보행 이미지를 기반으로 슬개골 탈구 가능성을 예측하는 인공지능 서비스입니다.  
YOLOv8 Pose 모델로 keypoint를 추출하고, XGBoost 분류기를 통해 탈구의 중증도(severity)를 예측합니다.

---

## 📌 프로젝트 개요

- **프로젝트명**: 슬개지킴이 (Patella Guardian)
- **수행 기간**: 2025.08.01 ~ 2025.08.08
- **수행 장소**: 강동 새싹캠퍼스
- **사용 목적**: 반려견 슬개골 탈구의 조기 진단 및 건강관리 지원

---

## 🧠 핵심 기능

- 반려견 보행 이미지 기반 keypoint 추출 (YOLO11 Pose)
- keypoint 기반 슬개골 탈구 중증도 예측 (XGBoost)
- PyQt GUI를 통한 자가 진단 앱 제공
- 전/측/후면 데이터 방향 필터링 및 bbox 자동 생성

---

## 🗂️ 주요 파일

| 파일명 | 설명 |
|--------|------|
| `gy_pets_keypoint.ipynb` | Keypoint 추출 및 시각화 |
| `sp_PetBone_gayoon_supervison.ipynb` | 슬개골 탈구 예측 모델 학습 |
| `PetBone_gayoon.ipynb` | 전체 파이프라인 통합 |
| `app_ui_powerpoint.py` | PyQt 기반 예측 앱 구현 |
| `[최가윤2] 반려견 슬개지킴이.pptx` | 프로젝트 발표 자료 |

---

## 🧾 데이터 구성

- **AI Hub 반려견 건강관리 데이터셋**
  - 12개 keypoint 포함 JSON (측면/정면/후면)
- **StanfordExtra Dataset**
  - 24개 keypoint 포함, 일반화 목적 전이학습용
- **전처리**
  - keypoint → YOLO txt 라벨 변환
  - bbox는 keypoint 기반 자동 생성

---

## 🏗️ 모델 아키텍처

