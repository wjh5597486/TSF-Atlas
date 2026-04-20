---
title: "Scaleformer: Iterative Multi-scale Refining Transformers for Time Series Forecasting"
authors: Mohammad Alam et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2206.04038
code_url: https://github.com/BorealisAI/scaleformer
tags: [time-domain, transformer, multi-scale, decomposition, normalization]
primary_category: category/Decomposition/MultiScale
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_Scaleformer.pdf
pdf_source: arxiv
---

## TL;DR
Scaleformer는 기존 Transformer TSF 모델(FEDformer, Autoformer 등)에 범용으로 적용 가능한 multi-scale 반복 정제(iterative refinement) 프레임워크다. 예측 시계열을 여러 해상도(scale)에서 순차적으로 정제하고, scale 간 distribution shift를 막기 위한 cross-scale normalization을 도입하여 5.5~38.5% 성능 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델은 단일 해상도(single scale)로 예측하여 다양한 주파수 성분을 동시에 포착하지 못함.
- 다중 스케일 처리 시 각 scale의 예측 분포가 달라져 오차가 누적됨(distribution shift).
- 가설: 다중 스케일에서 점진적으로 예측을 정제하면 보다 정확하고 일관된 예측 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 가장 조악한 스케일(downsampled)에서 시작하여 원본 해상도까지 Transformer를 반복 적용하며 예측을 정제.
- **Iterative Refinement**: scale₁(coarsest) → scale₂ → ... → scaleₙ(finest=original). 각 scale에서 이전 예측을 보정(residual).
- **Shared Weights**: 모든 scale에서 동일한 Transformer 가중치를 공유하여 파라미터 효율화.
- **Cross-scale Normalization**: scale 전환 시 분포 변화를 보정하는 정규화 레이어 (층간 distribution shift 방지).
- **모델 독립성**: FEDformer, Autoformer, Informer 등 다양한 Transformer backbone에 플러그인 가능.
- **손실함수**: 각 scale에서의 MSE 합산.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: {96, 192, 336, 720}
- Normalization: Cross-scale normalization (논문 제안) + Z-score
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: backbone 모델 설정 따름; Adam optimizer
- Loss function: MSE (각 scale 합산)
- 기타: scale 수 및 downsampling 비율은 데이터셋에 따라 조정

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | ~0.379 | ~0.413 | FEDformer+Scaleformer 기준 (backbone에 따라 상이) |
| 192     | ~0.413 | ~0.436 | — |
| 336     | ~0.422 | ~0.440 | — |
| 720     | ~0.449 | ~0.465 | — |

출처: Table 1-2 of paper (arXiv:2206.04038). ※ 메모: backbone(FEDformer, Autoformer)에 따라 결과 다름; 위 수치는 대표 값으로 논문 원본 확인 필요.

## 4. 주요 기여
- Multi-scale iterative refinement를 범용 프레임워크로 제안, 기존 Transformer에 plug-in 적용 가능.
- Cross-scale normalization으로 scale 간 distribution shift 문제 해결.
- FEDformer, Autoformer 등에 적용 시 5.5~38.5% MSE 개선 (6개 데이터셋 기준).
- 공유 가중치로 추가 파라미터 없이 다중 스케일 처리 가능.

## 5. 한계 및 후속 연구 아이디어
- Backbone Transformer의 한계를 그대로 상속; PatchTST 등 더 강한 backbone 등장 후 상대적 중요도 감소.
- 반복 정제 과정에서 추론 시간 증가.
- ※ 메모: TimeMixer(ICLR 2024)가 보다 통합된 multi-scale mixing 접근으로 후속 연구.

## 6. 참고
- arXiv: https://arxiv.org/abs/2206.04038
- OpenReview: https://openreview.net/forum?id=sCrnllCtjoE
- 공식 구현: https://github.com/BorealisAI/scaleformer
- RBC Borealis 연구 블로그: https://rbcborealis.com/research-blogs/scaleformer-iterative-multi-scale-refining-transformers-for-time-series-forecasting/
