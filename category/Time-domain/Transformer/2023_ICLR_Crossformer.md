---
title: "Crossformer: Transformer Utilizing Cross-Dimension Dependency for Multivariate Time Series Forecasting"
authors: Yunhao Zhang et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2202.07125
code_url: https://github.com/Thinklab-SJTU/Crossformer
tags: [time-domain, transformer, channel-dependent, cross-dimension, multi-scale]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_Crossformer.pdf
pdf_source: arxiv
---

## TL;DR
Crossformer는 다변량 시계열의 cross-dimension(변수 간) 의존성을 명시적으로 모델링하는 Transformer로, Dimension-Segment-Wise(DSW) 임베딩과 Two-Stage Attention(TSA)을 통해 시간 축과 변수 축의 의존성을 계층적으로 포착한다. 6개 실세계 데이터셋에서 당시 SOTA를 달성했으나, 이후 iTransformer에 의해 채널 독립 방향의 우위가 재조명됨.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델(Informer, Autoformer, FEDformer)은 각 time step을 독립 토큰으로 처리하여 변수 간 의존성을 무시함.
- 채널 독립(CI) 방식은 multivariate correlation을 활용하지 못함.
- 가설: 다변량 시계열에서 cross-dimension dependency를 명시적으로 포착하면 예측 정확도가 향상됨.

## 2. 제안 방법론
- **핵심 아이디어**: 2D 벡터 어레이 임베딩(시간×변수) + Two-Stage Attention으로 cross-time과 cross-dimension 의존성을 동시에 포착.
- **Dimension-Segment-Wise (DSW) Embedding**: 입력 MTS를 L/S개 세그먼트(segment 길이 L_seg)로 분할하고, 각 (세그먼트, 변수) 쌍을 토큰으로 임베딩 → 2D array [D, L/L_seg].
- **Two-Stage Attention (TSA)**:
  1. Cross-time attention: 동일 변수의 서로 다른 시간 세그먼트 간 attention.
  2. Cross-dimension attention: 동일 시간의 서로 다른 변수 간 attention (router mechanism으로 효율화).
- **Hierarchical Encoder-Decoder**: Encoder는 세그먼트를 병합하여 계층적 스케일; Decoder는 각 스케일에서 예측하여 합산.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 168 (7일 = 168 hours)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: Z-score (zero-mean, unit-variance)
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: batch size 32, Adam, lr 0.0001
- Loss function: MSE
- 기타: segment length L_seg=6, d_model=256, n_heads=4, e_layers=3, d_ff=512

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | 0.404 | 0.426 | FEDformer 0.376/0.419 대비 소폭 높음 |
| 192     | 0.450 | 0.451 | FEDformer 0.420/0.448 대비 소폭 높음 |
| 336     | 0.532 | 0.515 | — |
| 720     | 0.666 | 0.589 | — |

출처: Table 1 of paper (OpenReview: vSVLM2j9eie). ※ 메모: ETTh1에서 PatchTST, TimesNet보다 성능이 낮은 경우가 많음; 크로스 차원 의존성 모델링의 ETTh1 효과는 제한적.

## 4. 주요 기여
- Cross-dimension dependency를 명시적으로 모델링한 최초의 주요 Transformer TSF 모델 중 하나.
- DSW 임베딩으로 시간-변수 2D 구조를 Transformer 입력으로 자연스럽게 표현.
- Router 메커니즘으로 cross-dimension attention을 효율화 (O(D·router_num) 복잡도).
- Hierarchical encoder-decoder로 다중 스케일 패턴 포착.

## 5. 한계 및 후속 연구 아이디어
- ETTh1 등 일부 데이터셋에서 채널 독립 방식(PatchTST) 대비 성능 열세.
- 변수 수(D) 증가 시 cross-dimension attention의 복잡도 문제.
- ※ 메모: iTransformer는 Crossformer와 반대 방향 (변수를 토큰으로)으로 더 효과적인 채널 모델링을 보임.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=vSVLM2j9eie
- 공식 구현: https://github.com/Thinklab-SJTU/Crossformer
- OpenReview: https://openreview.net/forum?id=vSVLM2j9eie
- arXiv: https://arxiv.org/abs/2202.07125
