---
title: "AliO: Output Alignment Matters in Long-Term Time Series Forecasting"
authors: Kwangryeol Park et al.
venue: NeurIPS
year: 2025
paper_url: https://openreview.net/forum?id=AuOZDp4gy7
code_url: https://github.com/eai-lab/AliO
tags: [time-domain, mlp-linear, output-alignment, loss, frequency-domain, model-agnostic]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Loss

reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_AliO.pdf
pdf_source: openreview
---

## TL;DR
AliO는 LTSF 모델에서 동일 미래 타임스텝에 대한 예측이 입력 시퀀스가 약간 달라질 때 불일치하는 '출력 정렬 문제'를 해결한다. TAM(Time Alignment Metric) 도입과 시간·주파수 도메인 Regression Pulling(RegPull)을 통해 TAM을 최대 58.2%, 예측 성능을 최대 27.5% 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- LTSF 모델은 동일 미래 타임스텝에 대해 입력 윈도우가 약간 다를 때 다른 예측을 출력하는 '출력 정렬 문제' 존재.
- 기존 모델들은 이 문제를 정의하거나 해결하지 않음.
- 가설: 출력 정렬을 개선하면 더 일관적인 예측과 함께 전반적 예측 정확도도 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: 서로 다른 입력 윈도우에서 동일 타임스텝에 대한 예측 일치도를 최대화.
- **아키텍처**:
  - **TAM(Time Alignment Metric)**: 출력 정렬 품질을 정량화하는 새로운 평가 지표.
  - **Regression Pulling (RegPull)**: Stop-gradient 연산 적용. 덜 정확한 예측을 더 정확한 예측으로 당기는 방식. 시간 도메인과 주파수 도메인 모두에서 적용.
  - **시간 도메인 RegPull**: 시간적 패턴 정렬.
  - **주파수 도메인 RegPull**: 위상(phase)·진폭(amplitude) 성분 정렬.
- **Model-Agnostic**: 기존 LTSF 모델에 추가 가능.
- **손실함수**: 기존 손실 + RegPull Loss.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): backbone 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: backbone 모델 설정 따름
- Loss function: 기존 손실 + RegPull Loss
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper (backbone 대비 향상) |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: TAM 58.2%, 예측 MSE 27.5% 향상(일부 설정).

## 4. 주요 기여
- 출력 정렬 문제(Output Alignment Problem) 최초 정의.
- TAM(Time Alignment Metric) 평가 지표 제안.
- 시간·주파수 이중 도메인 Regression Pulling 전략.
- Model-Agnostic 적용 가능.

## 5. 한계 및 후속 연구 아이디어
- RegPull 가중치 설정의 하이퍼파라미터 민감도.
- 출력 정렬 문제가 실제 응용에서의 중요성 추가 검증 필요.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=AuOZDp4gy7
- GitHub: https://github.com/eai-lab/AliO
※ 원본 PDF 미취득: arXiv ID 없음. OpenReview에서 다운로드 필요.
