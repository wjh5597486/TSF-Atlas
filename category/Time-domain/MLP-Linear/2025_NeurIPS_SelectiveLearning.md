---
title: "Selective Learning for Deep Time Series Forecasting"
authors: Yisong Fu et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2510.25207
code_url: ""
tags: [loss, training-strategy, overfitting, uncertainty, anomaly, model-agnostic]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_SelectiveLearning.pdf
pdf_source: arxiv
---

## TL;DR
Selective Learning은 MSE 손실 계산 시 전체 타임스텝이 아닌 일반화 가능한 타임스텝만 선택적으로 사용하는 모델 비종속(model-agnostic) 학습 전략이다. 불확실성 마스크와 이상치 마스크로 구성된 이중 마스킹 메커니즘으로 오버피팅을 방지하며, Informer·TimesNet·iTransformer 등에 적용 시 MSE를 최대 37.4% 감소시킨다.

## 1. 기존 모델의 한계 / 가설
- 딥러닝 기반 TSF 모델은 시계열의 노이즈와 이상치에 취약하여 심각한 오버피팅 발생.
- 기존 모델들은 모든 타임스텝을 동등하게 MSE 손실에 포함하여 비일반화 타임스텝도 학습.
- 가설: 불확실하거나 이상치인 타임스텝을 MSE 계산에서 제외하면 일반화 성능이 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: MSE 손실 계산 시 일반화 가능한 타임스텝만 선택적으로 포함(Selective Learning).
- **이중 마스킹 메커니즘 (추정기 모델의 잔차 분포로부터 계산)**:
  - **불확실성 마스크(Uncertainty Mask)**: Residual Entropy를 이용해 불확실한 타임스텝 필터링 (비율 하이퍼파라미터 r_u).
  - **이상치 마스크(Anomaly Mask)**: Residual Lower Bound Estimation을 이용해 이상치 타임스텝 제외 (비율 하이퍼파라미터 r_a).
-  **2단계 파이프라인**: 추정기(estimator) 모델을 먼저 학습해 잔차 통계 산출 → 본 모델 학습 시 해당 통계 기반 마스크를 loss에 적용.
- **Model-Agnostic**: 기존 TSF 모델에 추가 구조 변경 없이 적용 가능.
- **적용 결과**: Informer 37.4% MSE 감소, TimesNet 8.4%, iTransformer 6.5%.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 기본 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: 기본 모델 설정 따름
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 기본 모델 설정 따름
- Loss function: Selective MSE (선택적 타임스텝 기반)
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 등 8개 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper (backbone 모델 대비 개선) |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: Selective Learning은 기존 모델에 적용되어 성능 향상을 보고; 절대값보다는 상대적 개선폭이 핵심.

## 4. 주요 기여
- 선택적 타임스텝 MSE 손실 계산을 통한 오버피팅 방지 전략 제안.
- 불확실성 마스크 + 이상치 마스크의 이중 메커니즘.
- Model-Agnostic: 다양한 기존 TSF 모델에 플러그인 방식으로 적용 가능.
- Informer(37.4%), TimesNet(8.4%), iTransformer(6.5%) MSE 감소.

## 5. 한계 및 후속 연구 아이디어
- 선택 기준(엔트로피, lower bound)의 하이퍼파라미터 설정에 민감할 수 있음.
- 자동화된 마스킹 임계값 결정 방법 연구 필요.

## 6. 참고
- arxiv: https://arxiv.org/abs/2510.25207
- OpenReview: https://openreview.net/forum?id=kgzRy6nD6D
