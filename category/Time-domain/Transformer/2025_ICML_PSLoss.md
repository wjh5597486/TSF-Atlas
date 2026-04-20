---
title: "Patch-wise Structural Loss for Time Series Forecasting"
authors: Dilfira Kudrat et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2503.00877
code_url: https://github.com/Dilfiraa/PS_Loss
tags: [time-domain, loss-function, patch, structural-alignment, transformer, mlp]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/Loss, category/CrossCutting/Patching]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_PSLoss.pdf
pdf_source: arxiv
---

## TL;DR
PSLoss proposes a patch-wise structural loss function that compares time series predictions at the patch level using statistical properties (correlation, variance, mean) rather than individual time steps. It captures structural dependencies missed by point-wise MSE, and integrates with existing loss functions to improve diverse forecasting architectures.

## 1. 기존 모델의 한계 / 가설
- MSE 등 점별(point-wise) 손실 함수는 각 시간 스텝을 독립적으로 비교하여, 시계열에 내재된 구조적 의존성(시간적 패턴, 상관관계)을 무시함.
- 패치 수준에서 예측값과 실제값을 비교하면 더 풍부한 시계열 구조를 반영할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 예측된 시계열과 실제 시계열을 패치(짧은 세그먼트) 단위로 비교하여 구조적 유사성 측정.
- **Patch-wise Structural Loss**:
  - 각 패치에 대해 세 가지 통계량 비교: **상관관계(correlation)**, **분산(variance)**, **평균(mean)**.
  - 기존 MSE/MAE와 결합하여 플러그-앤-플레이 방식으로 적용 가능.
- **아키텍처**: 아키텍처-독립적 (iTransformer, PatchTST, DLinear, TimeMixer, TimesNet 등과 함께 사용).
- **손실함수**: PS Loss = weighted combination of patch structural term + point-wise term.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (모델별 기본값)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 기반 모델의 기본 설정
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 기반 모델 설정
- Loss function: PS Loss (기존 loss + patch structural term)
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, ECL, Exchange 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | improvement over MSE baseline |
| 192     | see paper | see paper | improvement over MSE baseline |
| 336     | see paper | see paper | improvement over MSE baseline |
| 720     | see paper | see paper | improvement over MSE baseline |

출처: Table of paper (ETTh, ETTm, Weather, ECL, Exchange).

## 4. 주요 기여
- 패치 수준 구조적 손실 함수로 시계열 구조적 의존성 포착.
- 플러그-앤-플레이: 다양한 아키텍처(Transformer, MLP, CNN)와 결합 가능.
- 상관관계, 분산, 평균의 세 가지 통계량을 통한 구조 정렬.
- 다양한 실제 데이터셋에서 기반 모델 성능 일관 향상.

## 5. 한계 및 후속 연구 아이디어
- 패치 크기 선택이 성능에 영향을 미침.
- ※ 메모: SAMformer(ICML 2024)의 loss 수준 기여와 유사한 위상의 논문.

## 6. 참고
- 관련 논문: SAMformer (ICML 2024), FreDF (ICLR 2025), RobustTSF (ICLR 2024)
- GitHub: https://github.com/Dilfiraa/PS_Loss
- arXiv: https://arxiv.org/abs/2503.00877
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44030
