---
title: "SimMTM: A Simple Pre-Training Framework for Masked Time-Series Modeling"
authors: Jiaxiang Dong et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2302.00861
code_url: https://github.com/thuml/SimMTM
tags: [self-supervised, pretraining, masked-modeling, contrastive, time-domain, forecasting, classification]
primary_category: category/SelfSupervised
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_SimMTM.pdf
pdf_source: arxiv
---

## TL;DR
SimMTM은 마스킹된 시계열을 재구성할 때, 다수의 마스킹 이웃 시계열들의 가중 합산으로 복원하는 단순하면서도 효과적인 사전학습 프레임워크다. 시간 변동성 정보를 여러 마스킹 뷰에서 보완적으로 조합함으로써 너무 어려운 재구성 태스크를 완화하고, NeurIPS 2023 Spotlight을 받았다.

## 1. 기존 모델의 한계 / 가설
- 기존 MAE(Masked Autoencoder) 방식을 시계열에 직접 적용하면 시간적 변동성 정보가 마스킹으로 손실되어 재구성이 과도하게 어려워진다.
- 시계열의 의미 정보는 주로 시간적 변동 패턴에 있으므로, 랜덤 마스킹 후 단순 재구성은 표현 학습에 비효율적이다.
- 가설: 여러 마스킹된 뷰의 정보를 가중 합산하면 완전한 시계열에 더 가깝게 복원할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 마스킹된 시계열을 동일 시계열의 다른 마스킹 이웃들의 가중 합산으로 복원. 각 이웃의 가중치는 임베딩 유사도로 결정.
- **아키텍처**:
  1. **다중 마스킹**: 동일 시계열에서 여러 랜덤 마스킹 뷰 생성.
  2. **유사도 기반 가중합**: 마스킹 뷰의 임베딩 유사도로 이웃 가중치 계산.
  3. **재구성**: 가중 합산으로 원본 시계열 복원.
  4. **파인튜닝**: 예측·분류 태스크별 헤드 적용.
- **손실함수**: 재구성 MSE + 대조 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (파인튜닝 시)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할 + 사전학습 데이터 포함
- Batch size / Optimizer / LR: Adam
- Loss function: MSE (파인튜닝); MSE+대조 손실 (사전학습)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 시계열 사전학습 SOTA 대비 향상 |
| 192     | see paper | see paper | see Table 1 |
| 336     | see paper | see paper | see Table 1 |
| 720     | see paper | see paper | see Table 1 |

출처: Table 1, Table 3 of paper (in-domain / cross-domain 파인튜닝 결과).

## 4. 주요 기여
- 마스킹 이웃의 가중 합산 복원으로 지나치게 어려운 재구성 태스크 완화.
- 예측·분류 두 태스크에서 동시에 SOTA 사전학습 방법 대비 향상.
- 도메인 내(in-domain)·도메인 간(cross-domain) 파인튜닝 모두 효과적.
- NeurIPS 2023 Spotlight 선정.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 사전학습 데이터 규모가 클수록 효과적이나, 소규모 데이터에서의 효용은 제한적.
- ※ 메모: TimeSiam(ICML 2024), TS2Vec와 함께 시계열 자기지도학습의 핵심 베이스라인. 멀티모달 시계열 사전학습으로 확장 가능.

## 6. 참고
- arXiv: https://arxiv.org/abs/2302.00861
- GitHub: https://github.com/thuml/SimMTM
- NeurIPS 2023 Spotlight
- 관련: TimeSiam (ICML 2024), TS2Vec, BasisFormer (NeurIPS 2023)
