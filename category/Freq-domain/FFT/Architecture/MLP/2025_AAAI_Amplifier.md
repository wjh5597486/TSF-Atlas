---
title: "Amplifier: Bringing Attention to Neglected Low-Energy Components in Time Series Forecasting"
authors: Jingru Fei et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2501.17216
code_url: https://github.com/aikunyi/Amplifier
tags: [freq-domain, fft, energy-amplification, decomposition, channel-interaction, long-term]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_Amplifier.pdf
pdf_source: arxiv
---

## TL;DR
Amplifier는 기존 예측 모델이 쉽게 놓치는 저에너지(low-energy) 주파수 성분을 에너지 증폭 기법으로 강조하는 플러그인 모듈이다. 스펙트럼 반전(spectrum flipping)으로 저에너지 성분을 증폭 후 계절-추세 예측기로 처리하고 복원한다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 MSE 손실 최소화 과정에서 고에너지 성분에 집중하고 저에너지 성분을 무시
- 저에너지 주파수 성분이 예측에 중요한 정보를 담고 있을 수 있음
- 가설: 저에너지 성분을 의도적으로 증폭시켜 처리하면 전반적인 예측 정확도 향상

## 2. 제안 방법론
- **핵심 아이디어**: 스펙트럼 반전으로 저에너지 성분 증폭 → 예측 → 복원
- **아키텍처 구성요소**:
  1. **Energy Amplification Block (EAB)**: FFT 후 스펙트럼 반전으로 저에너지 성분을 고에너지로 변환
  2. **Semi-Channel Interaction (SCI) Block**: 채널별 공통 및 고유 시간적 패턴 동시 캡처
  3. **Seasonal-Trend Forecaster**: 증폭된 성분을 계절·추세로 분해하여 예측
  4. **Energy Restoration Block (ERB)**: 예측 후 에너지 복원 (반전 연산 역적용)
- **적용 방식**: 임의 백본 모델에 플러그인 형태로 통합 가능

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam, NVIDIA RTX 3090
- Loss function: MSE
- 기타: ETTm1, ETTm2, ETTh1, ETTh2, ECL, Exchange, Traffic, Weather 8개 데이터셋

### 3.2 Results
| Horizon | MSE (avg) | MAE (avg) | 비교 SOTA 대비 |
|---------|-----------|-----------|----------------|
| avg     | 0.430 | 0.428 | ETTm1 Transformer+Amplifier: +9.837% MSE 개선 |
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 저에너지 주파수 성분 증폭 기법(Energy Amplification)으로 무시된 정보 활용
- 스펙트럼 반전(spectrum flipping)으로 저에너지 성분 가시화
- Semi-channel interaction으로 채널별 고유/공통 패턴 동시 처리
- 플러그인 모듈로 기존 모델에 쉽게 통합
- 8개 벤치마크에서 지속적 성능 향상

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 스펙트럼 반전이 일부 데이터셋에서 불안정할 수 있음
- ※ 메모: primary는 Freq-domain/FFT/Architecture/MLP (주파수 에너지 조작이 핵심); TrendSeasonal 심볼릭 링크

## 6. 참고
- [arXiv 2501.17216](https://arxiv.org/abs/2501.17216)
- [GitHub](https://github.com/aikunyi/Amplifier)
- 비교: FilterNet (NeurIPS 2024), FreTS (NeurIPS 2023)
