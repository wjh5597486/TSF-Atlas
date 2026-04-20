---
title: "TimeBridge: Non-Stationarity Matters for Long-term Time Series Forecasting"
authors: Peiyuan Liu et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2410.04442
code_url: https://github.com/Hank0626/TimeBridge
tags: [time-domain, transformer, non-stationarity, cointegration, channel-dependent, long-term-forecasting]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent, category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimeBridge.pdf
pdf_source: arxiv
---

## TL;DR
TimeBridge bridges the gap between non-stationarity and dependency modeling in long-term time series forecasting by proposing Integrated Attention to mitigate short-term non-stationarity and Cointegrated Attention to model long-term cointegration across variates, achieving state-of-the-art performance on standard LTSF benchmarks.

## 1. 기존 모델의 한계 / 가설
- 기존 LTSF 모델들은 비정상성(non-stationarity)을 다루기 위해 간단한 정규화(RevIN 등)에 의존하며, 이는 장기 의존성 모델링에 한계가 있음.
- 단기 비정상성과 장기 공적분(cointegration) 관계를 동시에 처리하는 통합 프레임워크 부재.
- 금융 시계열(S&P 500, CSI 500)에서 특히 두드러지는 비정상성 패턴.

## 2. 제안 방법론
- **핵심 아이디어**: 두 가지 attention 메커니즘으로 단기/장기 비정상성을 동시 처리.
- **아키텍처**:
  - **Integrated Attention**: 단기 비정상성(local non-stationarity)을 완화하고 안정적인 의존성 포착.
  - **Cointegrated Attention**: 장기 공적분 관계를 보존하며 변수 간 장기 의존성 모델링.
  - Transformer 기반 백본.
- **손실함수**: MSE.
- **학습 전략**: Standard supervised LTSF.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (표준 설정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 자체 non-stationarity 처리 (Integrated Attention)
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, Solar, PEMS 등 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | best overall |
| 192     | see paper | see paper | best overall |
| 336     | see paper | see paper | best overall |
| 720     | see paper | see paper | best overall |

출처: Table 1 of paper (ETTh, ETTm, Weather, Electricity, Traffic, Solar 포함).

## 4. 주요 기여
- Integrated Attention과 Cointegrated Attention으로 단기/장기 비정상성 동시 처리.
- 금융 시계열(CSI 500, S&P 500) 등 강한 비정상성 데이터에서 강력한 성능.
- 기존 LTSF 벤치마크(ETT, Weather, Electricity, Traffic)에서 전반적 SOTA 달성.

## 5. 한계 및 후속 연구 아이디어
- Cointegration 기반 분석이 실제로 모든 유형의 비정상성을 포괄하는지 이론적 검증 필요.
- ※ 메모: 비정상성 처리를 attention 메커니즘 내부에 통합한 독특한 접근.

## 6. 참고
- 관련 논문: Non-stationary Transformer (NeurIPS 2022), iTransformer (ICLR 2024), FAN (NeurIPS 2024)
- GitHub: https://github.com/Hank0626/TimeBridge
- arXiv: https://arxiv.org/abs/2410.04442
