---
title: "LightGTS: A Lightweight General Time Series Forecasting Model"
authors: (authors not publicly listed in abstract)
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2506.06005
code_url: ""
tags: [time-domain, mlp, lightweight, periodicity, tokenization, zero-shot, foundation]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/Freq-domain/PeriodicityGuided]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_LightGTS.pdf
pdf_source: arxiv
---

## TL;DR
LightGTS is a lightweight general time series forecasting model designed around consistent periodical modeling. It introduces Periodical Tokenization to extract consistent periodic patterns across datasets with varying scales, and Periodical Parallel Decoding to improve forecasting accuracy. It achieves SOTA on 9 real-world benchmarks in both zero-shot and full-shot settings with high efficiency compared to existing foundation models.

## 1. 기존 모델의 한계 / 가설
- 기존 대형 foundation model들(TimesFM, Moirai 등)은 zero-shot 성능을 위해 막대한 파라미터와 계산 비용을 요구.
- 주기성(periodicity)이 시계열 데이터의 핵심 특성이지만, 다양한 스케일의 데이터셋에서 일관된 주기 패턴 추출이 어려움.
- 경량 설계로도 충분한 zero-shot 및 full-shot 성능 달성 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Periodical Tokenization으로 다양한 스케일의 데이터에서 일관된 주기 패턴 추출, Periodical Parallel Decoding으로 과거 토큰을 활용한 예측 개선.
- **아키텍처**:
  - **Periodical Tokenization**: 주기 기반 토큰화로 consistent periodic pattern 추출.
  - **Periodical Parallel Decoding**: 역사적 주기 토큰을 병렬로 디코딩.
  - 경량 MLP 백본.
- **손실함수**: MSE.
- **학습 전략**: Full-shot 및 zero-shot 모두 지원.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 9개 실제 벤치마크 (ETTh1/2, ETTm1/2 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA (zero-shot + full-shot) |
| 192     | see paper | see paper | SOTA (zero-shot + full-shot) |
| 336     | see paper | see paper | SOTA (zero-shot + full-shot) |
| 720     | see paper | see paper | SOTA (zero-shot + full-shot) |

출처: Table of paper (9 benchmarks, zero-shot & full-shot).

## 4. 주요 기여
- 경량 설계로 대형 foundation model 대비 우수한 효율성.
- Periodical Tokenization으로 다양한 스케일에서 일관된 주기 패턴 추출.
- Zero-shot 및 full-shot 모두에서 SOTA 달성.

## 5. 한계 및 후속 연구 아이디어
- 명시적 주기 가정이 비주기적 데이터에서는 제한적.
- ※ 메모: TimesFM(ICML 2024)의 foundation model 철학을 경량화하는 방향.

## 6. 참고
- 관련 논문: TimesFM (ICML 2024), CycleNet (NeurIPS 2024), LightTS (NeurIPS 2022)
- arXiv: https://arxiv.org/abs/2506.06005
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44879
