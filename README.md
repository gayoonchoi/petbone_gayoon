

# 🐶 슬개지킴이 (Patella Guardian) from gayoon 

반려견의 보행 이미지를 기반으로 슬개골 탈구 가능성을 예측하는 인공지능 서비스입니다.  
YOLO11 Pose 모델로 keypoint를 추출하고, XGBoost 분류기를 통해 탈구의 중증도(severity)를 예측합니다.

---

## 📌 프로젝트 개요

- **프로젝트명**: 슬개지킴이 (Patella Guardian)
- **수행 기간**: 2025.08.01 ~ 2025.08.08
- **사용 목적**: 반려견 슬개골 탈구의 조기 진단 및 건강관리 지원

---
<img width="1423" height="754" alt="image" src="https://github.com/user-attachments/assets/5b873ed6-9bf2-41ff-a590-2ea03d4b1a9f" />


## 🧠 핵심 기능

- 반려견 보행 이미지 기반 keypoint 추출 (YOLO11 Pose)
- keypoint angle 기반 슬개골 탈구 중증도 예측 (XGBoost)
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

## 🏗️ 서비스 아키텍처

🧭 **사용 순서**  
① 이미지 업로드 → ② YOLO 11 Pose로 keypoint 추출 → ③ XGBoost로 중증도 분류 *(severity 1~3)* → ④ PyQt UI 결과 시각화

<img width="1454" height="805" alt="image" src="https://github.com/user-attachments/assets/283d6462-31dd-411b-a6bd-26f62b4e812a" />


## 📈 학습 결과 및 모델 성능 시각화
YOLOv11 Pose 모델 학습 결과, 다음과 같은 성능 지표를 확인했습니다.
전체적으로 손실 함수 감소, 정확도 및 정밀도 지표의 안정적인 수렴이 이루어졌습니다.

##  항목	설명
| 항목                                                           | 설명                              |
| ------------------------------------------------------------ | ------------------------------- |
| `box_loss`, `pose_loss`, `kobj_loss`, `cls_loss`, `dfl_loss` | 모두 꾸준히 감소하며 안정적인 학습곡선 형성        |
| `metrics/precision(P)`                                       | 약 0.9 이상으로 수렴 → 오탐률 낮음          |
| `metrics/recall(P)`                                          | 약 0.95 도달 → 누락 없이 잘 탐지          |
| `metrics/mAP50(P)`                                           | 약 0.95 이상 → 높은 keypoint 예측 정확도  |
| `metrics/mAP50-95(P)`                                        | 약 0.75 이상 → 다양한 IoU 기준에서도 성능 우수 |


<img width="1892" height="535" alt="image" src="https://github.com/user-attachments/assets/ca709be0-db86-48cb-8b01-0526f0809f4b" />
<br>
<img width="2065" height="572" alt="image" src="https://github.com/user-attachments/assets/70d3d638-6c42-4790-9ac1-5e9eeb5794bd" />


🔎 train/val 손실 모두 감소하며 과적합 없이 수렴하였고, 정밀도와 재현율(P)이 90% 이상으로 유지되어 keypoint 기반 예측 성능이 우수함을 확인했습니다.
- **Keypoint Confidence**: 평균 ≥ 0.85
- **분류 모델**: XGBoost (멀티클래스)
- **강점**:
  - 소형견/중형견 분리로 정확도 향상
  - 커스텀 threshold 설정 및 오분류 분석

---

## 🖥️ PyQt 앱

- 이미지 업로드 → keypoint 시각화 → 예측 결과 출력
- 간단한 UI 기반 자가 진단 제공
- 추후 동영상 입력 및 운동 처방 기능 확장 예정

<img width="1323" height="526" alt="image" src="https://github.com/user-attachments/assets/f1fb7dd7-54dd-4807-b5fa-bcf84025f33b" />


## 🔭 향후 발전 방향

- 프레임 단위 보행 분석 (영상 입력)
- 고관절·십자인대 등 관절 질환 예측 확대
- 수의사용 진단 리포트 및 병원 연계 B2B 서비스

<img width="1441" height="733" alt="image" src="https://github.com/user-attachments/assets/88b715e5-ee41-4d19-9f6d-b9911a5a0fc6" />


## 🙋‍♀️ 개발자

**최가윤 (GAYOON CHOI)**  
- 멀티모달 AI 프로젝트 과정 수료  
- 데이터 분석, 모델 설계, PyQt 앱 개발

---

## 🐾 키워드

`YOLO11`, `Pose Estimation`, `XGBoost`, `Patellar Luxation`, `PyQt5`, `AI for Pet`, `반려동물 건강`, `슬개골`, `Keypoint Detection`















