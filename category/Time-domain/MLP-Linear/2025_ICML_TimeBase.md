---
title: "TimeBase: The Power of Minimalism in Efficient Long-term Time Series Forecasting"
authors: Qihe Huang et al.
venue: ICML
year: 2025
paper_url: ""
code_url: https://github.com/hqh0728/TimeBase
tags: [time-domain, mlp, minimalist, periodicity, basis, segment-level, efficient]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/Freq-domain/PeriodicityGuided]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimeBase.pdf
pdf_source: none
---

## TL;DR
TimeBase (ICML 2025 Spotlight) is an ultra-lightweight network (<0.4K parameters) for long-term time series forecasting that extracts core basis temporal components from periodic structures, transforming point-level forecasting into efficient segment-level forecasting. It achieves SOTA on 28 prediction settings while reducing MACs by 35x.

## 1. 기존 모델의 한계 / 가설
- 기존 LTSF 모델들은 많은 파라미터로 장기 의존성을 포착하려 하지만, 실제로는 시계열 데이터에 temporal pattern similarity와 low-rank structure가 내재함.
- 불필요하게 복잡한 모델은 overfitting과 계산 낭비를 유발.
- 핵심 주기 패턴만 추출해도 충분한 예측 성능 달성 가능하다는 최소주의 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Temporal Pattern Similarity와 Low-rank Structure를 활용하여 core basis temporal components를 추출 후 segment-level 예측 수행.
- **아키텍처**:
  - **Basis Extraction**: Orthogonality constraint 하에서 full-rank typical period representation 추출.
  - **Segment-level Forecasting**: 포인트별 예측 대신 세그먼트 단위 예측으로 효율성 향상.
  - <0.4K 파라미터 (linear model보다 1000배 이상 파라미터 절감).
  - Plug-and-play 복잡도 감소기로 패치 기반 모델에 적용 가능.
- **손실함수**: MSE.
- **학습 전략**: 경량 지도 학습.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (다중 설정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic 포함 17개 벤치마크

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | Top1-Top5 (28개 설정) |
| 192     | see paper | see paper | Top1-Top5 (28개 설정) |
| 336     | see paper | see paper | Top1-Top5 (28개 설정) |
| 720     | see paper | see paper | Top1-Top5 (28개 설정) |

출처: Table of paper (17 benchmark datasets, 28 prediction settings).

## 4. 주요 기여
- 0.4K 미만의 파라미터로 SOTA 예측 성능 달성 (ICML 2025 Spotlight).
- MACs 35x 감소, 파라미터 1000배 이상 감소 vs. linear models.
- 모든 28개 예측 설정에서 Top1-Top5 달성.
- Plug-and-play: 패치 기반 모델의 복잡도 감소기로 활용 가능.

## 5. 한계 및 후속 연구 아이디어
- 최소주의 설계는 complex non-periodic 패턴 포착에 한계.
- ※ 메모: SparseTSF(ICML 2024)의 minimal parameter LTSF 방향을 더욱 극단화한 접근.

## 6. 참고
- 관련 논문: SparseTSF (ICML 2024), DLinear (ICLR 2023 NLP 논문), CycleNet (NeurIPS 2024)
- GitHub: https://github.com/hqh0728/TimeBase
- PMLR: https://proceedings.mlr.press/v267/huang25az.html
- OpenReview: https://openreview.net/forum?id=GhTdNOMfOD
- ICML 2025 Spotlight Poster: https://icml.cc/virtual/2025/poster/45815
- ※ 원본 PDF 미취득: arxiv preprint 없음, PMLR 직접 접근 필요
