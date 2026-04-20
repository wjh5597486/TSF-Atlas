---
title: "Non-stationary Diffusion For Probabilistic Time Series Forecasting"
authors: Weiwei Ye et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2505.04278
code_url: ""
tags: [time-domain, diffusion, generative, probabilistic-forecasting, non-stationarity]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_NsDiff.pdf
pdf_source: arxiv
---

## TL;DR
NsDiff is an ICML 2025 Spotlight paper that proposes a non-stationary diffusion framework for probabilistic time series forecasting. By combining a denoising diffusion-based conditional generative model with a pre-trained conditional mean and variance estimator, it explicitly models the time-varying uncertainty patterns in non-stationary time series.

## 1. 기존 모델의 한계 / 가설
- 기존 확산 모델 기반 확률적 예측(CSDI, Diffusion-TS 등)은 고정된 노이즈 스케줄을 사용하여 비정상(non-stationary) 시계열의 시변 불확실성을 적절히 모델링하지 못함.
- 표준 가우시안 확산 프레임워크는 시계열의 local 분산 변화(heteroscedasticity)를 포착하기 어려움.

## 2. 제안 방법론
- **핵심 아이디어**: 사전 학습된 조건부 평균/분산 추정기와 결합한 비정상 확산(non-stationary diffusion) 프레임워크.
- **아키텍처**:
  - Pre-trained conditional mean estimator: 결정론적 예측 기반값 제공.
  - Pre-trained conditional variance estimator: 시변 불확실성 추정.
  - Non-stationary forward process: 시변 노이즈 스케줄로 평균/분산 추정값 활용.
  - Denoising diffusion: 역방향 과정으로 샘플 생성.
- **손실함수**: ELBO / diffusion 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: Diffusion ELBO
- 기타: 9개 실제/합성 데이터셋 평가 (ETTh/m 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA (probabilistic) |
| 192     | see paper | see paper | SOTA (probabilistic) |
| 336     | see paper | see paper | SOTA (probabilistic) |
| 720     | see paper | see paper | SOTA (probabilistic) |

출처: Table of paper (9 real-world and synthetic datasets).

## 4. 주요 기여
- 시변 불확실성 패턴을 명시적으로 모델링하는 비정상 확산 프레임워크 (ICML 2025 Spotlight).
- 사전 학습된 평균/분산 추정기와의 결합으로 확산 프레임워크 개선.
- 9개 데이터셋에서 기존 확률적 예측 방법 대비 우수한 성능.

## 5. 한계 및 후속 연구 아이디어
- 두 단계 학습(mean/variance estimator + diffusion) 구조로 훈련 복잡도 증가.
- ※ 메모: RATD(NeurIPS 2024)와 유사하게 결정론적 모델로 확산의 조건을 가이드하는 방식.

## 6. 참고
- 관련 논문: CSDI (NeurIPS 2021), Diffusion-TS (ICLR 2024), RATD (NeurIPS 2024)
- arXiv: https://arxiv.org/abs/2505.04278
- ICML 2025 Spotlight Poster: https://icml.cc/virtual/2025/poster/44783
