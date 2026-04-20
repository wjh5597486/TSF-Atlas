---
title: "Towards Accurate Time Series Forecasting via Implicit Decoding"
authors: Xinyu Li et al.
venue: NeurIPS
year: 2025
paper_url: https://openreview.net/forum?id=gqoeQPhQcE
code_url: https://github.com/rakuyorain/Implicit-Forecaster
tags: [freq-domain, fft, mlp, implicit-decoding, wave-decomposition, plug-in]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_ImplicitForecaster.pdf
pdf_source: openreview
---

## TL;DR
Implicit Forecaster(IF)는 개별 타임스텝을 독립적으로 예측하는 대신, 시계열을 구성하는 파동의 주파수·진폭·위상을 암묵적으로 예측하는 디코딩 모듈이다. 기존 예측 모델에 플러그인 방식으로 추가되어 일관되게 성능을 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 미래 타임스텝을 개별적이고 독립적으로 예측하여 전체 시계열의 구조적 패턴을 놓침.
- 각 타임포인트를 독립적으로 예측하면 시계열의 연속성·주기성 등 글로벌 구조가 반영되지 않음.
- 가설: 시계열을 구성하는 파동(frequency, amplitude, phase)을 암묵적으로 예측하면 더 정확하고 구조적인 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 개별 포인트 예측 대신, 시계열을 구성하는 파동의 주파수·진폭·위상을 암묵적으로 예측하는 IF(Implicit Forecaster) 모듈.
- **아키텍처**:
  - **IF Module**: 기존 예측 모델의 출력에 추가되는 디코딩 모듈.
  - 구성 파동의 주파수, 진폭, 위상을 암묵적으로 추정.
  - 추정된 파동을 합성하여 최종 예측 생성.
- **특성**: Plug-and-Play 방식. 다양한 기존 주류 TSF 모델에 적용 가능.
- **손실함수**: 기존 모델 손실 + IF 모듈 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): backbone 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: 표준 split
- Batch size / Optimizer / LR: backbone 모델 설정 따름
- Loss function: 기존 모델 손실 + IF 디코딩 손실
- 기타: 다수의 실제 데이터셋 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper (backbone 대비 일관적 향상) |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 다수의 주류 모델에서 일관적 성능 향상 보고.

## 4. 주요 기여
- 타임포인트 개별 예측 대신 파동(frequency, amplitude, phase) 기반 암묵적 디코딩 모듈 제안.
- Plug-and-Play 방식으로 기존 TSF 모델에 적용 가능.
- 다양한 주류 모델에서 일관된 성능 향상 달성.

## 5. 한계 및 후속 연구 아이디어
- 파동 수(wave count) 선택의 하이퍼파라미터 민감도.
- 비정상(non-stationary) 시계열에서의 동적 파동 적응 방안.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=gqoeQPhQcE
- GitHub: https://github.com/rakuyorain/Implicit-Forecaster
※ 원본 PDF 미취득: arXiv ID 없음. OpenReview에서 다운로드 필요.
