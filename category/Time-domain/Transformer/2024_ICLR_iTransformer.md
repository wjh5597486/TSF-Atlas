---
title: "iTransformer: Inverted Transformers Are Effective for Time Series Forecasting"
authors: Yong Liu, Tengge Hu, Haoran Zhang, Haixu Wu, Shiyu Wang, Lintao Ma, Mingsheng Long
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2310.06625
code_url: https://github.com/thuml/iTransformer
tags: [time-domain, transformer, channel-dependent, inverted-attention]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_iTransformer.pdf
pdf_source: arxiv
---

## TL;DR
표준 Transformer 구조를 그대로 두되 **축을 뒤집어** 각 변수(variate)의 전체 시계열을 하나의 토큰으로 임베딩한다. Attention은 변수 간 상관, FFN은 변수별 비선형 표현을 학습 → 구조 수정 없이 다변량 TSF SOTA 달성.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer-TSF는 같은 시점의 여러 변수를 합쳐 하나의 temporal token을 만들어, 물리적으로 이질적인 변수가 섞여 **attention map이 무의미**해진다.
- Lookback을 늘리면 성능이 오히려 떨어지고 계산량이 폭발.

## 2. 제안 방법론
- **Inverted embedding**: 각 변수의 길이-T 시계열 전체를 1개 variate token으로 projection.
- **Attention on variates**: 변수 간 상관관계 포착.
- **FFN per variate**: 각 변수별 비선형 표현 학습.
- 나머지는 바닐라 Transformer — LayerNorm/Position 인코딩 불필요.

## 3. ETTh1 실험 결과

### 3.1 Setting
- Lookback T = **96** (고정)
- Horizons S = {96, 192, 336, 720}
- Split: (8545, 2881, 2881) train/val/test, 정규화는 채널별 z-score (코드 기준)
- Loss: L2 (MSE), Optimizer: Adam. 상세 하이퍼파라미터는 원문 Table 12 및 코드 참고.

### 3.2 Results (Table 2, ETTh1)
| Horizon | MSE   | MAE   |
|---------|-------|-------|
| 96      | 0.386 | 0.405 |
| 192     | 0.441 | 0.436 |
| 336     | 0.487 | 0.458 |
| 720     | 0.503 | 0.491 |
| Avg     | 0.454 | 0.447 |

출처: Table 2 of paper.

## 4. 주요 기여
- Transformer 컴포넌트를 **수정 없이 축만 뒤집어** multivariate TSF에서 SOTA.
- 긴 lookback 활용성과 unseen variate 일반화 개선.
- "iTransformer" 프레임워크를 다른 Transformer backbone에도 플러그인 가능함을 제시.

## 5. 참고
- 공식 코드: https://github.com/thuml/iTransformer
