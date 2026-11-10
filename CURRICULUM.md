# 인공지능 60일 커리큘럼

매일 한 주제씩 순서대로 진행합니다. 각 자료는 `days/dayNNN-주제.md`에 올라가며, 완료된 날은 체크 표시가 됩니다.

> 진행 규칙: 자동 작성 시 아래 목록에서 **체크되지 않은 첫 번째 항목**을 그날의 주제로 삼고, 작성 후 `[x]`로 바꿉니다.

## 1부. 기초 개념 (1~8일)
- [x] Day 001 — 인공지능이란? AI·머신러닝·딥러닝의 관계와 역사
- [x] Day 002 — 데이터의 종류와 전처리 (정형/비정형, 정규화, 결측치, 인코딩)
- [x] Day 003 — 지도학습·비지도학습·강화학습
- [x] Day 004 — 학습의 기본 흐름: 모델, 손실 함수, 최적화, 학습/검증/테스트 분할
- [ ] Day 005 — 과적합과 과소적합, 편향-분산 트레이드오프, 정규화(L1/L2)
- [ ] Day 006 — 평가 지표: 정확도, 정밀도, 재현율, F1, 혼동행렬, ROC-AUC, MSE
- [ ] Day 007 — AI를 위한 수학 1: 벡터·행렬·텐서
- [ ] Day 008 — AI를 위한 수학 2: 미분, 그래디언트, 확률 기초

## 2부. 고전 머신러닝 (9~16일)
- [ ] Day 009 — 선형 회귀와 경사하강법
- [ ] Day 010 — 로지스틱 회귀와 분류
- [ ] Day 011 — 결정 트리
- [ ] Day 012 — 앙상블: 랜덤 포레스트, 부스팅(XGBoost, LightGBM)
- [ ] Day 013 — SVM (서포트 벡터 머신)과 커널
- [ ] Day 014 — k-NN과 거리 기반 학습
- [ ] Day 015 — 군집화: k-means, DBSCAN
- [ ] Day 016 — 차원 축소: PCA, t-SNE, UMAP

## 3부. 딥러닝 기초 (17~24일)
- [ ] Day 017 — 퍼셉트론과 다층 퍼셉트론(MLP)
- [ ] Day 018 — 활성화 함수: Sigmoid, Tanh, ReLU, GELU, Softmax
- [ ] Day 019 — 역전파(Backpropagation)
- [ ] Day 020 — 옵티마이저: SGD, Momentum, Adam, AdamW, 학습률 스케줄링
- [ ] Day 021 — 배치 정규화, 레이어 정규화, 드롭아웃
- [ ] Day 022 — 가중치 초기화와 기울기 소실/폭주
- [ ] Day 023 — PyTorch 기초: 텐서, autograd, nn.Module, 학습 루프
- [ ] Day 024 — GPU와 학습 인프라: CUDA, 배치 크기, 혼합 정밀도

## 4부. 컴퓨터 비전 (25~36일)
- [ ] Day 025 — 이미지의 표현과 OpenCV 기초
- [ ] Day 026 — CNN 구조: 합성곱, 필터, 패딩, 스트라이드, 풀링
- [ ] Day 027 — 고전 CNN: LeNet, AlexNet, VGG
- [ ] Day 028 — ResNet과 잔차 연결, Inception
- [ ] Day 029 — 경량 모델: MobileNet, EfficientNet
- [ ] Day 030 — 전이학습과 파인튜닝, 데이터 증강
- [ ] Day 031 — 객체 탐지 1: 개념, IoU, NMS, mAP, 2-stage(R-CNN 계열)
- [ ] Day 032 — 객체 탐지 2: YOLO 구조와 버전별 변화
- [ ] Day 033 — 세그멘테이션: U-Net, Mask R-CNN, SAM
- [ ] Day 034 — 객체 추적: SORT, DeepSORT, ByteTrack
- [ ] Day 035 — 얼굴 인식과 포즈 추정 (임베딩, MediaPipe, OpenPose)
- [ ] Day 036 — Vision Transformer(ViT)와 CLIP

## 5부. 순차 데이터와 자연어 처리 (37~44일)
- [ ] Day 037 — 자연어 처리 기초: 토큰화, 임베딩, Word2Vec
- [ ] Day 038 — RNN 구조와 한계
- [ ] Day 039 — LSTM과 GRU
- [ ] Day 040 — Seq2Seq와 어텐션 메커니즘
- [ ] Day 041 — Transformer 구조 1: Self-Attention, Multi-Head Attention
- [ ] Day 042 — Transformer 구조 2: 위치 인코딩, 인코더/디코더, FFN
- [ ] Day 043 — BERT 계열 (인코더 모델)
- [ ] Day 044 — GPT 계열 (디코더 모델)

## 6부. 생성형 AI와 LLM (45~52일)
- [ ] Day 045 — 대규모 언어 모델(LLM)의 학습: 사전학습, 스케일링 법칙
- [ ] Day 046 — 파인튜닝, RLHF, LoRA
- [ ] Day 047 — 프롬프트 엔지니어링
- [ ] Day 048 — RAG(검색 증강 생성)와 벡터 데이터베이스
- [ ] Day 049 — AI 에이전트와 도구 사용
- [ ] Day 050 — 생성 모델 1: 오토인코더, VAE
- [ ] Day 051 — 생성 모델 2: GAN
- [ ] Day 052 — 생성 모델 3: 디퓨전 모델 (Stable Diffusion)

## 7부. 강화학습과 로보틱스 (53~57일)
- [ ] Day 053 — 강화학습 기초: MDP, 보상, 정책, 가치 함수
- [ ] Day 054 — Q-Learning과 DQN
- [ ] Day 055 — 정책 경사, PPO
- [ ] Day 056 — 로보틱스와 AI: 비전 기반 제어, Sim-to-Real, 픽앤플레이스
- [ ] Day 057 — VLA(Vision-Language-Action) 모델과 로봇 파운데이션 모델

## 8부. 실무와 배포 (58~60일)
- [ ] Day 058 — 모델 경량화: 양자화, 프루닝, 지식 증류
- [ ] Day 059 — 엣지 배포: ONNX, TensorRT, 라즈베리파이·Jetson
- [ ] Day 060 — MLOps와 AI 윤리·보안

---
60일 과정을 마친 뒤에는 최신 논문과 모델을 다루는 심화 주제로 이어서 진행합니다.
