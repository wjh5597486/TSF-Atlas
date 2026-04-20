---
title: "FilterNet: Harnessing Frequency Filters for Time Series Forecasting"
authors: Kun Yi et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2411.01623
code_url: https://github.com/aikunyi/FilterNet
tags: [freq-domain, learnable-filter, fft, time-series, non-stationary]
primary_category: category/Freq-domain/LearnableFilter
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_FilterNet.pdf
pdf_source: arxiv
---

## TL;DR
FilterNet은 두 종류의 학습 가능한 주파수 필터를 제안: (1) 고주파 잡음을 처리하는 Plain Shaping Filter와 (2) 전체 주파수 스펙트럼을 활용하는 Contextual Shaping Filter. 전통적 디지털 신호 처리 필터를 딥러닝에 통합.

## 1. 기존 모델의 한계 / 가설
- 기존 주파수 도메인 모델(FEDformer 등)은 주파수 성분 일부만 선택적으로 사용하거나 학습 불가능한 고정 필터 사용.
- 고주파 잡음을 제대로 처리하지 못해 비정상(non-stationary) 시계열에 취약.
- 가설: 학습 가능한 주파수 필터로 전체 스펙트럼을 adaptive하게 활용하면 성능 향상 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 두 종류 학습 가능 필터로 주파수 도메인에서 적응적 신호 처리.
- **아키텍처**:
  - Plain Shaping Filter: 고주파 잡음 억제를 위한 학습 가능 저역통과 필터
  - Contextual Shaping Filter: 전체 주파수 스펙트럼 활용, 문맥 인식 필터링
  - FFT → 필터 적용 → iFFT → 예측 head
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 기존 필터 기반 모델 대비 향상 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- Plain + Contextual Shaping Filter 두 종류의 학습 가능 주파수 필터 제안
- 전통적 디지털 신호처리 필터를 딥러닝으로 통합하는 이론적 근거
- 고주파 잡음과 전체 스펙트럼 활용을 동시에 해결
- 다양한 TSF 벤치마크에서 효과 검증

## 5. 한계 및 후속 연구 아이디어
- 주파수 도메인 처리로 인한 주기성 가정 내재
- 비주기적 이상 시계열에서 효과 제한 가능

## 6. 참고
- NeurIPS 2024 Proceedings
