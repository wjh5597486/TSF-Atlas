---
title: "Conditional Diffusion Model with Nonlinear Data Transformation for Time Series Forecasting"
authors: Rishi Jinka et al.
venue: ICML
year: 2025
paper_url: ""
code_url: https://github.com/quest-lab-iisc/CNDiff
tags: [time-domain, diffusion, generative, nonlinear-transformation, probabilistic-forecasting]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_CNDiff.pdf
pdf_source: none
---

## TL;DR
CNDiff proposes a conditional diffusion model with novel nonlinear data transformations and learnable conditions in the forward process for time series forecasting. It treats the reverse process as non-Markovian to match the non-Markovian forward process, achieving top performance on ETTh1 and multiple other real-world datasets.

## 1. 기존 모델의 한계 / 가설
- 기존 확산 모델(CSDI, TimeGrad 등)은 선형(가우시안) 확산 프로세스를 사용하여 비선형 시계열 패턴을 포착하기 어려움.
- 표준 Markovian 역방향 프로세스는 비선형 순방향 프로세스와 불일치.
- 비선형 변환과 학습 가능한 조건을 순방향 과정에 도입하면 더 복잡한 시계열 분포를 학습할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 비선형 데이터 변환과 학습 가능한 조건을 순방향 확산 프로세스에 도입.
- **아키텍처**:
  - **Nonlinear Forward Process**: 비선형 시간 의존 변환 + 학습 가능한 조건.
  - **Non-Markovian Reverse Process**: 비Markovian 순방향 프로세스에 맞춘 역방향 프로세스.
  - 새로운 손실 공식화 및 순/역방향 프로세스의 상세한 유도식 제공.
- **손실함수**: 새로운 ELBO (비선형 변환 포함).
- **학습 전략**: Conditional diffusion.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: 새로운 ELBO (비선형)
- 기타: 9개 실제 데이터셋 (ETTh1 포함, Norpool 등)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | Rank 1 on Norpool and ETTh1 |
| 192     | see paper | see paper | Top ranking |
| 336     | see paper | see paper | Top ranking |
| 720     | see paper | see paper | Top ranking |

출처: PMLR v267, pp. 51703-51723 (ETTh1 Rank 1).

## 4. 주요 기여
- 비선형 데이터 변환과 학습 가능한 조건을 확산 모델에 통합.
- 비Markovian 역방향 프로세스로 비선형 순방향 프로세스와 정합성 확보.
- 9개 데이터셋 중 ETTh1, Norpool에서 1위 달성.
- 순/역방향 프로세스의 상세한 이론적 유도 제공.

## 5. 한계 및 후속 연구 아이디어
- 비선형 변환 설계가 dataset-specific할 수 있음.
- ※ 메모: TimeDiff(ICML 2023)의 조건부 확산 접근을 비선형 변환으로 확장.

## 6. 참고
- 관련 논문: TimeDiff (ICML 2023), CSDI (NeurIPS 2021), NsDiff (ICML 2025)
- GitHub: https://github.com/quest-lab-iisc/CNDiff
- OpenReview: https://openreview.net/forum?id=kcUNMKqrCg
- PMLR: https://proceedings.mlr.press/v267/rishi25a.html
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44243
- ※ 원본 PDF 미취득: arxiv preprint 없음, PMLR 직접 접근 필요
