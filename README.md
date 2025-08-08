

# 🐶 슬개지킴이 (Patella Guardian) from gayoon choi

반려견의 보행 이미지를 기반으로 슬개골 탈구 중증도(severity)를 예측 서비스로, 
**YOLOv11 Pose** 활용하여 keypoint 추출 및 **XGBoost** 기반 분류 모델을 통해 자가 진단 도구로 활용 가능한 GUI 애플리케이션 구현 

<img width="1302" height="596" alt="image" src="https://github.com/user-attachments/assets/5465aecf-de5b-4ab8-87da-e6b37677a5f7" />


## 📌 프로젝트 개요

- **프로젝트명**: 슬개지킴이 (Patella Guardian)
- **수행 기간**: 2025.08.01 ~ 2025.08.08
- **사용 목적**: 반려견 슬개골 탈구의 조기 진단 및 건강관리 지원

<img width="1345" height="553" alt="image" src="https://github.com/user-attachments/assets/0f6b6ed2-737e-4bef-9811-e23acaec3376" />

## 🧠 주요 기능

- 소형견/중형견 분리 및 방향별 모델 정제
- YOLOv11 Pose 모델 기반 keypoint 추출 (측면/정면/후면 별 모델)
- 관절 각도 기반 슬개골 탈구 중증도 예측 (XGBoost 다중 분류)
- PyQt 기반 자가 진단 GUI
- 추후 영상 입력 및 수의사용 리포트 기능 확장 가능

## 🗂 데이터 수집 및 구성

| 구분          | 설명           |
|---------------|---------------|
| **AI Hub** | 반려견 보행 이미지 약 1,500장 (측면/정면/후면 구분), 12개 keypoint json|
| **Stanford Dogs** | 약 2만장 (품종 120종), keypoint 24개, 전이학습용 |
| **분포** | 소형견 1027장 / 중형견 23장 |
| **Label** | severity(0~4), 견종, 나이, 병원 등 포함 |
| **전처리** | JSON → YOLO txt 변환, keypoint 기반 bbox 자동 생성, 방향별 yaml 구성 (측면/정면/후면) 
---
<img width="1335" height="544" alt="image" src="https://github.com/user-attachments/assets/aa3b716a-a43e-413a-b372-ec92376d4fd3" />
---
<img width="1285" height="587" alt="image" src="https://github.com/user-attachments/assets/4745c73e-50d7-4990-a4d3-db03c8bf7e8f" />
---

<img width="1315" height="586" alt="image" src="https://github.com/user-attachments/assets/ec18decf-7393-499b-a59b-44658ffacc6d" />



## 🔧 데이터 전처리 및 워크플로우

| 단계          | 설명          |
|---------------|---------------|
| **수집** | 방향별 이미지/JSON/메타정보 수집 (AI Hub / Stanford Dogs) |
| **전처리** | 방향별로 데이터 분할 → txt 파일 생성 → yaml 구성 |
| **분석** | keypoint → angle feature 생성 (무릎, 고관절, 어깨, 등, 꼬리) |
| **모델** | YOLOv11 Pose + XGBoost 분류기 훈련 |
| **평가** | 방향별 mAP, Precision, Recall 측정 및 예측 시각화 수행


## 데이터 워크플로우 상세화 
<img width="1397" height="607" alt="image" src="https://github.com/user-attachments/assets/ba93ddd8-667b-4f20-85ea-4dceefa7a4e6" />


## 🏗️ 서비스 아키텍처

<img width="1337" height="586" alt="image" src="https://github.com/user-attachments/assets/697221d6-b70e-43f6-814a-2a88ac1b1945" />


## 📈 Yolo 학습 결과 및 모델 성능 시각화

## 1. 측면 방향 (Side) 
전체적으로 손실 함수 감소, 정확도 및 정밀도 지표의 안정적인 수렴이 이루어졌습니다. 하지만 Early Stopping이 적용되어 400 epoch 이전인 **366 epoch**에서 학습이 종료되었습니다.  

<img width="1892" height="535" alt="image" src="https://github.com/user-attachments/assets/ca709be0-db86-48cb-8b01-0526f0809f4b" />

##  항목	설명
| 항목                                                           | 설명                              |
| ------------------------------------------------------------ | --------------------------------- |
| `box_loss`, `pose_loss` 등 주요 손실값 | 전체 학습 과정에서 꾸준히 감소, 안정적인 학습곡선 형성 |
| `metrics/precision(P)`                                       | 최종 0.926 → 오탐률 낮음 |
| `metrics/recall(P)`                                          | 최종 0.937 → 대부분의 객체를 놓치지 않고 탐지 |
| `metrics/mAP50(P)`                                           | 최종 0.919 → keypoint 예측 정확도 양호 |
| `metrics/mAP50-95(P)`                                        | 최종 0.752 → 다양한 IoU 기준에서도 준수한 성능 |
| **Best Epoch (208)**                                         | Precision 0.962, Recall 0.975, mAP50 0.981, mAP50-95 0.756 |

## object detection, class, keypoint pose 측면 방향 (Side) 

<img width="2065" height="572" alt="image" src="https://github.com/user-attachments/assets/70d3d638-6c42-4790-9ac1-5e9eeb5794bd" />

🔎 Early Stopping으로 인한 조기 종료에도 불구하고, 정밀도와 재현율(P)이 92~97% 범위로 유지되며 안정적인 예측 성능을 확보했습니다.

