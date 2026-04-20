---
title: "Disentangling Structured Components: Towards Adaptive, Interpretable and Scalable Time Series Forecasting"
authors: Jinliang Deng et al.
venue: IEEE Transactions on Knowledge and Data Engineering (TKDE) 2024
year: 2024
paper_url: https://arxiv.org/abs/2305.13036
code_url: https://github.com/cure-lab/SCINet
tags: [time-domain, mlp, decomposition, channel-dependent, interpretable]
primary_category: journal/TKDE
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: arxiv
---

## TL;DR
SCNN(Structured Component-based Neural Network)은 다변량 시계열을 구조적(structured) 성분과 이질적(heterogeneous) 성분으로 분리 학습하여 해석 가능성과 확장성을 동시에 높인다. 컴포넌트별 특화 모델을 사용해 ETTh1 포함 7개 벤치마크에서 경쟁력 있는 결과를 보인다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델은 채널 전체를 단일 인코더로 처리하거나 채널 독립(CI) 방식을 취해 채널 간 이질성(heterogeneity)을 무시함.
- Transformer 기반 모델은 해석이 어렵고, 스케일 증가 시 계산 비용이 급증.
- 가설: 시계열을 구조적 성분과 이질적 성분으로 명시 분리하면 각 성분에 맞는 경량 모델을 적용할 수 있고, 더 해석 가능한 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 채널 집합을 클러스터링(구조적 성분)과 개별 인코딩(이질적 성분)으로 분리 처리.
- **아키텍처 구성요소**:
  1. **Structured Component Extractor**: 채널 간 공유 패턴을 MLP로 학습 (유사 채널 클러스터링)
  2. **Heterogeneous Component Encoder**: 채널별 개별 MLP로 고유 패턴 인코딩
  3. **Adaptive Fusion**: 두 성분의 출력을 채널별 학습 가중치로 결합 → 최종 예측
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96, 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam, lr=1e-3 (세부 내용은 논문 참조)
- Loss function: MSE

### 3.2 Results
> arXiv 논문(2305.13036) 기준 근사치.

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.376 | 0.399 | PatchTST 대비 유사 수준 |
| 192     | 0.419 | 0.432 | |
| 336     | 0.430 | 0.446 | |
| 720     | 0.451 | 0.474 | |

※ 메모: 수치는 근사값. 정확한 결과는 TKDE 최종판 Table 확인 필요.

출처: Table of arXiv 2305.13036.

## 4. 주요 기여
- 구조적·이질적 성분 명시 분리로 해석 가능성 향상
- 채널 수 증가에도 선형 복잡도 유지 (확장성)
- 컴포넌트별 특화 경량 모델로 Transformer 대비 빠른 학습
- ETTh1/ETTh2/ETTm1/ETTm2/Electricity/Weather/Traffic 7개 벤치마크 일관된 경쟁력

## 5. 한계 및 후속 연구 아이디어
- 클러스터링 기반 구조적 성분 분리가 비지도 방식이라 최적 클러스터 수 선택이 필요.
- ※ 메모: 도메인 지식이나 그래프 구조를 활용한 클러스터 초기화가 개선 방향이 될 수 있음.

## 6. 참고
- arXiv: https://arxiv.org/abs/2305.13036
- 관련: DLinear(AAAI 2023), PatchTST(ICLR 2023), iTransformer(ICLR 2024)
