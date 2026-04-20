---
title: "SparseTSF: Modeling Long-term Time Series Forecasting with 1k Parameters"
authors: Shengsheng Lin et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2405.00946
code_url: https://github.com/lss-1138/SparseTSF
tags: [mlp, linear, sparse, cross-period, time-domain, lightweight]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/Decomposition/TrendSeasonal]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_SparseTSF.pdf
pdf_source: arxiv
---

## TL;DR
SparseTSF는 1k 미만 파라미터로 장기 시계열 예측을 달성하는 초경량 모델. Cross-Period Sparse Forecasting 기법으로 주기성과 추세를 분리하여 주기 간 다운샘플링 후 추세만 예측. ICML 2024 Oral (상위 1.5%).

## 1. 기존 모델의 한계 / 가설
- 최신 LTSF 모델들은 수백만 파라미터를 사용하여 소규모 데이터나 edge 환경에 부적합.
- 단순 Linear 모델이 놀라운 성능을 보이는 이유가 실제로 주기 추세만 예측하기 때문임을 관찰.
- 가설: 시계열의 주기성을 명시적으로 활용하여 주기 간 추세만 예측하면 파라미터를 극단적으로 줄일 수 있음.

## 2. 제안 방법론
- **핵심 아이디어**: Cross-Period Sparse Forecasting — 주기 P로 다운샘플링하여 주기 간 포인트만 선택, Linear 레이어로 추세 예측 후 업샘플링.
- **아키텍처**:
  - 입력 시계열을 주기 P로 stride 다운샘플링 (주기 내 변동 제거)
  - 단일 가중치 공유 Linear 레이어로 각 채널 독립 예측
  - 업샘플링으로 전체 예측 복원
  - 전체 파라미터 < 1k
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size: 표준; Optimizer: Adam
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.359 | 0.391 | PatchTST 대비 우수 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 1k 미만 파라미터로 SOTA 수준 LTSF 달성
- Cross-Period Sparse Forecasting 기법으로 주기성-추세 분리
- 소규모 데이터/저품질 데이터에서의 우수한 일반화
- ICML 2024 Oral 수상 (상위 1.5%)

## 5. 한계 및 후속 연구 아이디어
- 주기 길이 P를 수동으로 설정해야 함
- 비주기적 불규칙 시계열에서 효과 감소

## 6. 참고
- [GitHub](https://github.com/lss-1138/SparseTSF)
- [arXiv 2405.00946](https://arxiv.org/abs/2405.00946)
