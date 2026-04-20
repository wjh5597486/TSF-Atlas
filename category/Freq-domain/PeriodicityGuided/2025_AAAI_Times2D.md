---
title: "Times2D: Multi-Period Decomposition and Derivative Mapping for General Time Series Forecasting"
authors: Reza Nematirad et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2504.00118
code_url: https://github.com/Tims2D/Times2D
tags: [freq-domain, periodicity, decomposition, 2d-representation, cnn, derivative, multi-period]
primary_category: category/Freq-domain/PeriodicityGuided
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_Times2D.pdf
pdf_source: arxiv
---

## TL;DR
Times2D는 1D 시계열을 2D 표현으로 변환하여 CNN 기반 처리를 적용하는 예측 모델이다. 주기 분해 블록으로 다중 주기 패턴을 추출하고 1차/2차 미분 히트맵으로 급격한 변화와 전환점을 포착한다.

## 1. 기존 모델의 한계 / 가설
- 1D 처리는 다중 주기성, 급격한 변동, 전환점 포착에 한계
- TimesNet 등 2D 변환 모델은 단일 주기만 사용
- 미분 정보(변화율)가 예측에 중요하나 기존 모델에서 미활용
- 가설: 다중 주기 분해 + 미분 히트맵의 2D 결합이 풍부한 시계열 표현 제공

## 2. 제안 방법론
- **핵심 아이디어**: 1D → 2D 변환 (다중 주기 + 미분 히트맵) + CNN 처리
- **아키텍처 구성요소**:
  1. **Periodic Decomposition Block (PDB)**: 주파수 도메인에서 다중 주기를 추출하고 각 주기에 따라 1D→2D 변환
  2. **First and Second Derivative Heatmaps (FSDH)**: 1차 미분(변화율) 및 2차 미분(가속도) 히트맵으로 급격한 변화와 전환점 포착
  3. **Aggregation Forecasting Block (AFB)**: PDB와 FSDH 출력을 2D CNN으로 통합 후 최종 예측

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정 (96~336)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준
- Train/Val/Test split: 표준 chronological
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Exchange, Solar, ILI, Weather, Traffic, M4

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 다중 주기 분해 기반 2D 표현으로 다양한 시계열 패턴 포착
- 미분 히트맵으로 급격한 변화와 전환점 명시적 모델링
- CNN을 활용한 2D 공간에서의 효과적 패턴 학습
- 장기 및 단기 예측 모두 지원 (M4 포함)

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 2D 변환 과정의 계산 비용 증가
- ※ 메모: 주기 감지에 FFT 사용하므로 PeriodicityGuided primary; 분해 성분으로 TrendSeasonal 심볼릭

## 6. 참고
- [arXiv 2504.00118](https://arxiv.org/abs/2504.00118)
- [GitHub](https://github.com/Tims2D/Times2D)
- 비교: TimesNet (ICLR 2023), PatchTST, DLinear
