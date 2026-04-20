---
title: "Frequency Adaptive Normalization For Non-stationary Time Series Forecasting"
authors: Nan Mu et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2409.20371
code_url: ""
tags: [normalization, non-stationary, fft, freq-domain, mlp, model-agnostic]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_FAN.pdf
pdf_source: arxiv
---

## TL;DR
FAN(Frequency Adaptive Normalization)은 푸리에 변환으로 인스턴스별 지배적 주파수 성분을 식별하여 비정상(non-stationary) 요인을 정규화하는 모델 독립적 방법. RevIN의 한계를 주파수 도메인에서 극복.

## 1. 기존 모델의 한계 / 가설
- RevIN 등 기존 인스턴스 정규화는 추세(trend)는 처리하지만 계절성(seasonal) 비정상성은 다루지 못함.
- 동적 추세와 계절 패턴이 혼합된 데이터에 대한 통합 정규화 부재.
- 가설: 주파수 도메인에서 지배적 성분을 식별하고 그 차이를 명시적으로 예측하면 비정상성 완화 가능.

## 2. 제안 방법론
- **핵심 아이디어**: FFT로 인스턴스별 Top-K 지배 주파수 성분 추출 → MLP로 입력-출력 간 주파수 차이 예측 → 정규화.
- **아키텍처**:
  - 입력에 FFT 적용 → Top-K 지배 주파수 성분 선택
  - 이들 주파수 성분의 입력/출력 간 차이를 MLP로 예측
  - 예측 결과로 역정규화 수행 (모델 예측값에 주파수 보정 추가)
  - 임의 예측 백본과 결합 가능 (model-agnostic)
- **손실함수**: MSE (백본 손실 + 주파수 예측 손실)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: FAN (RevIN 대체 또는 보완)
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | RevIN 대비 7.76~37.90% MSE 향상 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 주파수 도메인 인스턴스 정규화 최초 적용
- 동적 추세 + 계절 비정상성 통합 처리
- 모델 독립적(model-agnostic) 설계
- RevIN 대비 7.76~37.90% MSE 개선

## 5. 한계 및 후속 연구 아이디어
- Top-K 주파수 성분 수 K 선택이 성능에 영향
- 불규칙 샘플링 시계열에서 FFT 적용 제한

## 6. 참고
- [arXiv 2409.20371](https://arxiv.org/abs/2409.20371)
