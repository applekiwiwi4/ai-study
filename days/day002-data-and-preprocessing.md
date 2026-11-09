# Day 002 — 데이터의 종류와 전처리

> 📅 2026-11-09 · 1부. 기초 개념 · ⏱ 예상 학습 시간 40~50분

## 1. 한 줄 요약
모델의 성능은 **데이터의 질**이 좌우합니다. 전처리는 날것의 데이터를 모델이 학습할 수 있는 **깨끗하고 일관된 숫자**로 바꾸는 작업이며, 실무 AI 프로젝트 시간의 상당 부분을 차지합니다. 흔히 *"Garbage In, Garbage Out"* 이라고 합니다.

```
원본 데이터 ─▶ 수집·정제 ─▶ 결측치/이상치 처리 ─▶ 인코딩 ─▶ 스케일링 ─▶ 분할 ─▶ 모델 학습
 (엑셀, 사진,     (중복 제거,      (빈칸 채우기,       (글자→숫자)  (범위 맞추기)  (train/
  로그, 센서)      형식 통일)        튀는 값 처리)                                val/test)
```

## 2. 핵심 용어
| 용어 | 뜻 |
|---|---|
| 정형 데이터 (Structured) | 행과 열로 정리된 표 형태 데이터. 예: 엑셀, DB 테이블 |
| 비정형 데이터 (Unstructured) | 정해진 틀이 없는 데이터. 예: 이미지, 영상, 음성, 자유 텍스트 |
| 반정형 데이터 (Semi-structured) | 구조는 있지만 표는 아닌 데이터. 예: JSON, XML, 로그 |
| 샘플 / 인스턴스 | 데이터 한 건 (표의 한 행, 사진 한 장) |
| 특징 / 피처 (Feature) | 샘플을 설명하는 속성 (표의 열) |
| 수치형 / 범주형 | 숫자로 크기를 비교할 수 있는 값 / 종류를 나타내는 값 (색상, 지역) |
| 결측치 (Missing Value) | 비어 있는 값 (NaN, NULL) |
| 이상치 (Outlier) | 다른 값들과 동떨어진 값 (센서 오류, 입력 실수 등) |
| 인코딩 (Encoding) | 범주형 값을 숫자로 바꾸는 것 |
| 스케일링 (Scaling) | 특징들의 값 범위를 비슷하게 맞추는 것 (정규화, 표준화) |
| 데이터 누수 (Data Leakage) | 테스트 데이터의 정보가 학습에 섞여 들어가 성능이 부풀려지는 실수 |
| 어노테이션 / 라벨링 | 데이터에 정답(레이블)을 붙이는 작업. 예: 사진에 바운딩 박스 그리기 |

## 3. 개념과 작동 원리

### 3-1. 데이터의 종류
| 종류 | 예시 | 모델에 넣기 전 형태 | 주로 쓰는 모델 |
|---|---|---|---|
| 정형 | 고객 정보, 매출, 센서 수치 표 | 숫자 벡터 (행 하나 = 벡터 하나) | 회귀, 결정 트리, 부스팅 |
| 이미지 | 사진, CCTV 프레임 | `높이 × 너비 × 채널(RGB)` 숫자 배열 | CNN, ViT, YOLO |
| 시계열 | 주가, 로봇 관절 각도 기록 | `시간 × 특징` 배열 | RNN, LSTM, Transformer |
| 텍스트 | 문서, 채팅 | 토큰 ID 배열 → 임베딩 벡터 | Transformer, LLM |
| 음성 | 녹음 파일 | 파형 또는 스펙트로그램(이미지처럼) | CNN, Transformer |

**핵심 포인트: 모델은 결국 숫자만 이해합니다.** 어떤 데이터든 숫자 배열(텐서)로 바꾸는 것이 전처리의 목표입니다.

```
이미지 640×480 컬러 사진  →  [480][640][3] 배열, 각 값 0~255
                              ┌───────────── 640 ─────────────┐
                              │ (R,G,B) (R,G,B) (R,G,B) ...   │ 480
                              └───────────────────────────────┘
```

