---
title: "BasisFormer: Attention-based Time Series Forecasting with Learnable and Interpretable Basis"
authors: Zelin Ni et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2310.20496
code_url: https://github.com/nzl5116190/Basisformer
tags: [transformer, attention, basis, contrastive-learning, forecasting, time-domain, interpretable]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_BasisFormer.pdf
pdf_source: arxiv
---

## TL;DR
BasisFormer는 자기지도학습(대조 학습)으로 학습 가능하고 해석 가능한 기저(basis) 집합을 먼저 학습한 뒤, 양방향 교차-어텐션으로 기저와 시계열 간 유사도를 계산하여 예측에 활용한다. 기저 기반 예측이 단변량·다변량 태스크 모두에서 SOTA 대비 11~16% 개선을 달성했다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델은 어텐션 패턴 해석이 어렵고, 예측 기반이 되는 표현이 명시적이지 않다.
- 시계열의 패턴을 수동으로 정의하거나(예: 푸리에 기저) 데이터에서 자동 학습하는 방식이 필요하다.
- 가설: 대조 학습으로 학습된 기저 집합이 시계열의 반복 패턴을 포착하고 해석 가능한 예측을 가능하게 한다.

## 2. 제안 방법론
- **핵심 아이디어**: 자기지도 대조학습으로 기저를 학습 후, 양방향 교차-어텐션(Coef)으로 유사도 계산, Forecast 모듈로 기저 가중 합산.
- **아키텍처**:
  1. **Basis Learning**: 역사(historical)와 미래(future) 시계열 뷰를 대조 학습으로 기저 집합 획득.
  2. **Coef 모듈**: 역사 뷰에서 시계열-기저 간 양방향 교차-어텐션으로 유사도 계수 계산.
  3. **Forecast 모듈**: 미래 뷰에서 유사도 계수로 기저 가중 합산하여 예측.
- **손실함수**: 대조 손실 + MSE 예측 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 단변량 11.04% ↓, 다변량 15.78% ↓ 평균 |
| 192     | see paper | see paper | see Table 1/2 |
| 336     | see paper | see paper | see Table 1/2 |
| 720     | see paper | see paper | see Table 1/2 |

출처: Table 1, Table 2 of paper.

## 4. 주요 기여
- 대조학습으로 학습 가능한 해석 가능 기저 집합 제안.
- 양방향 교차-어텐션 기반 Coef 모듈로 시계열-기저 유사도 계산.
- 단변량(11.04%)·다변량(15.78%) SOTA 대비 큰 폭의 개선.
- 기저의 시각화를 통한 예측 해석 가능성 제공.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 기저 개수 하이퍼파라미터 선택에 민감.
- ※ 메모: SimMTM(NeurIPS 2023)의 대조학습 기반 사전학습과 연결되는 아이디어. 도메인 특화 기저 학습으로 확장 가능.

## 6. 참고
- arXiv: https://arxiv.org/abs/2310.20496
- 관련: SimMTM (NeurIPS 2023), PatchTST (ICLR 2023)
