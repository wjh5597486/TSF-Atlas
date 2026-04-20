---
title: "Learning Deep Time-index Models for Time Series Forecasting"
authors: Woo et al.
venue: ICML 2023
year: 2023
paper_url: https://arxiv.org/abs/2207.06046
code_url: https://github.com/salesforce/DeepTime
tags: [time-domain, mlp, implicit-neural-representation, meta-learning, non-stationary]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICML_DeepTime.pdf
pdf_source: arxiv
---

## TL;DR
DeepTime는 implicit neural representation (INR)을 deep time-index model로 활용하고 meta-optimization을 통해 학습하는 시계열 예측 프레임워크다. Non-stationarity에 강한 smoothness prior를 가지며 covariate shift를 피하고 sample efficiency가 뛰어나, 다양한 multivariate forecasting 벤치마크에서 state-of-the-art를 달성했다.

## 1. 기존 모델의 한계 / 가설
- 기존 딥러닝 기반 time series 모델은 고정된 컨텍스트 윈도우(look-back window)에서 미래 값을 직접 예측하는 방식으로, non-stationarity 문제(분포 이동)에 취약함
- 데이터 기반 모델은 covariate shift에 의해 훈련-테스트 분포 불일치가 발생
- Time-index model (implicit representation) 방식은 시계열 전체를 연속 함수로 모델링하지만, 기존 방식은 meta-optimization 없이 각 인스턴스에 독립적으로 fitting하여 sample efficiency가 낮음

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 연속 함수 $f_\theta(t)$로 표현하는 INR(Implicit Neural Representation)에 meta-learning(MAML-inspired)을 결합하여 적은 스텝으로 새 시계열에 빠르게 적응
- **아키텍처 구성요소**:
  - **Deep time-index model**: 시간 인덱스 $t$를 입력으로 받아 시계열 값을 출력하는 MLP 기반 INR
  - **Fourier Features Layer**: 고주파 패턴을 학습하기 위한 concatenated Fourier feature encoding 추가
  - **Ridge Regressor (closed-form)**: meta-optimization inner loop를 폐쇄형 ridge regression으로 대체하여 학습 효율 향상
  - **Meta-optimization**: 여러 time series 인스턴스들에 걸쳐 초기 파라미터 $\theta$를 학습하고, 예측 시 look-back window 데이터로 빠르게 adaptation
- **학습 전략**: gradient-based meta-learning (MAML-like) + inner loop는 closed-form ridge regression

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: 명시되지 않음 (논문의 preprocessing 설명 참고)
- Train/Val/Test split: 표준 ETTh1 분할
- Batch size / Optimizer / LR: Adam optimizer, 논문 실험 세팅 참고
- Loss function: MSE
- 기타: 6개 실제 데이터셋 (ETT ×4, Electricity, Exchange, Traffic, Weather, ILI) 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 20/24 settings SOTA |
| 192     | see paper | see paper | |
| 336     | see paper | see paper | |
| 720     | see paper | see paper | |

출처: Table 1-2 of paper (proceedings.mlr.press/v202/woo23b).

## 4. 주요 기여
- Time-index model (INR 기반)을 meta-optimization과 결합한 최초의 forecasting 프레임워크 제안
- Concatenated Fourier feature layer로 고주파 패턴 효율적 학습
- Inner loop를 closed-form ridge regression으로 대체하여 학습 안정성 및 속도 향상
- Non-stationarity에 강한 이론적 해석 제시 (smoothness prior, covariate shift 회피)
- 24개 설정 중 20개에서 SOTA 달성

## 5. 한계 및 후속 연구 아이디어
- Meta-optimization 훈련 비용이 기존 모델 대비 높을 수 있음
- 명시적인 채널 간 의존성(cross-channel dependency) 모델링 부재
- ※ 메모: INR 기반 접근은 irregular sampling이나 missing data 처리에 자연스럽게 확장 가능; multivariate INR로의 확장 연구 방향

## 6. 참고
- arXiv: https://arxiv.org/abs/2207.06046
- Official code: https://github.com/salesforce/DeepTime
- ICML 2023 poster page: https://icml.cc/virtual/2023/poster/24424
- PMLR proceedings: https://proceedings.mlr.press/v202/woo23b.html
