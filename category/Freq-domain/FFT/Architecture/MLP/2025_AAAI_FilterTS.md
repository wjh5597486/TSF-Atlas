---
title: "FilterTS: Comprehensive Frequency Filtering for Multivariate Time Series Forecasting"
authors: Yulong Wang et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2505.04158
code_url: https://github.com/wyl010607/FilterTS
tags: [freq-domain, fft, frequency-filtering, cross-variable, multivariate, long-term]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_FilterTS.pdf
pdf_source: arxiv
---

## TL;DR
FilterTS는 FFT 기반 동적 교차변수 필터링(Dynamic Cross-Variable Filtering)과 정적 글로벌 필터링(Static Global Filtering)을 결합한 주파수 도메인 다변량 시계열 예측 모델이다. 8개 벤치마크 중 6개에서 1위를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 주파수 도메인 모델은 변수 간 공유 주파수 성분을 충분히 활용하지 못함
- 정적 필터만 사용하면 입력에 따른 적응적 필터링이 불가능
- 노이즈 주파수 성분이 예측 정확도를 저하시킴
- 가설: 동적 교차변수 필터링으로 변수 간 공유 주파수 성분을 강화하면 다변량 예측이 개선

## 2. 제안 방법론
- **핵심 아이디어**: FFT 변환 후 동적/정적 이중 필터링으로 중요 주파수 강조
- **아키텍처 구성요소**:
  1. **Time to Frequency Embedding Module**: FFT로 시간 도메인 입력을 주파수 도메인으로 변환
  2. **Dynamic Cross-Variable Filtering Module**: 다른 변수들을 필터로 활용하여 변수 간 공유 주파수 성분 추출·강화
  3. **Static Global Filtering Module**: 학습 데이터에서 지배적인 안정 주파수 성분 식별
  4. **Frequency to Time Projection Module**: 역FFT로 시간 도메인으로 복원 후 예측
- **분위수 임계값(Quantile Threshold)**: 필터링 강도 결정하는 핵심 하이퍼파라미터

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Exchange Rate, Traffic, Weather 8개 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg (8 datasets) | 0.433 | 0.430 | 8개 중 6개 데이터셋 1위 |
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper (averaged across prediction lengths).

## 4. 주요 기여
- Dynamic Cross-Variable Filtering: 변수 간 공유 주파수 성분 동적 추출
- Static Global Filtering: 안정적인 지배 주파수 성분 캡처
- FFT 기반으로 계산 효율성 확보
- 8개 벤치마크 중 6개에서 전체 1위 달성

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: quantile threshold 하이퍼파라미터에 성능 의존
- ※ 메모: 동적 교차변수 필터링이 핵심 novelty로 ChannelDependent 심볼릭 링크 적절

## 6. 참고
- [arXiv 2505.04158](https://arxiv.org/abs/2505.04158)
- [GitHub](https://github.com/wyl010607/FilterTS)
- 비교: FilterNet (NeurIPS 2024), FEDformer, FreTS
