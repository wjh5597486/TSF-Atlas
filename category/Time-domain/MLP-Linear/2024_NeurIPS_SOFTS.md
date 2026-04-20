---
title: "SOFTS: Efficient Multivariate Time Series Forecasting with Series-Core Fusion"
authors: Lu Han et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2404.14197
code_url: https://github.com/Secilia-Cxy/SOFTS
tags: [mlp, multivariate, channel-dependent, time-domain, star-module]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_SOFTS.pdf
pdf_source: arxiv
---

## TL;DR
SOFTS는 STar Aggregate-Redistribute(STAR) 모듈을 통해 다변량 시계열의 채널 간 의존성을 O(N) 복잡도로 모델링하는 MLP 기반 예측기. 중심화된 코어 표현으로 채널 정보를 집약·재분배하여 효율적인 다변량 예측을 달성.

## 1. 기존 모델의 한계 / 가설
- iTransformer 등 채널 의존(CD) 모델은 O(N²) self-attention으로 채널 수에 따른 확장성 문제가 있음.
- 채널 독립(CI) 모델은 채널 간 관계를 무시하여 다변량 특성을 활용하지 못함.
- 가설: 글로벌 코어 표현 하나로 채널 정보를 집약하면 선형 복잡도로 채널 의존성을 학습 가능.

## 2. 제안 방법론
- **핵심 아이디어**: STAR(STar Aggregate-Redistribute) 모듈 — 각 채널 임베딩을 집약하여 단일 코어 표현을 만들고, 이를 다시 각 채널에 재분배.
- **아키텍처**:
  - 시계열 패치 임베딩 (각 채널 독립)
  - STAR 모듈: Aggregate(평균/가중합) → Core representation → Redistribute(add/concat 후 MLP)
  - 복잡도 O(N) (N = 채널 수)
  - 다중 STAR 블록 스택 → 예측 head
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam, 표준 설정
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | iTransformer 대비 경쟁력 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- STAR 모듈로 채널 의존성을 O(N) 복잡도로 모델링
- 글로벌 코어 표현을 통한 채널 간 상호작용 효율적 포착
- iTransformer와 동등하거나 우수한 성능, 낮은 계산 비용
- 채널 수 증가에 따른 선형 확장성 확보

## 5. 한계 및 후속 연구 아이디어
- 단일 코어 표현이 다양한 채널 그룹의 복잡한 관계를 충분히 포착하는지 의문
- 동적 채널 관계 모델링으로 확장 가능

## 6. 참고
- [GitHub](https://github.com/Secilia-Cxy/SOFTS)
- [arXiv 2404.14197](https://arxiv.org/abs/2404.14197)
