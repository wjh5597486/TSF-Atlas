---
title: Variance Reduced Training with Stratified Sampling for Forecasting Models
authors: Yucheng Lu, Youngsuk Park, Lifan Chen, Yuyang Wang, Christopher De Sa, Dean Foster
venue: ICML 2021
year: 2021
paper_url: https://proceedings.mlr.press/v139/lu21d.html
code_url: https://github.com/awslabs/gluon-ts
tags: [optimization | training | variance-reduction | control-variate | stratification]
primary_category: CrossCutting/Loss
related_categories: []
reports_etth1: false
swept_in: 2026-04-20
pdf_local: ./2021_ICML_VarianceReduced.pdf
pdf_source: arxiv
---

## TL;DR
다변량 시계열 예측 모델 학습 시 stochastic optimizer의 높은 gradient 추정 분산 문제를 해결하기 위해 stratified sampling과 control variate를 결합한 SCott 최적화 알고리즘을 제안. 수렴 이론 보장 및 실험적 수렴 속도 개선 입증.

## 1. 기존 모델의 한계 / 가설

- **문제**: 대규모 시계열 예측에서 시간 패턴이 데이터셋 내에서 이질적(heterogeneous)일 때, 표준 SGD 최적화가 매우 높은 gradient 분산으로 인해 느린 수렴
- **가설**: 시계열 데이터를 유사한 특성별로 사전 그룹화(stratification)하면 샘플링 분산을 줄일 수 있음
- **동기**: 금융, 에너지, 교통 등 실무 데이터에서는 수백~수천 개의 이질적 시계열을 동시에 처리해야 하는데, 이에 최적화된 학습 방법 부재

## 2. 제안 방법론

### 핵심 아이디어
시계열을 사전에 정의된 strata로 분할하고, 각 stratum에서 control variate를 활용한 분산 감소 기법을 적용하는 optimizier (SCott: Stochastic Stratified Control Variate Gradient Descent)

### 방법론 구성요소

#### 2.1 Stratification
- 시계열을 K개 stratum으로 그룹화 (예: 데이터 특성, 도메인, 시계열 길이별)
- 각 stratum S_k에서 데이터 분포 P_k가 유사하다고 가정

#### 2.2 Control Variate 기반 분산 감소
- 표준 gradient: ∇_θ L(θ) = E[∇L_i(θ)]
- Control variate: Z = ∇L_i(θ) - (∇L_ref(θ) - μ_ref) 형태로 분산 감소
  - 여기서 L_ref는 reference loss (예: 전체 데이터셋 평균)

#### 2.3 Stratified Sampling
- Mini-batch를 구성할 때 각 stratum에서 확률적으로 샘플
- Stratum별로 다른 learning rate 적용 가능

### 주요 수식
```
SCott Update:
θ_{t+1} = θ_t - α_t × (∇L_{S_i}(θ_t) - (∇L̂_{ref}(θ_t) - ∇L̂_{ref,prev}))
         = θ_t - α_t × Z_t    (control variate 형태)
```

- Variance reduction rate: Var(Z) < Var(∇L) (이론적 보장)
- Convergence guarantee: 평활 non-convex 목적함수에 대해 ε-정상점 수렴

### 손실함수 / 학습 전략
- **손실함수**: 표준 예측 손실 (MSE, MAE 등, 아키텍처 불가지론적)
- **학습 전략**:
  1. Pre-processing: 시계열을 K개 stratum으로 분할
  2. Online: 각 iteration에서 stratum별로 control variate 계산 및 업데이트
  3. Warm-up: 초기 iterations에서 reference gradient 수집

## 3. 실험 설정 및 결과

### 주의
이 논문은 **최적화 알고리즘** 중심의 방법론 논문으로, 특정 예측 아키텍처와 벤치마크(예: ETTh1)에 대한 정량적 성능 결과를 주요 초점으로 하지 않음. 대신 수렴 속도와 학습 효율을 평가하는 synthetic 및 일반적인 시계열 데이터셋에서 검증.

### 평가 관점
- **이론**: Smooth non-convex 목적함수에 대한 convergence 증명
- **실험 대상**: 합성 데이터 + 실제 forecasting 문제
- **메트릭**: Wall-clock time, iteration count, final loss value (점 예측 MSE/MAE 아님)

### 결론
SCott은 표준 SGD 대비 **수렴 속도 개선** (iterations & wall-clock time 모두) 입증. 특히 이질적 시계열을 다루는 대규모 예측 문제에서 효과적.

## 4. 주요 기여

- **첫 시도**: 시계열 예측의 이질성(heterogeneity) 문제를 stratified sampling으로 명시적 해결
- **이론적 보장**: Smooth non-convex 목적함수에 대한 SCott 수렴 증명
- **메모리 효율**: Control variate는 reference gradient만 저장하면 되므로 추가 메모리 최소
- **실무 적용성**: GluonTS 및 주요 forecasting 라이브러리에 통합 가능한 범용 optimizer
- **광범위 적용**: 아키텍처 불가지론적이므로 모든 신경망 기반 예측 모델에 적용 가능

## 5. 한계 및 후속 연구 아이디어

- **Stratum 정의**: 최적 stratification 방법은 도메인/데이터셋마다 다를 수 있음 → 자동 stratum 발견 필요
- **Hyperparameter 튜닝**: Learning rate schedule, control variate 업데이트 주기 등 추가 튜닝 가능
- **이론 확장**: Non-smooth, decentralized, privacy-preserving settings로 확장 미검토
- **벤치마크 부재**: ETTh1 등 표준 forecasting 벤치마크에 대한 최종 성능 비교 제시 안 됨 (optimizer 중심이므로 자연스러움)

※ 메모: 이 논문은 forecasting 아키텍처 자체 novelty보다 **학습 효율** 개선에 중점. 기존 모든 LTSF 모델에 orthogonal하게 적용 가능한 성격.

## 6. 참고

- **원본**: [PMLR proceedings](https://proceedings.mlr.press/v139/lu21d.html)
- **arXiv**: [2103.02062](https://arxiv.org/abs/2103.02062)
- **관련 라이브러리**: [GluonTS](https://github.com/awslabs/gluon-ts)
- **관련 논문**: 
  - Variance-reduced SGD (Johnson & Zhang, 2013)
  - Stratified sampling in machine learning
  - Heterogeneous time series forecasting literature
