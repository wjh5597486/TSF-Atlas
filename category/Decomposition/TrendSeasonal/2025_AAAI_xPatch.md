---
title: "xPatch: Dual-Stream Time Series Forecasting with Exponential Seasonal-Trend Decomposition"
authors: Artyom Stitsyuk et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2412.17323
code_url: https://github.com/stitsyuk/xPatch
tags: [time-domain, decomposition, trend-seasonal, mlp, cnn, dual-stream, patching, loss]
primary_category: category/Decomposition/TrendSeasonal
related_categories:
  - category/CrossCutting/Patching
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_xPatch.pdf
pdf_source: arxiv
---

## TL;DR
xPatch는 지수 이동 평균(EMA) 기반 계절-추세 분해 모듈, MLP 선형 스트림과 CNN 비선형 스트림의 이중 구조, arctangent 손실 함수, sigmoid 학습률 조정을 결합한 비-Transformer 예측 모델이다. 9개 데이터셋에서 unified setting 기준 최고 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델은 연산 비용이 높고 LTSF에서 항상 최선이 아님
- 기존 지수 평활 방법(ES)은 딥러닝과의 통합이 어색함
- 분해 방법(이동 평균)이 표준이나, EMA 기반 분해가 더 적응적일 수 있음
- 가설: EMA 분해 + 이중 스트림(선형+비선형) 구조가 다양한 시계열 패턴을 효과적으로 캡처

## 2. 제안 방법론
- **핵심 아이디어**: EMA 기반 분해 + 선형/비선형 이중 스트림 + 커스텀 손실/LR
- **아키텍처 구성요소**:
  1. **Exponential Seasonal-Trend Decomposition Module**: EMA로 추세/계절 성분 분리
  2. **MLP Linear Stream**: 선형 관계 모델링 (채널 독립)
  3. **CNN Non-linear Stream**: 비선형 관계 및 로컬 패턴 모델링
  4. **Patching**: 두 스트림 모두에 패치 기법 적용
  5. **Arctangent Loss Function**: 이상치에 강건한 커스텀 손실
  6. **Sigmoid LR Adjustment**: 과적합 방지를 위한 적응적 학습률 조정
- **손실함수**: $\mathcal{L}_{arctan}(y, \hat{y}) = \frac{2}{\pi}\arctan(|y - \hat{y}|)$

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): L=96 (unified), L≥96 (hyperparameter search)
- Forecast horizons: T={96, 192, 336, 720}; ILI: T={24, 36, 48, 60}
- Normalization: RevIN
- Train/Val/Test split: 표준 chronological split
- Batch size / Optimizer / LR: Quadro RTX 6000 GPU, PyTorch, sigmoid LR scheduler
- Loss function: Arctangent loss (커스텀)
- 기타: 9개 데이터셋 (ETTh1/h2/m1/m2, Weather, Traffic, Electricity, Exchange, Solar-Energy, ILI)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg (unified) | 0.428 | 0.419 | best among tested models |
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper (unified settings).

## 4. 주요 기여
- 지수 이동 평균 기반 계절-추세 분해 모듈 (EMA Decomposition) 제안
- 선형(MLP) + 비선형(CNN) 이중 스트림 구조로 다양한 시계열 특성 처리
- 이상치에 강건한 Arctangent 손실 함수 도입
- Sigmoid 기반 적응적 학습률 조정 기법
- 9개 벤치마크에서 최고 수준 성능

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: EMA decay 계수 등 hyperparameter 튜닝 필요
- ※ 메모: primary는 Decomposition/TrendSeasonal (EMA 분해가 핵심 novelty); 패칭과 커스텀 손실은 CrossCutting 심볼릭

## 6. 참고
- [arXiv 2412.17323](https://arxiv.org/abs/2412.17323)
- [GitHub](https://github.com/stitsyuk/xPatch)
- 비교 논문: PatchTST, TimeMixer, DLinear, iTransformer
