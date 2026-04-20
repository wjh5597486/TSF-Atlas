---
title: Deep Switching Auto-Regressive Factorization: Application to Time Series Forecasting
authors: Amirreza Farnoosh, Bahar Azari, Sarah Ostadabbas
venue: AAAI 2021
year: 2021
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/16907
code_url: https://github.com/ostadabbas/DSARF
tags: [generative, vae, probabilistic-forecasting, factor-analysis, switching-dynamics]
primary_category: Time-domain/Generative/VAE-GAN
related_categories: []
reports_etth1: false
swept_in: 2026-04-20
pdf_local: ./2021_AAAI_DSARF.pdf
pdf_source: aaai
---

## TL;DR
Deep Switching Auto-Regressive Factorization (DSARF) is a deep generative model for spatio-temporal data that captures multimodal temporal dynamics through switching vector auto-regressive likelihoods. Enables robust short- and long-term predictions on weather, traffic, climate, and disease spread data.

## 1. 기존 모델의 한계 / 가설
- 단순 factor analysis는 시간 변화하는 weights와 factors의 비선형 의존성 포착 불가능
- 고정된 선형 모델은 복잡한 비선형 spatio-temporal 패턴 표현 불가
- 다양한 동역학(multimodal dynamics)을 가진 시계열 분석 필요 (예: 날씨의 계절성 변화, 질병 확산 단계)
- 기존 방법들은 isolated feature representation으로 독립 변수 간 상관성 무시

## 2. 제안 방법론

### 핵심 아이디어
Weights를 deep switching vector auto-regressive process로 모델링하여, 시간에 따라 변하는 latent states가 weight 및 factor의 비선형 상호작용을 동적으로 결정. Markovian prior로 상태 전이 관리.

### 아키텍처 구성요소

**Switching Vector Auto-Regressive (VAR) Likelihood**
- Weight matrix W_t의 동역학을 switching VAR로 표현
- Discrete latent state s_t가 W_t의 평균과 공분산 결정
- Markovian prior: p(s_t | s_{t-1}) 상태 전이 확률

**Deep Generative Factor Model**
- 고차원 데이터: y_t = W_t × f_t + noise
- W_t: time-dependent weights (동적)
- f_t: latent factors (저차원)
- Hierarchical parametrization: 신경망이 W_t와 f_t 통제

**Variational Inference**
- Stochastic variational inference (SVI)로 scalable 학습
- Recognition network로 approximate posterior 학습
- ELBO 최적화

**Gating Mechanism**
- 각 Gaussian mixture component의 가중치를 동적 조정
- Adaptive distribution tuning for flexible modeling

### 주요 수식
- Factor model: p(y_t | W_t, f_t) = N(y_t; W_t f_t, Σ)
- Switching prior: p(W_t | s_t) where s_t ~ Categorical(π)
- Markovian state transition: p(s_t | s_{t-1})
- Joint likelihood: p(y, W, f, s)로 모든 변수 적분

### 손실함수 / 학습 전략
- ELBO 기반 variational inference
- Reparameterization trick으로 gradient 계산
- Mini-batch SGD with Adam optimizer
- Annealing schedule for KL weight

## 3. ETTh1 실험 결과

> 본 논문은 ETTh1/ETTh2/ETTm 등의 표준 LTSF 벤치마크를 명시적으로 보고하지 않음.
> 대신 weather (real meteorological data), traffic (METR-LA, PEMS-BAY), climate change, 질병 확산 등의 데이터셋 사용.
> frontmatter `reports_etth1: false`로 표시.

### 3.1 Datasets Evaluated
- **Weather**: Max Planck Institute 기상 관측 데이터
- **Traffic**: METR-LA, PEMS-BAY 교통 흐름 데이터
- **Climate**: 과거 기후 변화 데이터
- **Disease**: 감염병 확산 수열 데이터
- **Nonlinear Systems**: 합성 동역학계 데이터

### 3.2 Key Results (Non-ETT Benchmarks)
- Weather forecasting에서 기존 SOTA 대비 우수한 성능
- Traffic prediction의 multiple steps ahead에서 일관된 개선
- 질병 확산 예측의 비선형 동역학 포착 우수

※ 메모: DSARF는 ETTh1 등 표준 multivariate 벤치마크 대신, 도메인-특화 데이터(weather, traffic, epidemiology)에 중점. Switching dynamics의 필요성이 명확한 응용 분야에 강점.

## 4. 주요 기여
- Switching auto-regressive weight model로 시변 다이나믹 포착 (최초 제안)
- Deep generative factor analysis로 비선형 spatio-temporal 의존성 학습
- Markovian switching prior로 해석 가능한 상태 전이 모델링
- 다양한 실제 응용(weather, traffic, disease spread)에서 검증
- Stochastic variational inference로 scalable 학습

## 5. 한계 및 후속 연구 아이디어
- Discrete latent state의 개수 선택 어려움 (model selection 문제)
- 계산 복잡도 높음 (variational inference + switching dynamics)
- Very long-term forecast에서 상태 불확실성 누적
- Cold-start: 초기 상태 분포 설정의 영향
- ETT 등 표준 벤치마크로의 적응성 미검증 (도메인-특화 데이터 중심)

## 6. 참고
- GitHub: https://github.com/ostadabbas/DSARF (PyTorch 구현)
- ArXiv: https://arxiv.org/abs/2009.05135
- AAAI official: https://aaai.org/papers/07394-deep-switching-auto-regressive-factorization-application-to-time-series-forecasting/
- 저자 프로필: Sarah Ostadabbas (Northeastern University)
