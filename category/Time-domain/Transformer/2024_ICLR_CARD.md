---
title: "CARD: Channel Aligned Robust Blend Transformer for Time Series Forecasting"
authors: Xue Wang, Tian Zhou, Qingsong Wen, Jinyang Gao, Bolin Ding, Rong Jin
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2305.12095
code_url: https://github.com/wxie9/CARD
tags: [time-domain, transformer, channel-dependent, loss]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_CARD.pdf
pdf_source: arxiv
---

## TL;DR
채널 간 정렬 + 인접 토큰 **blend**로 강건한 multivariate Transformer. Signal decomposition 기반의 **분산 가중 MSE loss**를 제안해 horizon-평균 품질 향상.

## 1. 기존 모델의 한계 / 가설
- Channel-independent(PatchTST)는 단순하지만 변수 간 정보를 활용 못 함.
- 단순 channel-dependent는 과적합 위험 → 정렬과 blending으로 완화 가능.

## 2. 제안 방법론
- **Token blend** 모듈로 인접 patch 정보 융합.
- Variate-wise alignment attention.
- **Signal decomposition loss**: 미래 구간의 분산 가중 MSE.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Lookback = 96. Horizons = {96, 192, 336, 720}.

### 3.2 Results (Table 1, ETTh1)
- Avg MSE ≈ 0.443, Avg MAE ≈ 0.429 (CARD 열 기준).
- 상세 per-horizon은 원문 Table 1 참고.

## 4. 주요 기여
- Channel-dependent Transformer + token blending.
- 분산 가중 loss로 long-horizon 품질 개선.

## 5. 참고
- 코드: https://github.com/wxie9/CARD