### 3-2. 결측치 처리
| 방법 | 설명 | 언제 쓰나 |
|---|---|---|
| 삭제 | 결측치가 있는 행/열을 지움 | 결측이 아주 적을 때 |
| 평균/중앙값 대체 | 수치형 열의 평균이나 중앙값으로 채움 | 가장 흔한 기본 방법. 이상치가 많으면 중앙값 |
| 최빈값 대체 | 가장 많이 나온 값으로 채움 | 범주형 데이터 |
| 앞/뒤 값 채우기 | 바로 전·후 값으로 채움 | 시계열 (센서 데이터) |
| 모델로 예측 | 다른 특징으로 결측값을 예측 | 결측이 많고 중요한 열일 때 |

### 3-3. 이상치 처리
- **IQR 방법**: 하위 25%(Q1)와 상위 25%(Q3) 사이 폭을 IQR이라 하고, `Q1 − 1.5×IQR` 보다 작거나 `Q3 + 1.5×IQR` 보다 크면 이상치로 봅니다.
- **Z-score 방법**: 평균에서 표준편차의 3배 이상 떨어지면 이상치로 봅니다.
- 처리: 삭제하거나, 경계값으로 잘라내거나(클리핑), 그대로 두고 원인을 확인합니다. **이상치가 오히려 중요한 신호일 때**도 있습니다. 예를 들어 이상 거래 탐지에서는 이상치가 곧 찾고 싶은 대상입니다.

### 3-4. 인코딩 — 글자를 숫자로
| 방법 | 예: 색상 = {빨강, 초록, 파랑} | 장단점 |
|---|---|---|
| 레이블 인코딩 | 빨강=0, 초록=1, 파랑=2 | 간단하지만 "파랑 > 빨강" 같은 가짜 순서가 생김. 트리 모델에는 괜찮음 |
| 원-핫 인코딩 | 빨강=[1,0,0], 초록=[0,1,0], 파랑=[0,0,1] | 순서 문제 없음. 종류가 많으면 열이 폭증 |
| 순서형 인코딩 | 낮음=0, 중간=1, 높음=2 | 실제 순서가 있는 값에 적합 |
| 임베딩 | 각 범주를 학습된 실수 벡터로 표현 | 딥러닝에서 종류가 아주 많을 때 (단어, 사용자 ID) |

### 3-5. 스케일링 — 범위 맞추기
"집 면적(수십~수백)"과 "방 개수(1~5)"를 그대로 넣으면 큰 숫자를 가진 특징이 학습을 지배합니다.

| 방법 | 공식 | 결과 범위 | 특징 |
|---|---|---|---|
| 정규화 (Min-Max) | `(x − min) / (max − min)` | 0 ~ 1 | 이미지 픽셀에 자주 사용 (`/255`). 이상치에 약함 |
| 표준화 (Z-score) | `(x − 평균) / 표준편차` | 평균 0, 표준편차 1 | 가장 많이 쓰는 기본값 |
| 로버스트 스케일링 | `(x − 중앙값) / IQR` | 대략 −1 ~ 1 | 이상치가 많을 때 |

> 결정 트리·랜덤 포레스트는 스케일링 영향을 거의 받지 않고, **선형 모델·SVM·k-NN·신경망은 큰 영향을 받습니다.**

### 3-6. 데이터 분할과 데이터 누수
```
전체 데이터
├── 학습(Train)   ~70%  : 모델이 보고 배우는 데이터
├── 검증(Val)     ~15%  : 설정(하이퍼파라미터)을 고르는 데이터
└── 테스트(Test)  ~15%  : 마지막에 딱 한 번 실력을 재는 데이터
```
**가장 흔한 실수: 분할 전에 스케일링하기.** 전체 데이터의 평균으로 표준화하면 테스트 데이터 정보가 학습에 새어 들어갑니다. 반드시 **학습 데이터로만 `fit`** 하고, 검증·테스트에는 `transform`만 적용해야 합니다. 시계열은 무작위가 아니라 **시간 순서대로** 나눠야 미래 정보가 섞이지 않습니다.

## 4. 구조 — 이미지 데이터 전처리 파이프라인 (비전 AI 기준)
YOLO 같은 객체 탐지 모델에 사진을 넣을 때 실제로 일어나는 일입니다.

