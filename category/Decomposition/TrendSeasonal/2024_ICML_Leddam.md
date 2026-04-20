---
title: "Revitalizing Multivariate Time Series Forecasting: Learnable Decomposition with Inter-Series Dependencies and Intra-Series Variations Modeling"
authors: Guoqi Yu et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.12694
code_url: https://github.com/Levi-Ackman/Leddam
tags: [decomposition, trend-seasonal, inter-series, intra-series, dual-attention, time-domain]
primary_category: category/Decomposition/TrendSeasonal
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_Leddam.pdf
pdf_source: arxiv
---

## TL;DR
Leddam(LEarnable Decomposition and Dual Attention Module)은 학습 가능한 분해(Learnable Decomposition)로 동적 추세를 포착하고, 채널 간(inter-series) + 채널 내(intra-series) 이중 어텐션으로 다변량 예측 개선.

## 1. 기존 모델의 한계 / 가설
- 기존 이동평균 분해는 비선형 추세 구조 포착에 한계.
- 채널 간(inter-series) 의존성과 채널 내(intra-series) 시간 변화를 동시에 다루는 메커니즘 부재.
- 가설: 학습 가능한 분해 + 이중 어텐션으로 다변량 예측 성능 대폭 향상 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Learnable Decomposition + Dual Attention Module(채널 간 + 채널 내).
- **아키텍처**:
  - Learnable Decomposition: 비선형 추세를 학습 가능한 분해로 동적 포착
  - Channel-Wise Self-Attention: 채널 간 의존성 (inter-series)
  - Autoregressive Self-Attention: 채널 내 시간 변화 (intra-series)
  - plug-in 모듈로 기존 모델에 적용 가능 (11.87~48.56% MSE 감소)
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 기존 모델 대비 SOTA |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 학습 가능한 분해로 이동평균의 한계 극복
- 채널 간 + 채널 내 이중 어텐션 (Dual Attention Module)
- 기존 모델에 plug-in 적용 시 11.87~48.56% MSE 감소
- ETT, Electricity, Traffic, Weather 8개 데이터셋 SOTA

## 5. 한계 및 후속 연구 아이디어
- 이중 어텐션으로 계산 비용 증가
- 매우 다변량(>1000 변수) 시계열에서의 확장성 미검토

## 6. 참고
- [GitHub](https://github.com/Levi-Ackman/Leddam)
- [arXiv 2402.12694](https://arxiv.org/abs/2402.12694)
