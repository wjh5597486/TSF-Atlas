---
title: "ST-MTM: Masked Time Series Modeling with Seasonal-Trend Decomposition for Time Series Forecasting"
authors: Hyunwoo Seo et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2507.00013
code_url: ""
tags: [self-supervised, masking, decomposition, seasonal-trend, mlp, fft, transformer, channel-independent]
primary_category: category/SelfSupervised
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_ST-MTM.pdf
pdf_source: arxiv
---

## TL;DR
ST-MTM은 계절-추세 분해(seasonal-trend decomposition)를 Masked Time Series Modeling에 통합한 자기지도 사전학습 프레임워크다. Seasonal Frequency MLP(FFT 기반)로 계절 성분을, Transformer로 추세 성분을 인코딩하고, 컨텍스트 대조 학습으로 도메인 간 전이 성능을 강화한다. 9개 벤치마크에서 대부분의 자기지도 및 감독 예측 기법 대비 우수한 성능을 보인다.

## 1. 기존 모델의 한계 / 가설
- 기존 Masked Time Series Modeling(SimMTM, TimeSiam 등)은 계절/추세 성분을 구분하지 않고 단일 마스킹을 적용하여 서로 다른 주파수 특성을 동시에 학습하기 어려움.
- 단일 구조 인코더는 고주파 계절 패턴과 저주파 추세 패턴을 효과적으로 동시에 포착하지 못함.
- 저자들은 분해 후 각 성분에 최적화된 인코더를 적용하면 크로스 도메인 전이 성능이 향상된다고 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 계절·추세로 분해한 뒤 각 성분에 적합한 인코더를 별도로 사용하는 Masked Modeling + Contrastive Learning 통합 사전학습.
- **아키텍처 구성요소**:
  1. **Decomposition**: Moving Average 연산으로 계절·추세 분리.
  2. **Seasonal Encoder (SFM)**: FFT → 주파수별 독립 MLP → IFFT (Seasonal Frequency MLP).
  3. **Trend Encoder**: Multi-layer Transformer (채널 독립적).
  4. **Component-wise Gating**: 두 성분 표현을 가중 결합.
  5. **Contextual Contrastive Learning**: 원본-재구성 쌍 간 유사성 최대화.
  6. **Decoder**: Linear Layer (채널 차원).
- **손실함수**: Reconstruction Loss + Contrastive Loss.
- **학습 전략**: 사전학습 후 linear fine-tuning (또는 full fine-tuning).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720 (평균 보고)
- Normalization: 논문 기본 설정
- Train/Val/Test split: ETT 기본 설정
- Batch size: 후보 {16, 32, 64, 128} 중 선택
- Loss function: MSE (fine-tuning) + Reconstruction + Contrastive (사전학습)
- 기타: 9개 데이터셋 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, PEMS08, ILI, Solar), cross-domain 전이 평가 포함

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg (96-720) | 0.413 | 0.429 | 대부분의 자기지도 baseline 대비 best/2nd |

출처: 논문 Table (arxiv:2507.00013). ※ 개별 horizon 결과는 논문 참조.

## 4. 주요 기여
- 계절·추세 분해를 Masked Time Series Modeling에 통합한 ST-MTM 제안.
- FFT 기반 Seasonal Frequency MLP로 고주파 계절 패턴의 효율적 모델링.
- Contextual Contrastive Learning으로 크로스 도메인 전이 성능 강화.
- 9개 벤치마크에서 감독·자기지도 방법 대비 일관된 성능 우위.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: Moving Average 기반 분해는 계절 주기가 뚜렷하지 않은 데이터에서 불안정할 수 있음.
- ※ 메모: SFM의 주파수별 독립 MLP가 어느 주파수를 중점 학습하는지 주파수 attribution 분석 가능.

## 6. 참고
- [arxiv:2507.00013](https://arxiv.org/abs/2507.00013)
- ACM KDD 2025 Research Track