## 2. 전면 방향 (Front) 
전체적으로 손실 함수 감소, 정확도 및 정밀도 지표의 안정적인 수렴이 이루어졌습니다.

<img width="4200" height="1200" alt="results_front" src="https://github.com/user-attachments/assets/92445312-a36b-4a96-a31a-8bf41028116c" />


##  항목	설명
| 항목                                                           | 설명                              |
| ------------------------------------------------------------ | --------------------------------- |
| `box_loss`, `pose_loss`, `kobj_loss`, `cls_loss`, `dfl_loss` | 모두 꾸준히 감소하며 안정적인 학습곡선 형성 |
| `metrics/precision(P)`                                       | **0.944**로 수렴 → 오탐률 낮음 |
| `metrics/recall(P)`                                          | **0.945** → 대부분의 객체를 놓치지 않고 탐지 |
| `metrics/mAP50(P)`                                           | **0.941** → keypoint 예측 정확도 우수 |
| `metrics/mAP50-95(P)`                                        | **0.807** → 다양한 IoU 기준에서도 양호한 성능 |

🔎 train/val 손실 모두 감소하며 과적합 없이 수렴했고, 정밀도와 재현율(P)이 94% 이상 유지되어 keypoint 기반 예측 성능이 안정적임을 확인했습니다.


## object detection, class, keypoint pose 전전면 방향 (Front) 
<img width="2062" height="571" alt="image" src="https://github.com/user-attachments/assets/4f6a77b6-a923-4965-ab00-a8560ec3969e" />



## 3. 후면 방향 (Tail) 
<img width="2065" height="572" alt="results_tail" src="https://github.com/user-attachments/assets/ef084531-ef6e-43af-a781-eacb66df913b" />
전체적으로 손실 함수 감소, 정확도 및 정밀도 지표의 안정적인 수렴이 이루어졌습니다.

##  항목	설명
| 항목                                                           | 설명                              |
| ------------------------------------------------------------ | --------------------------------- |
| `box_loss`, `pose_loss`, `kobj_loss`, `cls_loss`, `dfl_loss` | 모두 꾸준히 감소하며 안정적인 학습곡선 형성 |
| `metrics/precision(P)`                                       | **0.999**로 수렴 → 오탐률 거의 없음 |
| `metrics/recall(P)`                                          | **1.000** 도달 → 누락 없이 완벽 탐지 |
| `metrics/mAP50(P)`                                           | **0.995** → 높은 keypoint 예측 정확도 |
| `metrics/mAP50-95(P)`                                        | **0.839** → 다양한 IoU 기준에서도 우수 |

🔎 train/val 손실 모두 감소하며 과적합 없이 수렴하였고, 정밀도와 재현율(P)이 99% 이상 유지되어 keypoint 기반 예측 성능이 매우 우수함을 확인했습니다.

## object detection, class, keypoint pose 후면 방향 (Tail) 
<img width="2067" height="545" alt="image" src="https://github.com/user-attachments/assets/22657594-11e0-4790-b0ab-e6b33e5f9592" />

### 📌 해석
객체 탐지와 포즈(keypoint) 검출 모두 매우 높은 정확도와 재현율을 기록했습니다.
mAP50이 0.995로 거의 완벽에 가까우며, 다양한 IoU 조건을 고려한 mAP50-95도 포즈 기준 0.84로 우수한 편입니다.
Validation 손실이 안정적으로 낮아, 학습이 잘 수렴했고 과적합 징후도 크지 않습니다.


## 📈  XGBoost 학습 결과 및 모델 성능 시각화 (업데이트 예정) 
- **다중 분류**: Severity 값 기준으로 슬개골 탈구 1,2,3기 설정 
- **강점**:
  - 소형견/중형견 분리로 정확도 향상
  - 커스텀 threshold 설정 및 오분류 분석
  
<img width="446" height="355" alt="image" src="https://github.com/user-attachments/assets/3fa9588c-97fc-4e80-9594-8bc27348399b" />

---

## 🖥️ PyQt 앱

- 이미지 업로드 → keypoint 시각화 (supervision) → 예측 결과 출력
- 간단한 UI 기반 반려견 슬개구 탈구 자가 진단 제공
- 추후 동영상 입력 및 운동 처방 기능 확장 예정

<img width="774" height="501" alt="image" src="https://github.com/user-attachments/assets/87659f3d-ffe5-429e-aa07-dfbf5dd84fd9" />


## 🔭 향후 발전 방향

- 프레임 단위 보행 분석 (영상 입력)
- 고관절·십자인대 등 관절 질환 예측 확대
- 수의사용 진단 리포트 및 병원 연계 B2B 서비스

<img width="1315" height="537" alt="image" src="https://github.com/user-attachments/assets/7252ab9e-8f73-4171-96b0-0f7eb0a78a89" />

## 슬개지킴이 서비스 운영 전략 

<img width="1392" height="575" alt="image" src="https://github.com/user-attachments/assets/5351d7b2-89c3-4bdc-b6e1-0f58fbf54265" />



## 🙋‍♀️ 개발자

**최가윤 (GAYOON CHOI)**  
- 멀티모달 AI 프로젝트 진행 
- 데이터 분석, 모델 설계, PyQt 앱 개발

---

## 🐾 키워드

`YOLO11`, `Pose Estimation`, `XGBoost`, `Patellar Luxation`, `PyQt5`, `AI for Pet`, `반려동물 건강`, `슬개골`, `Keypoint Detection`















