---
title: "FRNet: Frequency-based Rotation Network for Long-term Time Series Forecasting"
authors: Xinyu Zhang et al.
venue: KDD 2024
year: 2024
paper_url: https://dl.acm.org/doi/10.1145/3637528.3671713
code_url: https://github.com/SiriZhang45/FRNet
tags: [freq-domain, fft, linear, decomposition, rotation, period, trend, time-series-forecasting]
primary_category: category/Freq-domain/FFT/Architecture/Linear
related_categories: [category/Decomposition/TrendSeasonal]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: 
pdf_source: none
---

## TL;DR
FRNet은 시계열을 주기(period)와 추세(trend) 성분으로 분해한 뒤, 각각을 복소수 선형 네트워크 기반의 주파수 도메인 회전(frequency rotation) 모듈로 예측하는 장기 시계열 예측 모델이다. 7개 실제 데이터셋에서 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 LTSF 방법들은 주기 성분의 동적 변화(dynamic period)를 효과적으로 포착하지 못함.
- 시간 도메인보다 주파수 도메인에서 주기 변화를 더 쉽게 정량화할 수 있다는 경험적 관찰.
- 복소수 선형 네트워크를 이용한 주파수 회전으로 주기 특징을 효과적으로 학습할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 분해(주기 + 추세) 후, 각 성분을 복소수 선형 네트워크 기반 주파수 회전 모듈로 예측.
- **아키텍처 구성요소**:
  - **Period-Trend Decomposition**: 이동 평균(moving average)으로 추세 분리, 나머지를 주기 성분으로 처리
  - **Period Frequency Rotation Module**: 주기 성분을 FFT 후 복소수 공간에서 회전 변환하여 동적 주기 특징 포착
  - **Patch Frequency Rotation Module**: 추세 성분을 패치 단위로 FFT 후 회전 변환하여 예측
  - 역FFT로 시간 도메인 복원 후 합산
- **손실함수**: MSE
- **핵심 수식**: 복소수 선형 매핑 $\hat{x}_f = W_c \cdot x_f$ (회전 행렬 $W_c \in \mathbb{C}^{d \times d}$)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 또는 512 (상세는 원문 참고)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic 7개 데이터셋에서 기존 SOTA(TimesNet, PatchTST 등) 대비 일관적 우수성 보고. H=96 기준 MSE: 0.143, MAE: 0.195 수준 (출처: 관련 조사 논문 인용).

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- 주파수 도메인에서 동적 주기 특징을 정량화하고 학습하는 새로운 프레임워크
- 복소수 선형 네트워크 기반 주파수 회전 모듈 설계
- Period-Trend 분해와 주파수 회전의 결합으로 7개 벤치마크 SOTA 달성
- 간결한 선형 구조로 높은 효율성 유지

## 5. 한계 및 후속 연구 아이디어
- 복소수 연산으로 인한 구현 복잡도 증가
- 비정상 시계열(non-stationary)에서의 성능 분석 부족
- ※ 메모: 복소수 회전 아이디어는 회전 임베딩(RoPE)과 유사한 동기 공유; 두 방법 결합 가능성 있음

## 6. 참고
- 관련 논문: FEDformer (ICML 2022), FITS (ICLR 2024), TimesNet (ICLR 2023)
- ACM DL: https://dl.acm.org/doi/10.1145/3637528.3671713
- GitHub: https://github.com/SiriZhang45/FRNet

※ 원본 PDF 미취득: arXiv 프리프린트 없음; ACM DL 유료 접근.
