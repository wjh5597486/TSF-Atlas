---
title: "K²VAE: A Koopman-Kalman Enhanced Variational AutoEncoder for Probabilistic Time Series Forecasting"
authors: Xingjian Wu et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2505.23017
code_url: https://github.com/decisionintelligence/K2VAE
tags: [time-domain, vae, generative, koopman, kalman, probabilistic-forecasting, long-term-forecasting]
primary_category: category/Time-domain/Generative/VAE-GAN
related_categories: [category/Decomposition/Operator]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_K2VAE.pdf
pdf_source: arxiv
---

## TL;DR
K²VAE is an ICML 2025 Spotlight paper that combines Koopman theory and Kalman filtering within a VAE framework for long-term probabilistic time series forecasting. KoopmanNet transforms nonlinear time series into a linear dynamical system, while KalmanNet refines predictions and models uncertainty, reducing error accumulation in long-horizon forecasting.

## 1. 기존 모델의 한계 / 가설
- 대부분의 생성 모델(diffusion, flow)은 단기 예측에 최적화되어 있으며, 장기 확률 예측(LPTSF)에서 비선형 동역학이 성능을 크게 저하시킴.
- 장기 예측 시 오차 누적(error accumulation) 문제가 심각.
- 비선형 시계열을 선형 동역학 시스템으로 변환하면 Kalman filter를 통한 효율적인 불확실성 추정이 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Koopman operator로 비선형 시계열을 선형 동역학 공간으로 변환 후, Kalman filter로 불확실성 정제.
- **아키텍처**:
  - **KoopmanNet**: 비선형 시계열 → 선형 동역학 시스템 변환. Koopman operator 학습.
  - **KalmanNet**: 선형 공간에서 예측 정제 및 불확실성 모델링.
  - VAE 프레임워크: 잠재 공간에서 확률 분포 학습.
  - 장기/단기 확률 예측 모두 지원.
- **손실함수**: ELBO (Evidence Lower BOund) + 예측 손실.
- **학습 전략**: VAE 표준 학습.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (단기/장기 각각 다름)
- Forecast horizons: 단기(96, 192, 336, 720), 장기(720+ 포함)
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: ELBO
- 기타: 8개 단기 + 9개 장기 실제 데이터셋 (ETTh1, ETTh2, ETTm1, ETTm2 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA (probabilistic) |
| 192     | see paper | see paper | SOTA (probabilistic) |
| 336     | see paper | see paper | SOTA (probabilistic) |
| 720     | see paper | see paper | SOTA (probabilistic) |

출처: Table of paper (short-term 8 datasets + long-term 9 datasets).

## 4. 주요 기여
- Koopman theory + Kalman filtering + VAE의 통합 프레임워크 제안.
- 장기 확률 예측에서 오차 누적 문제 완화.
- 단기 및 장기 확률 예측 모두에서 SOTA 성능 달성 (ICML 2025 Spotlight).
- 비선형 시계열의 선형화를 통한 효율적 불확실성 정량화.

## 5. 한계 및 후속 연구 아이디어
- Koopman operator 학습의 수렴 안정성 및 비선형 동역학 표현 한계.
- ※ 메모: Koopa(NeurIPS 2023)의 Koopman 기반 접근을 확률적 VAE로 확장한 형태.

## 6. 참고
- 관련 논문: Koopa (NeurIPS 2023), CSDI (NeurIPS 2021), Diffusion-TS (ICLR 2024)
- GitHub: https://github.com/decisionintelligence/K2VAE
- arXiv: https://arxiv.org/abs/2505.23017
- ICML 2025 Spotlight Poster: https://icml.cc/virtual/2025/poster/46346
