---
title: "ModernTCN: A Modern Pure Convolution Structure for General Time Series Analysis"
authors: Donghao Luo, Xue Wang
venue: ICLR
year: 2024
paper_url: https://openreview.net/forum?id=vpJMJerXHU
code_url: https://github.com/luodhhh/ModernTCN
tags: [time-domain, cnn, tcn, large-kernel]
primary_category: category/Time-domain/CNN-TCN
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_ModernTCN.pdf
pdf_source: publisher
---

## TL;DR
순수 Convolution 기반으로 **큰 커널 + depthwise + ConvFFN** 구조(ConvNeXt/Modern CNN 철학을 TSF로 이식)를 제안. 5개 TS 과제(long/short forecasting, imputation, classification, anomaly) 모두 SOTA 수준.

## 1. 기존 모델의 한계 / 가설
- Transformer 대비 CNN이 TSF에서 부진한 이유는 구조적 한계가 아니라 "현대적 설계"가 부족했기 때문이라는 가설.
- 작은 커널·일반 TCN으로는 ERF(effective receptive field)가 좁아 장기 의존성 포착 실패.

## 2. 제안 방법론
- **Large kernel DWConv** + **ConvFFN** 블록 반복.
- Channel-independent 처리.
- 입력 길이 = 2 × 예측 길이 (동적).

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length = **2 × horizon** (possible input pool에서 best 선택).
- Horizons = {96, 192, 336, 720}.

### 3.2 Results (Table 2, ETTh1 Avg)
| Metric | ModernTCN |
|--------|-----------|
| MSE    | 0.404     |
| MAE    | 0.420     |

## 4. 주요 기여
- ConvNeXt의 현대 CNN 설계를 1D 시계열에 적용, pure-conv로 Transformer·MLP 계열과 경쟁.
- 단일 아키텍처로 5개 TSA 과제 커버.

## 5. 참고
- 코드: https://github.com/luodhhh/ModernTCN
