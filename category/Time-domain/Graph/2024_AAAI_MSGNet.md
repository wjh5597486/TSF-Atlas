---
title: "MSGNet: Learning Multi-Scale Inter-Series Correlations for Multivariate Time Series Forecasting"
authors: Wanlin Cai et al.
venue: AAAI
year: 2024
paper_url: https://arxiv.org/abs/2401.00423
code_url: https://github.com/YoZhibo/MSGNet
tags: [time-domain, graph, multi-scale, fft, multivariate, transformer]
primary_category: category/Time-domain/Graph
related_categories:
  - category/Decomposition/MultiScale
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_MSGNet.pdf
pdf_source: arxiv
---

## TL;DR
MSGNet은 FFT 기반 주기 추출과 적응형 혼홉(mixhop) 그래프 합성곱을 결합하여 다중 시간 스케일에서의 변수 간 상관관계를 동시에 학습하는 MTSF 모델이다. 각 스케일별로 FFT 모듈 → 적응형 그래프 합성곱 → 멀티헤드 어텐션을 순서대로 적용하여 inter-series 및 intra-series 패턴을 포착한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델(Autoformer, FEDformer 등)은 단일 시간 해상도에서만 inter-series 상관관계를 모델링하여, 서로 다른 주기/스케일에서 나타나는 복잡한 변수 간 관계를 포착하지 못함.
- 선형 모델(DLinear 등)은 channel-independent 가정하에 변수 간 상호작용을 완전히 무시함.
- 가설: 다변량 시계열에서 변수 간 상관관계는 시간 스케일에 따라 달라지므로, 멀티스케일 표현을 활용한 그래프 기반 학습이 필요하다.

## 2. 제안 방법론
- **핵심 아이디어**: FFT로 각 시계열에서 지배적인 주기를 추출하여 여러 시간 스케일을 정의하고, 각 스케일에서 adaptive graph convolution으로 inter-series 상관관계를 학습.
- **ScaleGraph Block 구성**:
  1. **FFT Module**: 입력 시계열에서 상위-k 주기를 추출하여 다중 스케일로 재구성.
  2. **Adaptive Mixhop Graph Convolution Layer**: 학습 가능한 인접 행렬로 다양한 hop 거리의 inter-series correlation을 캡처.
  3. **Multi-Head Self-Attention Module**: 같은 스케일 내 intra-series temporal dependency 학습.
- **ScaleGraph Block** 여러 개를 스태킹하여 계층적 스케일 표현 획득.
- 손실함수: MSE.
- 학습 전략: 표준 지도학습.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 미기재 (표준 Z-score 또는 RevIN 추정)
- Train/Val/Test split: 8545 / 2881 / 2881 (hourly)
- Batch size: 32 / Optimizer: Adam / LR: 1e-4
- Loss function: MSE
- 기타: epochs=10, d_model∈{16,32}, ScaleGraph blocks∈{1,2}, Mixhop order=2, k∈{3,5}, 8 heads

### 3.2 Results
논문 Table 2 기준 ETTh1 평균 성능 (4 horizons 평균):

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 표 참조 | 표 참조 | 경쟁적 |
| 192     | 표 참조 | 표 참조 | 경쟁적 |
| 336     | 표 참조 | 표 참조 | 경쟁적 |
| 720     | 표 참조 | 표 참조 | 경쟁적 |

출처: Table 2 of paper (전체 수치는 논문 원문 참조). MSGNet은 ETTh1에서 rank 1 MSE를 대부분 horizons에서 달성(GPU memory 13-17GB, 316-482 sec/epoch).

## 4. 주요 기여
- 주파수 도메인(FFT)으로 자동으로 의미 있는 다중 시간 스케일을 추출하는 최초의 그래프 기반 MTSF 접근.
- ScaleGraph Block: FFT + 적응형 mixhop 그래프 합성곱 + self-attention의 통합 설계.
- 학습 가능한 인접 행렬로 사전에 그래프 구조 정의 불필요 (self-supervised graph learning).
- 표준 ETT/Weather/Electricity/Traffic 벤치마크에서 경쟁적 성능 달성.
- 가볍고 확장 가능한 설계 (파라미터 효율적).

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 주기 추출에 FFT를 사용하므로 비주기적(aperiodic) 시계열에서 성능 저하 가능성.
- ※ 메모: 멀티스케일 FFT 기반 그래프 학습을 확산 모델이나 SSM과 결합하는 방향 흥미로움.
- ※ 메모: 각 ScaleGraph Block의 스케일 수를 자동으로 결정하는 방법 연구 여지 있음.

## 6. 참고
- arXiv: https://arxiv.org/abs/2401.00423
- GitHub: https://github.com/YoZhibo/MSGNet
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/28991
