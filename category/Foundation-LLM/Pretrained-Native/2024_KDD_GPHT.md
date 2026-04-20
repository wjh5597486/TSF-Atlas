---
title: "Generative Pretrained Hierarchical Transformer for Time Series Forecasting"
authors: Zhiding Liu et al.
venue: KDD 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.16516
code_url: https://github.com/icantnamemyself/GPHT
tags: [transformer, pretrained, generative, autoregressive, foundation, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_KDD_GPHT.pdf
pdf_source: arxiv
---

## TL;DR
GPHT는 다양한 데이터셋을 혼합하여 사전학습된 계층적 Transformer 기반 생성 모델로, 자동회귀 방식으로 시계열 예측을 수행한다. 단일 unified 모델이 임의의 horizon에서 예측 가능하며, 사전학습 후 소량의 데이터로도 높은 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 방법들은 단일 데이터셋 훈련에 의존하여 일반화 능력이 제한됨.
- One-step generation schema가 일반적으로 사용되어 커스텀 forecasting head가 필요하며, 출력 시계열의 temporal dependency를 무시함.
- 저자들은 (1) 대규모 혼합 데이터셋 사전학습과 (2) 자동회귀적 token-level 생성으로 이 문제를 해결할 수 있다고 가설.

## 2. 제안 방법론
- **핵심 아이디어**: channel-independent 가정 하에 여러 데이터셋을 혼합한 대규모 데이터로 계층적 Transformer를 사전학습하고, 자동회귀 방식으로 예측 토큰을 생성.
- **아키텍처 구성요소**:
  - 시계열을 패치(토큰)로 분할
  - Multi-stage hierarchical Transformer blocks: 다양한 temporal scale에서 공통 패턴 포착
  - Autoregressive decoding: 커스텀 forecasting head 없이 임의 horizon 예측 가능
- **사전학습**: ETTh1/h2, ETTm1/m2, Weather, Electricity, Traffic, ILI 등 다수 데이터셋 혼합
- **손실함수**: MSE 기반 자동회귀 생성 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (표준 setting)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 ETTh1/h2에서 훈련 데이터 5% 조건 시 기존 대비 30.19% (ETTh1), 12.49% (ETTh2) 상대적 성능 향상 보고.
> Full training setting에서의 ETTh1 수치는 원문 Table 참고.

출처: 원문 주요 표 (전체 훈련 데이터 및 5% 데이터 실험 포함).

## 4. 주요 기여
- 다양한 시계열 데이터셋을 혼합한 대규모 사전학습 패러다임 제시
- 자동회귀 방식으로 임의 horizon 예측 가능 (커스텀 head 불필요)
- 계층적 Transformer blocks로 다양한 데이터의 공통 패턴 포착
- 소량 데이터(5%) 환경에서도 우수한 few-shot 성능 달성

## 5. 한계 및 후속 연구 아이디어
- ETTm1에서는 상대적으로 낮은 성능 (저자 인정)
- 자동회귀 생성 방식의 inference 비용이 높을 수 있음
- ※ 메모: 더 다양한 도메인 데이터 추가 시 일반화 성능 향상 기대

## 6. 참고
- 관련 논문: Timer (ICML 2024), MOIRAI, Lag-Llama, TimesFM
- ACM DL: https://dl.acm.org/doi/10.1145/3637528.3671855
- GitHub: https://github.com/icantnamemyself/GPHT
