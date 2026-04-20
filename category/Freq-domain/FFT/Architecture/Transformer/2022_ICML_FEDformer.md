---
title: FEDformer: Frequency Enhanced Decomposed Transformer for Long-term Series Forecasting
authors: Tian Zhou, Ziqing Ma, Qingsong Wen, Xue Wang, Liang Sun, Rong Jin
venue: ICML 2022
year: 2022
paper_url: https://arxiv.org/abs/2201.12740
code_url: https://github.com/DAMO-DI-ML/ICML2022-FEDformer
tags: [time-domain, freq-domain, transformer, fft, decomposition, trend-seasonal]
primary_category: category/Freq-domain/FFT/Architecture/Transformer
related_categories: 
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_ICML_FEDformer.pdf
pdf_source: arxiv
---

## TL;DR

FEDformer combines seasonal-trend decomposition with frequency-enhanced Transformer for long-term forecasting. By exploiting FFT-based frequency representations and decomposing time series into sparse periodic and trend components, FEDformer achieves linear complexity and significant improvements (14.8% and 22.6% error reduction for multivariate and univariate series) over SOTA.

## 1. 기존 모델의 한계 / 가설

- **Transformer 한계**: 표준 Transformer의 quadratic complexity는 긴 시계열에서 비효율적.
- **주파수 표현의 가능성**: 대부분의 시계열은 Fourier 변환과 같은 기저(basis)에서 sparse 표현을 가짐 → FFT를 활용한 효율화 가능.
- **분해(Decomposition)의 가치**: 계절성(seasonal)과 추세(trend)의 분리는 장기 예측에서 더 정확한 패턴 학습을 가능하게 함.

## 2. 제안 방법론

### 핵심 아이디어
계절-추세 분해와 FFT 기반 주파수 표현을 결합하여, 분해된 각 성분을 Transformer로 독립 모델링하면서 선형 복잡도를 유지.

### 아키텍처 구성

1. **Series Decomposition**
   - Seasonal component와 Trend component로 분해 (STL-like decomposition)
   - 각 component를 독립적으로 처리

2. **Fourier-Enhanced Transformer Encoder**
   - FFT를 통해 시계열을 주파수 도메인으로 변환
   - 가장 큰 magnitude를 가진 frequency modes를 선택 (sparse representation)
   - 선택된 modes만 Transformer encoder 입력으로 사용 → **linear complexity O(L) instead of O(L²)**

3. **Trend & Seasonal Decoder**
   - Trend component는 polynomial basis 사용
   - Seasonal component는 frequency space에서의 sparse representation 유지
   - 각각 독립 decoder로 처리

### 주요 수식

**FFT 기반 sparse selection:**
- 시계열 x를 FFT로 변환: $X = \text{FFT}(x)$
- Top-k magnitude modes 선택: $\{f_1, f_2, ..., f_k\}$
- 선택된 modes만 Transformer에 입력

**Decomposition:**
$$x = x_{\text{seasonal}} + x_{\text{trend}}$$

### 손실함수 / 학습 전략

- Loss function: MSE (Mean Squared Error)
- Optimizer: Adam
- 분해 components는 독립적으로 학습 가능하도록 모듈화

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)

- **Input length (lookback)**: 96
- **Forecast horizons**: 96, 192, 336, 720
- **Normalization**: Standardization (mean=0, std=1) per sample
- **Train/Val/Test split**: 7:1:2 ratio (standard LTSF protocol)
- **Batch size**: 32
- **Optimizer**: Adam
- **Learning rate**: 0.0001 (with learning rate scheduler)
- **Epochs**: 100
- **Loss function**: MSE
- **Number of Transformer layers**: 2
- **Number of attention heads**: 8
- **Hidden dimension**: 512
- **FFT modes selected**: 64 (default)
- **Dropout**: 0.05

### 3.2 Results

| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | 0.360 | 0.423 | Transformer baseline 대비 21% 개선 |
| 192     | 0.430 | 0.482 | Transformer baseline 대비 19% 개선 |
| 336     | 0.550 | 0.573 | Transformer baseline 대비 17% 개선 |
| 720     | 0.672 | 0.675 | Transformer baseline 대비 15% 개선 |

출처: Table 4-5 of FEDformer paper (ICML 2022). Results show multivariate forecasting performance.

**전체 벤치마크 비교**: FEDformer는 6개 데이터셋(ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather)에서 평균 14.8%(multivariate) ~ 22.6%(univariate) 오차 감소.

## 4. 주요 기여

- **주파수-분해 결합**: FFT 기반 sparse frequency representation과 seasonal-trend decomposition의 통합으로 높은 효율성과 정확도 달성
- **선형 복잡도**: Transformer의 O(L²) 복잡도를 O(L)로 감소 → 긴 시계열 처리 가능
- **광범위한 벤치마크 검증**: 6개 표준 LTSF 데이터셋에서 consistent SOTA 달성
- **해석 가능성**: 분해 components와 선택된 frequency modes로 모델의 동작 원리 명확화

## 5. 한계 및 후속 연구 아이디어

### 저자가 명시한 한계
- FFT 기반 sparse selection은 주기성이 명확한 데이터에 더 효과적 → 불규칙한 패턴의 데이터에는 개선 여지
- 분해 방법이 고정되어 있음 → 데이터 특성에 따라 적응적 분해 가능성 탐구

### 메모 및 확장 아이디어
- ※ 메모: Frequency-domain 접근은 이후 많은 후속 연구(FITS, FiLM, Fredformer 등)에 큰 영향을 미침
- ※ 메모: FEDformer는 분해(Decomposition)와 주파수(Freq-domain) 두 카테고리에 모두 해당하는 첫 주요 ICML 논문으로, 이후 하이브리드 접근법 확산의 계기 제공

## 6. 참고

- **GitHub**: https://github.com/DAMO-DI-ML/ICML2022-FEDformer (official implementation)
- **ArXiv**: https://arxiv.org/abs/2201.12740 (v3, June 2022)
- **ICML 2022 Presentation**: https://icml.cc/virtual/2022/poster/17985
- **Related**: Autoformer (decomposition 선행), Informer (attention-based forecasting), FiLM (FFT+MLP), FITS (FFT-based linear)
- **영향도**: ICML 2022 Spotlight & highly cited in subsequent LTSF research
