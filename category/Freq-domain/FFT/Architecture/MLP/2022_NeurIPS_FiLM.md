---
title: "FiLM: Frequency improved Legendre Memory Model for Long-term Time Series Forecasting"
authors: "Tian Zhou et al."
venue: NeurIPS
year: 2022
paper_url: "https://arxiv.org/abs/2205.08897"
code_url: "https://github.com/tianzhou2011/FiLM"
tags: [freq-domain, fft, mlp, legendre, fourier, forecasting]
primary_category: "Freq-domain/FFT/Architecture/MLP"
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: "./2022_NeurIPS_FiLM.pdf"
pdf_source: arxiv
---

## TL;DR
FiLM combines Legendre Polynomial projection with Fourier transform to improve frequency-domain time series forecasting. It effectively preserves historical information through orthogonal projections while removing noise, achieving 20.3% and 22.6% improvement over SOTA for multivariate and univariate forecasting respectively.

## 1. 기존 모델의 한계 / 가설
- 시계열 모델링에서 과거 정보를 정확하게 보존하면서도 노이즈를 제거하는 것이 어려움
- 주파수 영역 분석이 강력하지만, 단순 FFT는 phase 정보 손실 등의 문제 발생
- 가설: 직교 다항식(Legendre) 기저와 Fourier 기저를 결합하면 신호와 노이즈를 효과적으로 분리 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
시계열을 Legendre Polynomial 공간과 Fourier 공간으로 동시에 투영하여 구조화된 표현을 학습. 저계 다항식과 주파수 성분만 선택하여 noise 제거.

### 2.2 아키텍처 구성

**주요 컴포넌트**

1. **Legendre Memory Bank (LMB)**
   - 과거 T 스텝의 입력을 Legendre 다항식 기저로 투영
   - 선택된 M개의 주요 계수만 유지하여 과거 정보 요약
   - 식: 시계열을 P_0(t), P_1(t), ..., P_{M-1}(t) (Legendre 다항식)로 표현

2. **Fourier Projection (FP)**
   - 시계열을 주파수 영역으로 변환
   - FFT를 통해 진폭과 위상 추출
   - 상위 N개 주파수 성분만 선택하여 periodic 패턴 보존

3. **Low-Rank Approximation**
   - 계산 효율성을 위해 저계 근사(rank-r) 적용
   - 핵심 구조만 유지하면서 연산 복잡도 감소

4. **MLP Decoder**
   - Legendre + Fourier feature를 통합
   - 간단한 다층 신경망으로 미래 시점 예측

### 2.3 주요 수식 (간략)
- Legendre 기저: P_n(t) = (2n+1) 직교 다항식
- Fourier 기저: e^{i2πft}
- 최종 표현: Concat(LM_coefficients, Fourier_features) → MLP

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96 (주요 setting)
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standardization (mean, std)
- Train/Val/Test split: 7:1:2 또는 8:1:1
- Batch size: 32
- Optimizer: Adam, lr = 0.001
- Loss function: MSE
- Epochs: 100
- Early stopping: 20 epochs patience
- 기타: 모든 실험 5회 반복, 결과 평균 및 표준편차 보고

### 3.2 Results
상세 수치는 원문 Table 3, 4 참고.

**주요 성과:**
- 다변량 예측: FEDformer 대비 14.0% MSE 감소
- 단변량 예측: FEDformer 대비 16.8% MSE 감소
- 모든 horizon에서 SOTA 또는 준SOTA 달성

| Horizon | 대비 SOTA    | 특이사항           |
|---------|-------------|------------------|
| 96      | 우수        | Frequency 주기 강함 |
| 192     | 우수        |                  |
| 336     | SOTA/준SOTA | 수렴성 안정       |
| 720     | SOTA/준SOTA |                  |

*(자세한 MSE/MAE 수치는 원문 Table 3, 4 참고)*

## 4. 주요 기여
- Legendre 다항식과 Fourier 변환의 효과적 결합
- 과거 정보 보존과 노이즈 제거의 이론적·실증적 증명
- 저계 근사를 통한 계산 효율성 달성
- 주요 벤치마크(ETTh1/2, ETTm1/2 등) 모두에서 강력한 성능

## 5. 한계 및 후속 연구 아이디어
- Legendre 다항식 차수(M)와 Fourier 주파수 개수(N) 선택에 대한 하이퍼파라미터 튜닝 필요
- 극도로 비정상(highly non-stationary) 시계열에 대한 강건성 검증 필요
- ※ 메모: Appendix F의 하이퍼파라미터 튜닝 결과가 중요하며, M과 N의 최적값은 데이터셋마다 다를 수 있음

## 6. 참고
- [GitHub Repository](https://github.com/DAMO-DI-ML/NeurIPS2022-FiLM)
- arXiv preprint: 2205.08897
- 관련 논문: FEDformer, TimesNet, Autoformer
- 직교 다항식 기저 분석: Legendre, Hermite, Chebyshev 등의 특성 비교 (Appendix 참고)
