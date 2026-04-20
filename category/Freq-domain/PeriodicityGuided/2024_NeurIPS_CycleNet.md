---
title: "CycleNet: Enhancing Time Series Forecasting through Modeling Periodic Patterns"
authors: Shengsheng Lin et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2409.18479
code_url: https://github.com/ACAT-SCUT/CycleNet
tags: [periodicity, decomposition, mlp, linear, trend-seasonal, time-domain]
primary_category: category/Freq-domain/PeriodicityGuided
related_categories: [category/Decomposition/TrendSeasonal]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_CycleNet.pdf
pdf_source: arxiv
---

## TL;DR
CycleNet은 Residual Cycle Forecasting(RCF) 기법으로 학습 가능한 순환 사이클을 통해 주기 패턴을 명시적으로 모델링한 후, 잔차 성분을 예측하는 방식. NeurIPS 2024 Spotlight (Top 1%) 수상.

## 1. 기존 모델의 한계 / 가설
- 기존 모델은 주기성(periodicity)을 암묵적으로만 학습하며 명시적으로 모델링하지 않음.
- DLinear 등의 분해 방식은 추세와 계절성을 분리하지만, 정확한 반복 주기 패턴을 직접 파라미터화하지 않음.
- 가설: 시계열의 주기 성분을 전역 학습 가능한 순환 벡터로 모델링하면 잔차 예측이 쉬워짐.

## 2. 제안 방법론
- **핵심 아이디어**: Residual Cycle Forecasting(RCF) — 학습 가능한 주기 벡터(cycle)로 입력을 정규화한 뒤 잔차를 Linear/MLP로 예측.
- **아키텍처**:
  1. 채널 독립적으로 작동
  2. 전역 학습 가능한 순환 벡터 `W ∈ R^C` (C = cycle length) 유지
  3. 입력 시계열에서 해당 사이클 값을 빼서 잔차 계산
  4. Linear 또는 MLP로 잔차 예측 후, 예측 사이클 값과 합산
- **학습 전략**: MSE 손실, 채널 독립

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size: 표준; Optimizer: Adam; LR: 표준
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | PatchTST, iTransformer 대비 우수 또는 동등 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 주기 패턴 명시적 모델링을 위한 RCF 기법 제안 (plug-and-play)
- 90% 이상 파라미터 절감하면서 SOTA 달성
- PatchTST, iTransformer에 RCF 적용 시 일관된 성능 향상 확인
- NeurIPS 2024 Spotlight 수상 (상위 1%)

## 5. 한계 및 후속 연구 아이디어
- 주기 길이 C를 수동으로 설정해야 함 (자동 탐지 미지원)
- 비정상적(non-stationary) 시계열에서 고정 주기 가정이 깨질 수 있음

## 6. 참고
- [GitHub](https://github.com/ACAT-SCUT/CycleNet)
- [arXiv 2409.18479](https://arxiv.org/abs/2409.18479)