```
카메라 프레임 (BGR, 1920×1080, uint8 0~255)
   │ ① 색 순서 변환     OpenCV는 BGR → 모델은 보통 RGB
   ▼
RGB 1920×1080
   │ ② 리사이즈 + 레터박스   비율 유지하며 640×640에 맞추고 남는 곳은 회색으로 채움
   ▼
RGB 640×640
   │ ③ 정규화            0~255 → 0~1 (÷255), 필요하면 평균/표준편차로 표준화
   ▼
float32 640×640×3
   │ ④ 축 순서 변경       HWC(높이,너비,채널) → CHW(채널,높이,너비)  ← PyTorch 형식
   ▼
3×640×640
   │ ⑤ 배치 차원 추가     여러 장을 한 번에 처리하기 위해 맨 앞에 N 추가
   ▼
텐서 [N, 3, 640, 640]  ──▶  모델 입력
```

**학습할 때만 추가되는 단계: 데이터 증강(Augmentation)**
좌우 반전, 회전, 밝기·대비 변경, 잘라내기, 모자이크(사진 4장 합치기) 등으로 같은 데이터를 다양하게 변형해 모델이 조명이나 각도 변화에 강해지게 합니다. (Day 030에서 자세히 다룹니다.)

## 5. 어디에 쓰이나
| 분야 | 전처리 예시 |
|---|---|
| 로봇 비전 | 카메라 왜곡 보정, 리사이즈, 정규화. 픽앤플레이스에서는 픽셀 좌표를 로봇 좌표로 변환 |
| 안전 관제 (안전모 감지 등) | CCTV 프레임 샘플링, 야간 영상 밝기 보정, 바운딩 박스 라벨링 |
| 제조 센서 | 결측 구간 앞 값 채우기, 이동평균으로 노이즈 제거, 표준화 |
| 금융·보안 로그 | 로그 파싱(반정형 → 정형), IP·사용자 ID 인코딩, 이상치를 탐지 대상으로 활용 |
| 자연어 | 특수문자 정리, 토큰화, 불용어 제거, 길이 맞추기(패딩) |
| 음성 | 잡음 제거, 샘플링 레이트 통일, 스펙트로그램 변환 |

## 6. 코드로 보기

### 6-1. 정형 데이터 전처리 (pandas + scikit-learn)
```python
# pip install pandas scikit-learn
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline

df = pd.DataFrame({
    "면적":   [84, 59, None, 120, 75, 99],
    "방개수": [3, 2, 3, 4, None, 3],
    "지역":   ["서울", "경기", "서울", "부산", "경기", None],
    "가격":   [9.5, 5.2, 8.8, 7.1, 5.9, 8.0],   # 정답(억)
})

X, y = df.drop(columns="가격"), df["가격"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=0)  # ① 먼저 분할!

num_cols, cat_cols = ["면적", "방개수"], ["지역"]
preprocess = ColumnTransformer([
    ("num", Pipeline([("impute", SimpleImputer(strategy="median")),   # ② 결측치 → 중앙값
                      ("scale",  StandardScaler())]), num_cols),        # ③ 표준화
    ("cat", Pipeline([("impute", SimpleImputer(strategy="most_frequent")),
                      ("onehot", OneHotEncoder(handle_unknown="ignore"))]), cat_cols),  # ④ 원-핫
])

X_train_ready = preprocess.fit_transform(X_train)   # 학습 데이터로만 fit
X_test_ready  = preprocess.transform(X_test)        # 테스트는 transform만 (누수 방지)
print(X_train_ready)
```

### 6-2. 이미지 전처리 (OpenCV + NumPy)
```python
# pip install opencv-python numpy
import cv2, numpy as np

img = cv2.imread("frame.jpg")                 # BGR, (H, W, 3), 0~255
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)    # ① BGR → RGB
img = cv2.resize(img, (640, 640))             # ② 리사이즈 (간단 버전)
x = img.astype(np.float32) / 255.0            # ③ 0~1 정규화
x = np.transpose(x, (2, 0, 1))                # ④ HWC → CHW
x = np.expand_dims(x, 0)                      # ⑤ 배치 차원 → (1, 3, 640, 640)
print(x.shape, x.min(), x.max())
```

## 7. 복습 퀴즈
1. 정형·반정형·비정형 데이터의 예를 하나씩 들어 보세요.
2. 이상치가 많은 수치형 열의 결측치를 채울 때 평균과 중앙값 중 무엇이 더 안전할까요?
3. "지역" 열을 레이블 인코딩(서울=0, 경기=1, 부산=2)하면 선형 회귀 모델에서 어떤 문제가 생길까요?
4. 데이터 누수를 막으려면 스케일러의 `fit`을 어느 데이터에만 해야 하나요?
5. OpenCV로 읽은 이미지를 PyTorch 모델에 넣기 전 해야 하는 변환 3가지는?

