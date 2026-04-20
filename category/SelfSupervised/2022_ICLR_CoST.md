---
title: "CoST: Contrastive Learning of Disentangled Seasonal-Trend Representations for Time Series Forecasting"
authors: Gerald Woo, Chenghao Liu, Doyen Sahoo, Akshat Kumar, Steven Hoi
venue: ICLR 2022
year: 2022
paper_url: https://arxiv.org/abs/2202.01575
code_url: https://github.com/salesforce/CoST
tags: [self-supervised, contrastive-learning, representation-learning, seasonal-trend-decomposition, time-domain, freq-domain, forecasting]
primary_category: category/SelfSupervised
related_categories: [category/Decomposition/TrendSeasonal, category/CrossCutting/Loss]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_ICLR_CoST.pdf
pdf_source: arxiv
---

## TL;DR
CoST는 시계열을 계절과 추세의 분리된 표현으로 학습하기 위해 대조학습(contrastive learning)을 적용한 표현학습 프레임워크다. 시간영역과 주파수영역 대조손실을 통해 각각 구별가능한 추세와 계절 표현을 학습하며, 다변량 벤치마크에서 MSE 21.3% 개선을 달성했다.

## 1. 기존 모델의 한계 / 가설
- 종단간(end-to-end) 신경망 학습은 시계열의 표현을 얽히게(entangled) 학습하여, 한 지역 성분의 분포 이동(distribution shift) 발생 시 일반화가 어렵다.
- 시계열은 여러 독립적인 모듈(계절, 추세, 오차)로 구성되는데, 분포 이동이 국소적으로 발생할 수 있으므로 분리된 표현이 필요하다.
- 가설: 구조적 시계열 모델의 선험 지식(추세, 계절, 오차 분해)을 활용하고 대조학습으로 분리된 표현을 학습하면 강건한 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 추세(trend)와 계절(seasonal) 성분의 분리된 표현으로 학습한 후, 단순 회귀 헤드로 파인튜닝.
  
- **아키텍처**:
  1. **추세 표현 학습(Time Domain)**
     - Mixture of Auto-Regressive Experts (MAE): Lookback 창 선택의 어려움을 완화
     - 시간영역 대조손실로 학습
  
  2. **계절 표현 학습(Frequency Domain)**
     - Learnable Fourier Layer: 주파수 간 상호작용(intra-frequency interaction) 가능
     - 주기 길이 결정 문제를 우회하는 새로운 주파수영역 대조손실
  
  3. **파인튜닝**: 별도의 단순 회귀 헤드로 예측
  
- **손실함수**:
  - 시간영역 대조손실: 추세 표현의 일관성 학습
  - 주파수영역 대조손실: 계절 표현의 구별가능성 학습
  - 인코더와 레그레서를 분리하여 기울기 흐름 명확화

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 32, Adam, 기본 학습률
- Loss function: 시간/주파수 대조손실 (사전학습), MSE (파인튜닝)
- 기타: Mixture of Experts는 8개 전문가 사용

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | multivariate 다변량 벤치마크에서 21.3% MSE 개선 |
| 192     | see paper | see paper | see Table 4 |
| 336     | see paper | see paper | see Table 4 |
| 720     | see paper | see paper | see Table 4 |

출처: Table 4 of paper (ETTh1, ETTh2, ETTm1, ETTm2 포함).

## 4. 주요 기여
- 인과관계(causal) 관점에서 분리된 계절-추세 표현의 필요성을 이론적으로 제시.
- 시간영역 + 주파수영역 대조손실을 통해 두 성분을 효과적으로 학습하는 방법 제안.
- Mixture of Experts와 Learnable Fourier Layer로 추세와 계절의 특성 반영.
- 다양한 백본 인코더와 다운스트림 리그레서에 대해 강건성 입증.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 계절 주기가 명확하지 않은 데이터나 비정상(non-stationary) 계절 패턴에 대한 강건성 추가 평가 필요.
- ※ 메모: 대조학습을 활용한 분해 기반 TSF의 핵심 논문. Decomposition 카테고리와 SelfSupervised의 교집합 모델로 평가할 수 있음. FEDformer, Autoformer 등과 비교 시 대조학습의 효과를 명확히 보여줌.

## 6. 참고
- arXiv: https://arxiv.org/abs/2202.01575
- GitHub: https://github.com/salesforce/CoST
- ICLR 2022 포스터 발표
- 관련: Autoformer (ICLR 2021), FEDformer (ICML 2022), Contrastive Learning for TSF
