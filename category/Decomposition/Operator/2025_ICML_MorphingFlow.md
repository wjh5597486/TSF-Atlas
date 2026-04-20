---
title: "Slimming the Fat-Tail: Morphing-Flow for Adaptive Time Series Modeling"
authors: Tianyu Liu et al.
venue: ICML
year: 2025
paper_url: ""
code_url: ""
tags: [freq-domain, normalizing-flow, normalization, non-stationarity, fat-tail, test-time, adaptive]
primary_category: category/Decomposition/Operator
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_MorphingFlow.pdf
pdf_source: none
---

## TL;DR
Morphing-Flow (MoF) addresses fat-tailed, non-stationary distributions in time series forecasting by combining a spline-based normalizing flow (Flow component) and a test-time-trained method (Morph component) that adaptively normalizes distributions while preserving critical extreme features.

## 1. 기존 모델의 한계 / 가설
- 시계열 데이터는 흔히 첨도가 높은(leptokurtic) 분포와 지속적인 분포 이동(distribution shift)을 보여 모델 수렴을 방해함.
- 단순 z-score 정규화(RevIN 등)는 극값(extreme values)을 소실시키거나 fat-tail 특성을 제대로 처리하지 못함.
- Spline 기반 정규화 흐름(flow)과 test-time 적응을 결합하면 극값 보존과 분포 정규화를 동시에 달성 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 두 가지 컴포넌트를 결합하여 fat-tail 분포를 적응적으로 정규화.
- **아키텍처**:
  - **Flow (Spline-based Transform)**: 단조 증가 변환(monotone spline)으로 입력 분포를 정규 분포에 가깝게 변환.
  - **Morph (Test-time Training)**: 배포(deployment) 중 현재 데이터 분포에 맞게 플로우 파라미터 적응.
  - 단순 linear backbone과 결합 시 SOTA 달성 가능.
- **손실함수**: 예측 MSE + 플로우 likelihood.
- **학습 전략**: 사전 훈련 후 test-time 적응.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: Morphing-Flow (자체 정규화)
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE + flow likelihood
- 기타: 8개 데이터셋 (ETTh2, Electricity 명시, Exchange 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA on fat-tail datasets |
| 192     | see paper | see paper | SOTA on fat-tail datasets |
| 336     | see paper | see paper | SOTA on fat-tail datasets |
| 720     | see paper | see paper | SOTA on fat-tail datasets |

출처: Table of paper (8 datasets, Exchange -21.7% error reduction).

## 4. 주요 기여
- Fat-tail 분포를 위한 spline 기반 적응 정규화 (Flow 컴포넌트).
- Test-time training으로 분포 이동에 적응 (Morph 컴포넌트).
- Exchange 데이터셋에서 21.7% 예측 오차 감소.
- Patch-based Mamba 백본과 결합 시 best competitor 대비 6.3% 향상.

## 5. 한계 및 후속 연구 아이디어
- Test-time training으로 인한 추론 시간 증가.
- Spline 파라미터 수가 많을 경우 과적합 가능성.
- ※ 메모: RevIN(ICLR 2022)의 정규화 접근을 더 복잡한 분포(fat-tail)로 확장.

## 6. 참고
- 관련 논문: RevIN (ICLR 2022), SAN (NeurIPS 2023), FAN (NeurIPS 2024)
- OpenReview: https://openreview.net/forum?id=gwlJgoq5QZ
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44444
- ※ 원본 PDF 미취득: arxiv preprint 없음
