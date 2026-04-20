---
title: "TimesNet: Temporal 2D-Variation Modeling for General Time Series Analysis"
authors: Haixu Wu et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2210.02186
code_url: https://github.com/thuml/TimesNet
tags: [time-domain, cnn, freq-domain, periodicity, 2d-variation, general-analysis]
primary_category: category/Freq-domain/PeriodicityGuided
related_categories:
  - category/Time-domain/CNN-TCN
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_TimesNet.pdf
pdf_source: arxiv
---

## TL;DR
TimesNet은 FFT로 시계열에서 주기를 추출한 뒤, 1D 시계열을 2D 이미지 형태(period × truncated_length)로 변환하여 TimesBlock(2D CNN)으로 intraperiod 및 interperiod 변동을 동시에 포착하는 범용 시계열 분석 모델이다. 예측뿐 아니라 보간, 분류, 이상 탐지, 단기 예측 등 5개 태스크에서 SOTA 달성.

## 1. 기존 모델의 한계 / 가설
- 1D 시계열 모델링은 intraperiod 변동(한 주기 내 패턴)과 interperiod 변동(주기 간 추세) 둘 다 포착하기 어려움.
- Transformer 기반 모델은 예측에 특화되어 있어 다른 시계열 분석 태스크(보간, 분류 등)에 범용으로 쓰기 어려움.
- 가설: 시계열의 복잡한 시간 변동은 여러 주기의 intra/interperiod 변동으로 분해 가능하며, 이를 2D 공간에서 modeling하면 CNN의 공간적 귀납 편향이 효과적으로 작동함.

## 2. 제안 방법론
- **핵심 아이디어**: FFT로 상위 k개 주기(p₁,...,pₖ)를 추출 → 1D 시계열을 각 주기로 reshape → 2D map(period×truncated_length) → Inception 기반 2D CNN으로 처리 → 1D로 복원 → 가중 합산.
- **TimesBlock 구조**:
  1. FFT로 상위 k개 주기 주파수 선택 (k=5, amplitude 기준)
  2. 각 주기 pᵢ에 대해 1D → 2D reshape (zero-padding 후 [T/pᵢ, pᵢ] 형태)
  3. Inception 기반 2D Conv(다양한 커널 크기) 적용
  4. 2D → 1D unreshape, k개 결과를 amplitude-weighted 합산
- **범용성**: Embedding + TimesBlock stack + 태스크별 prediction head만 교체하여 5개 태스크 지원.
- **손실함수**: MSE (예측/보간), BCE (분류), 기타 태스크별.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (단기 예측 96 steps ahead에서는 96; 장기 예측 336~720에서는 336 또는 720 사용 — 논문 Appendix 참조)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 Z-score 정규화 (mean=0, std=1)
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: batch size 32, Adam, lr 0.0001
- Loss function: MSE
- 기타: k=5 (top-k periods), d_model=64, d_ff=64, e_layers=2

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | 0.384 | 0.402 | PatchTST 0.370 대비 소폭 높음 |
| 192     | 0.436 | 0.429 | — |
| 336     | 0.491 | 0.469 | — |
| 720     | 0.521 | 0.500 | — |

출처: Table 1 of paper (ICLR 2023 / arXiv:2210.02186). ※ 메모: 일부 수치는 버전에 따라 미세 차이 있을 수 있음.

## 4. 주요 기여
- FFT 기반 주기 추출 + 1D→2D 변환으로 intra/interperiod 변동 동시 모델링 아이디어 제안.
- 단일 모델(TimesNet)으로 5개 시계열 분석 태스크(장기 예측, 단기 예측, 보간, 이상 탐지, 분류)에서 SOTA 또는 경쟁력 있는 성능.
- Inception 스타일 2D CNN을 시계열에 성공적으로 적용.
- 이후 CycleNet, FilterNet 등 주기 기반 접근의 기반이 됨.

## 5. 한계 및 후속 연구 아이디어
- FFT 주기 추출이 비정상(non-stationary) 시계열에 덜 효과적일 수 있음.
- 2D reshape 과정에서 zero-padding이 필요해 경계 아티팩트 가능성.
- ※ 메모: 주기 추출이 미리 고정된 k=5개로 제한됨; adaptive k 선택이나 attention 기반 주기 가중화 검토 가능.

## 6. 참고
- arXiv: https://arxiv.org/abs/2210.02186
- OpenReview: https://openreview.net/forum?id=ju_Uqw384Oq
- 공식 구현: https://github.com/thuml/TimesNet
- Time-Series-Library (TSLib): https://github.com/thuml/Time-Series-Library
