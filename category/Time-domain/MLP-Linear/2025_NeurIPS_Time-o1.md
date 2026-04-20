---
title: "Time-o1: Time-Series Forecasting Needs Transformed Label Alignment"
authors: Hao Wang et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.17847
code_url: https://github.com/Master-PLC/Time-o1
tags: [loss, training-strategy, label-alignment, decorrelation, time-domain]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_Time-o1.pdf
pdf_source: arxiv
---

## TL;DR
Time-o1은 시계열 예측에서 표준 Temporal MSE의 두 가지 한계(레이블 자기상관 무시, 과도한 태스크 복잡도)를 해결하는 변환 기반 학습 목적 함수다. 레이블 시퀀스를 비상관 성분으로 변환하고 유의미한 성분에 집중 학습하여 자기상관 편향을 줄이고 태스크 양을 감소시킨다.

## 1. 기존 모델의 한계 / 가설
- 표준 Temporal MSE는 레이블 시퀀스의 자기상관(autocorrelation) 효과를 무시.
- 장기 예측에서 과도한 태스크 복잡도 문제 존재.
- 가설: 레이블을 비상관 성분으로 변환하고 가장 유의미한 성분만 학습하면 자기상관 편향이 감소하고 성능이 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: 레이블 시퀀스를 비상관(decorrelated) 성분으로 변환하고, 판별력 있는(discriminated significance) 성분에 정렬(align).
- **구현**:
  - 레이블 시퀀스 → 비상관 성분 변환(직교 변환 등)
  - 중요도에 따른 성분 선택 및 정렬 학습
  - 가장 중요한 성분에 집중함으로써 자기상관 편향 완화 + 태스크 복잡도 감소
- **특성**: Transform-enhanced loss function. 기존 모델의 학습 과정에 적용 가능.
- **적용 결과**: 다수 예측 모델에서 SOTA 성능 달성.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): backbone 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: 표준 split
- Batch size / Optimizer / LR: backbone 모델 설정 따름
- Loss function: Transformed Label Alignment Loss (제안)
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체적 수치는 논문 PDF 참조 필요. 기존 backbone 모델 대비 상대적 향상이 핵심.

## 4. 주요 기여
- Temporal MSE의 레이블 자기상관 문제와 태스크 복잡도 문제 명시적 정의.
- 레이블 변환 + 비상관 성분 정렬 학습 전략 제안.
- Model-Agnostic Loss: 기존 TSF 모델에 적용하여 SOTA 달성.

## 5. 한계 및 후속 연구 아이디어
- 변환 방식 선택(직교 변환 등)에 따른 성능 의존성.
- 더 적응적인 성분 선택 전략 연구 필요.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.17847
- GitHub: https://github.com/Master-PLC/Time-o1
- OpenReview: https://openreview.net/forum?id=RxWILaXuhb
