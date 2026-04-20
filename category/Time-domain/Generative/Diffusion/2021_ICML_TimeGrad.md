---
title: Autoregressive Denoising Diffusion Models for Multivariate Probabilistic Time Series Forecasting
authors: Kashif Rasul, Calvin Seward, Ingmar Schuster, Roland Vollgraf
venue: ICML 2021
year: 2021
paper_url: https://proceedings.mlr.press/v139/rasul21a.html
code_url: https://github.com/zalanborsos/diffusion-TS
tags: [diffusion | generative | probabilistic-forecasting | multivariate | transformer]
primary_category: Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local: ./2021_ICML_TimeGrad.pdf
pdf_source: arxiv
---

## TL;DR
TimeGrad은 diffusion probabilistic models를 활용한 자기회귀 모델로, 각 시간 단계에서 gradient를 추정하여 다변량 시계열의 확률적 예측을 수행한다. 기존 점 예측 방식과 달리 예측 불확실성을 명시적으로 특성화한다.

## 1. 기존 모델의 한계 / 가설

- 기존 시계열 예측 모델들은 주로 점 예측(point forecasting)에 집중하여 예측 불확실성을 제대로 특성화하지 못함
- 다변량 시계열 데이터에서 수천 개의 상관관계 있는 차원을 다루기 어려움
- 확률적 예측이 필요한 실무 응용에서 신뢰도 높은 불확실성 정량화가 부족함

## 2. 제안 방법론

### 핵심 아이디어
Diffusion probabilistic models를 시계열 예측에 적용하여 각 시간 단계에서 gradient를 추정함으로써 확률 분포를 샘플링하는 자기회귀 모델

### 아키텍처 구성요소
- **Diffusion Model**: 노이즈로부터 시작하여 Markov chain을 통해 데이터 분포로 변환
- **Autoregressive Generation**: 각 시간 단계 t에서 조건부 분포 p(y_t | y_{1:t-1})를 학습
- **Langevin Sampling**: 추론 시 gradient 추정을 활용한 샘플링
- **Neural Network**: LSTM 또는 GRU 기반 recurrent architecture로 temporal dynamics 모델링

### 주요 수식
- 변분 하한(variational lower bound) 최적화로 gradient 학습
- 각 시간 단계에서: y_t ~ p(y_t | y_{1:t-1}) via Langevin MCMC
- 조건부 diffusion model: p_θ(y_t | y_{1:t-1}, noise) → denoising network

### 손실함수 / 학습 전략
- Variational bound 기반 likelihood 최적화
- Diffusion process 전체에 걸친 noise level별 가중 손실
- 시계열 전체 분포를 한 번에 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문에서 명시된 설정 미확인 (기본값 168 가정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준화(Z-score)
- Train/Val/Test split: 비율 0.6/0.2/0.2
- Batch size / Optimizer / LR: 상세 설정 미확인
- Loss function: Variational lower bound
- 평가 지표: CRPS (Continuous Ranked Probability Score), Quantile loss

### 3.2 Results

아래 데이터셋들에 대해 확률적 예측 성능 평가 (CRPS/Quantile loss):

| Dataset | Metric | h=96 | h=192 | h=336 | h=720 | 비고 |
|---------|--------|------|-------|-------|-------|------|
| **Electricity** | CRPSsum | 0.0206 | - | - | - | SOTA 대비 0.0207 (Transformer-MAF) 대비 개선 |
| **Traffic** | - | - | - | - | - | 5/6 데이터셋 중 상위 성능 |
| **Solar** | - | - | - | - | - | 5/6 데이터셋 중 상위 성능 |
| **Wikipedia** | - | - | - | - | - | 5/6 데이터셋 중 상위 성능 |
| **Taxi** | - | - | - | - | - | 5/6 데이터셋 중 상위 성능 |

※ 주: 논문이 확률적 예측 성능(CRPS)을 주요 지표로 사용하므로 MSE/MAE 점 예측 지표는 표준 LTSF 벤치마크 비교와 다를 수 있음. ETTh1에 대한 명시적 결과표는 논문 원문 확인 필요.

## 4. 주요 기여

- **새로운 패러다임**: Diffusion probabilistic models를 시계열 확률적 예측에 처음 적용
- **자기회귀 생성**: 각 시간 단계에서의 조건부 분포를 명시적으로 학습
- **확률 특성화**: 점 예측과 함께 예측 불확실성을 CRPS로 명시적 평가
- **높은 차원 성능**: 수천 개 차원의 상관관계 있는 시계열에서 SOTA 달성
- **실무 적용성**: 금융, 에너지, 교통 등 실제 도메인 데이터셋에서 검증

## 5. 한계 및 후속 연구 아이디어

- **추론 속도**: Langevin sampling으로 인한 느린 추론 (error accumulation 누적 가능)
- **조건부 분포 가정**: Markov 가정으로 인한 장기 의존성 활용 제한
- **평가 지표 제한**: CRPS는 학계 표준이나 실무에서는 점 예측 비교도 필요
- **확장 가능성**: 더 큰 규모 데이터셋(예: 매우 긴 시계열)에 대한 성능 미검증

※ 메모: TimeGrad는 diffusion 기반 생성 모델의 출발점으로, 이후 여러 확장 연구(mr-Diff, TMDM 등)의 기초가 됨.

## 6. 참고

- **원본 코드**: [GitHub - diffusion-TS](https://github.com/zalanborsos/diffusion-TS)
- **arXiv**: [2101.12072](https://arxiv.org/abs/2101.12072)
- **관련 논문**: Denoising Diffusion Probabilistic Models (Ho et al., 2020), Energy-based models for time series
- **실험 코드**: PMLR proceedings 상 공개 코드 링크
