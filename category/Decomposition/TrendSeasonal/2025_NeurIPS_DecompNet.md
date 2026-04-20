---
title: "DecompNet: Enhancing Time Series Forecasting Models with Implicit Decomposition"
authors: Donghao Luo et al.
venue: NeurIPS
year: 2025
paper_url: https://openreview.net/forum?id=ioXn68lBjO
code_url: https://github.com/luodhhh/DecompNet
tags: [decomposition, trend-seasonal, implicit, model-agnostic, time-domain]
primary_category: category/Decomposition/TrendSeasonal
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_DecompNet.pdf
pdf_source: openreview
---

## TL;DR
DecompNet은 시계열 분해 지식을 추론 비용 없이 예측 모델에 암묵적으로 전달하는 새로운 분해 기반 향상 프레임워크다. 명시적 분해 없이 분해의 성능 향상 효과를 상속받으며, 최신 정규화 기반 프레임워크를 최초로 능가한다.

## 1. 기존 모델의 한계 / 가설
- 명시적 추세-계절 분해는 추론 비용을 증가시키고, 분해 정확도에 의존함.
- 기존 정규화 기반 향상(RevIN 등)은 성능 향상에 한계 존재.
- 가설: 분해 지식을 암묵적으로 모델에 주입하면, 추론 비용 없이 분해의 이점을 누릴 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 추론 시 시계열을 실제로 분해하지 않고, 분해 관련 지식을 예측 모델에 암묵적으로 전달.
- **아키텍처**:
  - DecompNet은 분해 기반 교사(teacher) 신호를 통해 학습 시 분해 지식을 학생(student) 예측 모델에 이식.
  - 추론 시에는 분해 연산 없이 학습된 모델만 사용 → 추가 추론 비용 없음.
- **특성**: 명시적 분해 없이 분해의 성능 향상 효과 상속. 기존 TSF 모델에 적용 가능.
- **결과**: 기존 정규화 기반 향상 프레임워크 최초 능가.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): backbone 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: backbone 모델 설정 따름
- Loss function: 기존 모델 손실 + 분해 지식 이식 손실
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체적 수치는 논문 참조 필요. 정규화 기반 프레임워크 능가가 핵심 주장.

## 4. 주요 기여
- 암묵적 분해(Implicit Decomposition) 개념 최초 제안.
- 추론 비용 없이 분해 성능 향상 상속.
- 기존 최신 정규화 기반 향상 프레임워크(RevIN 등) 최초 능가.
- 최신 SOTA 모델 성능 한계를 크게 향상.

## 5. 한계 및 후속 연구 아이디어
- 분해 교사 신호의 품질에 의존.
- 더 복잡한 분해 패턴(다중 계절성 등)에 대한 확장 가능성.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=ioXn68lBjO
- GitHub: https://github.com/luodhhh/DecompNet
※ 원본 PDF 미취득: arXiv ID 없음. OpenReview에서 다운로드 필요.
