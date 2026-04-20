---
title: "Non-autoregressive Conditional Diffusion Models for Time Series Prediction"
authors: Shen et al.
venue: ICML 2023
year: 2023
paper_url: https://arxiv.org/abs/2306.05043
code_url: ""
tags: [time-domain, diffusion, generative, non-autoregressive, conditional]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICML_TimeDiff.pdf
pdf_source: arxiv
---

## TL;DR
TimeDiff는 두 가지 새로운 conditioning 메커니즘(future mixup, autoregressive initialization)을 도입한 non-autoregressive diffusion 모델이다. 기존 diffusion 기반 시계열 모델을 일관되게 능가하며 Transformer, FiLM 등 강한 기준 모델 대비 전반적으로 최고 성능을 달성했다.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion 기반 시계열 예측 모델(TimeGrad, CSDI 등)은 autoregressive 방식으로 느리거나, conditioning이 불충분함
- 단순히 과거 관측값을 conditioning으로 쓰는 기존 방법은 미래 추세나 단기 패턴을 초기화에 반영하지 못함
- Non-autoregressive 생성이 가능하면 속도와 품질을 동시에 개선할 수 있다는 가설

## 2. 제안 방법론
- **핵심 아이디어**: 두 conditioning 메커니즘 도입으로 diffusion denoising 과정을 더 잘 가이드
- **아키텍처 구성요소**:
  - **Future Mixup**: 훈련 시 ground-truth 미래 시퀀스의 일부를 noisy conditioning으로 활용 — 모델이 미래 예측을 점진적으로 확정할 수 있도록 유도
  - **Autoregressive Initialization**: diffusion 초기 샘플을 무작위 가우시안이 아닌 autoregressive 모델(간단한 AR)의 예측값으로 초기화 — 단기 추세 및 패턴을 반영
  - Non-autoregressive denoising score network (Transformer 기반)
- **손실함수**: score matching loss (denoising diffusion)
- **학습 전략**: 훈련 시 future mixup ratio를 랜덤하게 샘플링하여 다양한 conditioning 강도 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (or varies by dataset)
- Forecast horizons: 96, 192, 336, 720 (long-term), 1~7 days (short-term도 평가)
- Normalization: instance normalization / 표준화 적용
- Train/Val/Test split: 표준 long-term forecasting 분할
- Batch size / Optimizer / LR: Adam optimizer, 상세 설정은 논문 Appendix 참고
- Loss function: denoising score matching (MSE 기반)
- 기타: 9개 실제 데이터셋 평가 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, KDD Cup 2018, Solar, Electricity 등)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 기존 diffusion 모델 일관 능가 |
| 192     | see paper | see paper | |
| 336     | see paper | see paper | |
| 720     | see paper | see paper | |

출처: Table 1-3 of paper (proceedings.mlr.press/v202/shen23d).

## 4. 주요 기여
- Future Mixup과 Autoregressive Initialization이라는 두 conditioning 메커니즘 제안
- Non-autoregressive 방식으로 빠른 추론 가능
- 9개 실제 데이터셋에서 기존 diffusion 기반 SOTA 전반 능가
- Transformer 및 FiLM 등 결정론적 모델 대비에서도 경쟁력 있는 성능

## 5. 한계 및 후속 연구 아이디어
- Diffusion 모델 특성상 deterministic 모델 대비 샘플링 과정이 여전히 느림
- Future Mixup의 mixing ratio 선택에 대한 민감도 분석 필요
- ※ 메모: probabilistic forecasting 관점에서 calibration 평가 강화 필요; future mixup 아이디어는 다른 생성 모델에도 적용 가능

## 6. 참고
- arXiv: https://arxiv.org/abs/2306.05043
- ICML 2023 poster: https://icml.cc/virtual/2023/poster/25084
- PMLR proceedings: https://proceedings.mlr.press/v202/shen23d.html