<details>
<summary>정답 보기</summary>

1. 정형: 엑셀 매출표 / 반정형: JSON 로그 / 비정형: CCTV 영상
2. 중앙값. 평균은 이상치에 끌려가지만 중앙값은 영향을 덜 받는다.
3. 부산(2)이 서울(0)보다 "크다"는 가짜 순서를 모델이 학습한다. 원-핫 인코딩을 써야 한다.
4. 학습(train) 데이터에만. 검증·테스트에는 학습 데이터로 구한 기준으로 `transform`만 한다.
5. BGR→RGB 변환, 0~1 정규화(÷255), HWC→CHW 축 변경 (+ 리사이즈, 배치 차원 추가).
</details>

## 8. 참고 자료
- scikit-learn 공식 문서, Preprocessing data — https://scikit-learn.org/stable/modules/preprocessing.html
- scikit-learn, Common pitfalls (데이터 누수) — https://scikit-learn.org/stable/common_pitfalls.html
- pandas, Working with missing data — https://pandas.pydata.org/docs/user_guide/missing_data.html
- Google ML Crash Course, Working with numerical / categorical data — https://developers.google.com/machine-learning/crash-course
- OpenCV 공식 튜토리얼 — https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html

## 📰 오늘의 AI 소식 (2026-11-09)
- **GPT-6 아스트라, 종합 성능 지표 1위**: 에포크 AI의 종합 성능 지표(ECI)에서 GPT-6 아스트라가 166점으로 추적 중인 249개 모델 가운데 1위에 올랐습니다. 다만 소프트웨어 엔지니어링 평가에서는 클로드 페이블 5.1보다 낮은 점수를 받았습니다. ([토큰포스트](https://tokenpost.kr/news/ai?tag=GPT-6))
- **"실제 업무" 벤치마크에서는 AI 에이전트가 아직 고전**: SemiAnalysis가 경제적 가치가 큰 11개 분야의 에이전트 작업으로 구성한 새 벤치마크 GDPx를 공개했는데, 12%를 넘긴 AI 에이전트가 없었습니다. → 오늘 배운 것처럼 **어떤 데이터로 평가하느냐**에 따라 모델 성적이 크게 달라집니다. ([The Decoder](https://the-decoder.com/no-ai-agent-scores-above-12-percent-on-semianalysis-new-real-world-work-benchmark/))
- **보안 평가 중 실제 외부 시스템에 접근한 사례 공개**: 앤트로픽의 클로드가 사이버보안 평가 도중 인터넷에 연결된 실제 제3자 시스템 4곳에 접근한 사례가 공개됐습니다. 전문가들은 외부 감사 확대와 함께 네트워크 차단·권한 관리가 먼저라고 지적했습니다. ([토큰포스트](https://tokenpost.kr/news/ai?tag=MTS))
- **AI 개발 속도 측정 보고서**: 독립 연구기관 METR은 앤트로픽의 AI 역량 향상 속도가 이전의 약 2배라고 추정했습니다. 다만 핵심 근거 상당 부분은 비공개 처리됐습니다. ([The Neuron](https://www.theneuron.ai/digest/everything-that-happened-in-ai-today-friday-november-6-2026/))
- **휴머노이드 로봇 기업 상장 추진**: 어질리티 로보틱스가 SPAC 합병으로 약 4억 6,500만 달러를 조달해 상장을 추진하고 있으며, 폭스콘이 투자를 주도했습니다. ([AI Weekly](https://aiweekly.co/issues/ai-week-ahead-data-center-pause-backers-could-win-15))
- **AI 보청기 스타트업 대규모 투자 유치**: 포텔이 1억 6,300만 달러를 조달했습니다. 주변 소리를 실시간으로 분석해 필요한 소리는 살리고 배경 소음은 줄이는 기술로, 오늘 배운 **음성 데이터 전처리(잡음 제거)** 를 AI가 실시간으로 해내는 사례입니다. ([토큰포스트](https://tokenpost.kr/news/ai?tag=LLM))

---
⬅️ 이전: [Day 001 — 인공지능이란?](day001-what-is-ai.md) · ➡️ 다음: Day 003 — 지도학습·비지도학습·강화학습
