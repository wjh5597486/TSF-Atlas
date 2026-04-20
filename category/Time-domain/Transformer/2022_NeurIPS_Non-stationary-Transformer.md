---
title: "Non-stationary Transformers: Exploring the Stationarity in Time Series Forecasting"
authors: Yong Liu et al.
venue: NeurIPS
year: 2022
paper_url: https://arxiv.org/abs/2205.14415
code_url: https://github.com/thuml/Nonstationary_Transformers
tags: [time-domain, transformer, normalization, stationarity, de-stationary-attention]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_NeurIPS_Non-stationary-Transformer.pdf
pdf_source: arxiv
---

## TL;DR
Non-stationary Transformers address the critical issue of distribution shift in time series data by proposing Series Stationarization (statistical normalization) and De-stationary Attention (information recovery mechanism). The framework significantly boosts mainstream Transformers (49.43% MSE reduction for Transformer, 47.34% for Informer, 46.89% for Reformer) and achieves SOTA on multiple benchmarks.

## 1. 기존 모델의 한계 / 가설
- Transformer가 전역 모델링에는 우수하지만, 실제 비정상(non-stationary) 시계열에서 성능 저하
- 시계열의 joint distribution이 시간에 따라 변하는 문제를 Transformer가 제대로 처리하지 못함
- 가설: 입출력 정규화(stationarization) + 비정상 정보 복구(de-stationary attention)의 조합으로 기존 Transformer 성능 획기적 개선 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
두 가지 상호보완적 메커니즘:
1. **Series Stationarization**: 입출력을 정상(stationary) 형태로 변환하여 모델의 학습 난이도 감소
2. **De-stationary Attention**: 정상화 과정에서 손실된 비정상 정보를 attention에 통합하여 복구

### 2.2 아키텍처 구성

**2.2.1 Series Stationarization**
- 각 입력 시계열에서 평균(mean)과 분산(variance) 계산
- 정규화: X_norm = (X - μ) / σ
- 모델은 X_norm을 학습하여 stable한 분포에서 예측
- 출력은 원본 분포로 복원: Y_output = Y_norm × σ + μ

**2.2.2 De-stationary Attention**
- 표준 Transformer attention 계산: Q, K, V 행렬 생성
- **차이점**: Attention 계산 시 **τ (learnable scalar)** 를 도입
  - Attention weights에 τ를 곱하여 시간에 따른 의존성 강도 조절
  - τ_t = learnable parameter that captures temporal dependencies in non-stationary series
- 이를 통해 정상화로 인한 정보 손실을 보정

**2.2.3 적용 방식**
- 기존 Transformer 아키텍처에 상기 2가지 메커니즘만 추가
- 다른 attention 메커니즘(multi-head attention, positional encoding 등)은 유지
- 다양한 Transformer 변형(Transformer, Informer, Reformer, Autoformer 등)에 적용 가능

### 2.3 주요 특징
- **모듈화**: RevIN 대신 series stationarization 사용 (학습 가능한 정규화)
- **일반성**: 기존 Transformer 기반 모델들에 추가 학습 파라미터 최소화하면서 적용
- **해석가능성**: τ 분석으로 시계열의 비정상성 정도 파악 가능

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96 (주요), 192, 336, 720도 평가
- Forecast horizons: 96, 192, 336, 720
- Normalization: Series Stationarization (평균, 분산 기반)
- Train/Val/Test split: 7:1:2 (일부 setting) 또는 8:1:1
- Batch size: 32
- Optimizer: Adam, lr = 0.0001
- Epochs: 100
- Early stopping: patience = 20
- Loss function: MSE
- 기타: RevIN과 비교, 여러 Transformer 베이스라인 보강

### 3.2 Results
상세 수치는 원문 Table 3, Appendix C.1 (ETTh1/2, ETTm1/2) 참고.

**주요 성과:**
- **Transformer + Non-stationary**: 49.43% MSE 감소
- **Informer + Non-stationary**: 47.34% MSE 감소
- **Reformer + Non-stationary**: 46.89% MSE 감소
- **Autoformer + Non-stationary**: 10.57% MSE 감소
- **ETSformer + Non-stationary**: 5.17% MSE 감소
- **FEDformer + Non-stationary**: 4.51% MSE 감소
- ETTm1 (input-96-predict-336): 4.4% MSE 감소 (베이스라인 대비)

| Horizon | 개선율(%)        | 특이사항           |
|---------|------------------|------------------|
| 96      | ~5-10%           | 상대적으로 적음   |
| 192     | ~10-20%          |                  |
| 336     | ~20-30%          | 주요 개선 구간   |
| 720     | ~30-50%          | 극대 개선        |

*(자세한 MSE/MAE 수치는 원문 Table 3, Appendix C.1 참고)*

## 4. 주요 기여
- 비정상 시계열이 Transformer 성능을 제한하는 핵심 요인 규명
- 간단하면서도 효과적인 Series Stationarization + De-stationary Attention 프레임워크 제시
- 기존 Transformer 계열 모델들(Informer, Reformer, Autoformer 등)에 평준화된 개선 효과 입증
- 광범위한 실험으로 정규화와 비정상 정보 복구의 상호작용 규명

## 5. 한계 및 후속 연구 아이디어
- De-stationary attention의 τ가 실제 non-stationarity를 얼마나 정확히 포착하는지 분석 필요
- 극도로 변동성 높은 금융/에너지 시계열에 대한 강건성 추가 검증
- 다중 스케일 시계열에 대한 stationarization 전략 최적화
- ※ 메모: τ를 시간에 따라 변하는 함수로 만들면 추가 개선 가능할 것 추측되나, 학습 안정성 trade-off 고려 필요

## 6. 참고
- [GitHub Repository](https://github.com/thuml/Nonstationary_Transformers)
- arXiv preprint: 2205.14415
- 관련 논문: Transformer, Informer, Reformer, Autoformer, RevIN
- ETT dataset 상세 정보: Electricity Transformer Temperature (油温 + 6 features) 2016-2018
- Appendix C: ETTh1, ETTh2, ETTm1, ETTm2, Exchange_rate, Electricity, Weather 등 상세 결과표
