---
title: "Unlocking the Power of Patch: Patch-Based MLP for Long-Term Time Series Forecasting"
authors: Peiwang Tang et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2405.13575
code_url: ""
tags: [time-domain, mlp, patching, decomposition, channel-mixing, long-term]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Patching
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_PatchMLP.pdf
pdf_source: arxiv
---

## TL;DR
PatchMLP은 Transformer 없이 패치 메커니즘만으로 LTSF를 수행하는 경량 MLP 모델이다. 이동 평균으로 시계열을 분해하고 채널 믹싱으로 변수 간 상호작용을 처리하여 8개 벤치마크에서 Transformer 기반 모델을 능가한다.

## 1. 기존 모델의 한계 / 가설
- 최근 Transformer 기반 LTSF 모델의 성능 향상이 Transformer 구조 자체보다는 패치(patch) 메커니즘에서 기인함
- 복잡한 Transformer 아키텍처가 오히려 성능 저하 또는 불필요한 복잡성 야기
- 가설: "단순 선형 레이어 + 패치 메커니즘"이 복잡한 Transformer 기반 LTSF 모델을 능가할 수 있음

## 2. 제안 방법론
- **핵심 아이디어**: 패치 + MLP만으로 SOTA 달성, Transformer 불필요성 입증
- **아키텍처 구성요소**:
  1. **Multi-Scale Patch Embedding (MPE)**: 다중 스케일 패치 임베딩으로 로컬 시간 패턴 캡처
  2. **Feature Decomposition**: 평균 풀링(Average Pooling)으로 smooth/residual 성분 분리
  3. **Intra-Variable MLP**: 각 채널 내부 시간적 패턴 학습
  4. **Inter-Variable MLP**: 채널 간 상호작용 학습 (dot-product 메커니즘)
  5. **Projection**: 최종 예측값 출력
- **채널 전략**: 채널 독립성(CI) + 채널 믹싱 혼합

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (기본 unified setting)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: RevIN
- Train/Val/Test split: 표준 chronological split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: Solar Energy, Weather, Traffic, ECL, ETTh1, ETTh2, ETTm1, ETTm2 등 8개 데이터셋 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.438 | 0.429 | PatchTST 대비 개선 |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 패치 메커니즘이 Transformer 성공의 핵심임을 실증적으로 입증
- 단순 MLP + 패치 구조로 16개 벤치마크 전체 SOTA 달성
- 이동 평균 기반 분해로 노이즈 처리
- 채널 믹싱으로 변수 간 상관관계 모델링
- 계산 효율성과 성능의 균형

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: lookback 길이가 성능에 민감할 수 있음
- ※ 메모: 패치 기반 MLP이므로 primary는 Time-domain/MLP-Linear; 패칭은 CrossCutting 심볼릭, 분해는 TrendSeasonal 심볼릭

## 6. 참고
- [arXiv 2405.13575](https://arxiv.org/abs/2405.13575)
- 비교 논문: PatchTST, iTransformer, TimeMixer, DLinear
