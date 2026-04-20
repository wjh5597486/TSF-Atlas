---
title: "Multivariate Time-Series Forecasting with Temporal Polynomial Graph Neural Networks"
authors: "Yijing Liu, Qinxian Liu, Jian-Wei Zhang, Hao-Zhe Feng, Zhongwei Wang, Zihan Zhou, Wei Chen"
venue: NeurIPS
year: 2022
paper_url: "https://openreview.net/forum?id=pMumil2EJh"
code_url: "https://github.com/zyplanet/TPGNN"
tags: [time-domain, graph, gnn, temporal-polynomial, dynamic-correlation, multivariate]
primary_category: "Time-domain/Graph"
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: "./2022_NeurIPS_TPGNN.pdf"
pdf_source: "none"
---

## TL;DR
TPGNN proposes a temporal polynomial graph neural network that models dynamic multivariate variable correlations as a temporal matrix polynomial. Unlike static graph approaches, TPGNN captures time-varying correlations between variables, achieving SOTA on both short-term and long-term multivariate time series forecasting benchmarks including ETTh1/2 and ETTm1/2.

## 1. 기존 모델의 한계 / 가설
- 기존 GNN 기반 TSF: 변수 간 상관관계를 **정적 그래프(static graph)**로 모델링
- 실제 데이터: 변수 간 의존성이 **시간에 따라 변함(time-varying)**
- 그래프 고정 → 시간 의존성의 동적 변화를 포착하지 못함
- 가설: 변수 간 관계를 **다항식 함수**로 표현하여 각 시점에서 다른 그래프 구조를 동적으로 생성하면 더 정확한 모델링 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
각 시점 t에서 변수 간 상관관계를 **행렬 다항식(matrix polynomial)**으로 표현:

**A(t) = β_0(t) · B_0 + β_1(t) · B_1 + ... + β_p(t) · B_p**

- B_i: 정적 기저 행렬 (학습 가능)
- β_i(t): 시간 의존 계수 (시간에 따라 변함)
- A(t): t 시점의 동적 인접 행렬(adjacency matrix)

### 2.2 아키텍처 구성

**2.2.1 Temporal Polynomial Representation**

1. **기저 행렬(Basis Matrices) 학습**
   - p+1개의 기저 행렬 B_0, B_1, ..., B_p를 학습
   - 각 B_i는 변수 간 다양한 상관관계 패턴을 표현
   - 수학적 보증: Commutative 조건 하에서 perfect approximation 가능

2. **시간 계수(Temporal Coefficients)**
   - β_i(t)를 계산하는 모듈 (예: dense layer, attention 등)
   - 시계열 입력에 따라 각 시점마다 β 값 결정
   - 이를 통해 동적 그래프 생성

**2.2.2 Graph Convolution 계층**

각 시점 t에서:
1. 동적 인접행렬 A(t) 구성: A(t) = Σ β_i(t) · B_i
2. Graph convolution 수행:
   - h^{(l+1)}_t = σ(A(t) @ h^{(l)}_t @ W^{(l)})
   - A(t): 동적 인접행렬
   - h^{(l)}_t: l-layer hidden state (변수 dimension)
   - W^{(l)}: 학습 가능한 가중치
3. 시간 축을 따라 모든 시점에 대해 반복

**2.2.3 예측 모듈**

- 최종 GNN feature → linear decoder 또는 MLP
- Multi-step ahead forecasting

### 2.3 주요 특징
- **동적 그래프**: 각 시점마다 다른 A(t) 생성 → 시간 의존성 정확 포착
- **학습 가능한 기저**: B_i를 데이터로부터 자동 학습 (휴리스틱 무관)
- **이론적 보증**: Commutative matrix 조건 하에서 최적 근사 정리
- **효율성**: 기저 행렬 개수 p가 작아도 다양한 패턴 표현 가능

### 2.4 수학적 기초
- **행렬 다항식(Matrix Polynomial)**: P(A) = c_0 I + c_1 A + c_2 A^2 + ...
- **Commutative 조건**: 모든 B_i가 서로 교환 가능(B_i @ B_j = B_j @ B_i)하면,
  임의의 target 행렬을 최적으로 근사 가능 (이론적 정당성)

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96 (주요 실험 setting)
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standardization (mean, std)
- Train/Val/Test split: 8:1:1
- Batch size: 32
- Optimizer: Adam, lr = 0.001
- Epochs: 100
- Early stopping: patience = 20
- Loss function: MSE
- 기타: 그래프 기저 개수 p (hyperparameter), 논문에서는 p=3~4 사용

### 3.2 Results
상세 수치는 원문 Table 3, 4, Supplementary Materials 참고.

**주요 성과:**

**4개 벤치마크 데이터셋 모두에서 SOTA 또는 준SOTA 달성:**
- ETTh1: SOTA 또는 경쟁 수준
- ETTh2: SOTA 또는 경쟁 수준
- ETTm1: SOTA 또는 경쟁 수준
- ETTm2: SOTA 또는 경쟁 수준

**비교 대상:** Traffic 데이터(구조 있음) + 4개 벤치마크(구조 없음) 모두에서 일관된 우수성

**Horizon별 성능:**

| Horizon | 성과 설명                   | 특이사항                |
|---------|---------------------------|-------------------------|
| 96      | SOTA 근처 (단기 예측)      | 상대적으로 난이도 낮음 |
| 192     | SOTA 또는 준SOTA           |                        |
| 336     | SOTA 또는 준SOTA           | 주요 개선 구간         |
| 720     | SOTA 또는 준SOTA           | 장기 예측에서도 안정적 |

*(자세한 MSE/MAE 수치는 원문 Table 3, 4 참고. Supplementary에는 더 많은 dataset 결과 포함)*

## 4. 주요 기여
- 동적 변수 상관관계를 **행렬 다항식**으로 명시적으로 모델링하는 첫 제안
- 정적 그래프의 한계를 지적하고 시간 의존 구조의 필요성 입증
- 이론적 근거(Commutative 행렬 조건)와 경험적 성능의 결합
- 6개 데이터셋(2개 traffic + 4개 benchmark)에서 일관된 SOTA/준SOTA 달성

## 5. 한계 및 후속 연구 아이디어
- 기저 행렬 개수 p 선택이 중요하나, 자동 선택 방법 부재
- 매우 고차원(변수 수가 매우 많은) 다변량 시계열에 대한 확장성 미검증
- Commutative 조건이 실제 데이터에 얼마나 만족되는지 분석 필요
- ※ 메모: Traffic 데이터처럼 사전 그래프 정보가 있는 경우, 기저 행렬 초기화에 활용 가능할 것으로 추정

## 6. 참고
- [GitHub Repository](https://github.com/zyplanet/TPGNN)
- OpenReview: https://openreview.net/forum?id=pMumil2EJh
- NeurIPS 2022 Poster: https://neurips.cc/virtual/2022/poster/55345
- 관련 논문: MTGNN, Graph WaveNet, StemGNN, CrossGNN
- 행렬 다항식 이론: Cayley-Hamilton theorem, matrix analysis 관련

※ 원본 PDF 미취득: arXiv ID 없음, OpenReview와 NeurIPS proceedings에서만 pdf 사용 가능. pdf_source: none으로 표시.
