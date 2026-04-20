---
title: "Improving Time Series Forecasting via Instance-aware Post-hoc Revision"
authors: Zhiding Liu et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.23583
code_url: https://github.com/icantnamemyself/PIR
tags: [time-domain, mlp-linear, post-hoc, model-agnostic, instance-aware, distribution-shift]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_PIR.pdf
pdf_source: arxiv
---

## TL;DR
PIR(Post-forecasting Identification and Revision)은 모델 비종속(model-agnostic) 프레임워크로, 분포 이동·결측 데이터·롱테일 패턴으로 인한 편향된 예측 인스턴스를 사후(post-hoc) 식별하고, 로컬·글로벌 관점의 컨텍스트 정보를 이용해 수정한다. 기존 모델의 훈련 없이 예측 정확도를 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- 강력한 전체 성능에도 불구하고, 개별 인스턴스 수준에서 심각한 편향이 존재.
- 분포 이동, 결측값, 롱테일 패턴이 특정 인스턴스에서의 예측 실패를 유발.
- 가설: 사후적으로 편향된 인스턴스를 식별하고 컨텍스트 기반으로 수정하면 전반적 성능을 향상시킬 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 사전학습된 예측 모델의 출력을 사후에 식별·수정하는 model-agnostic 프레임워크.
- **아키텍처**:
  - **식별(Identification)**: 편향된 예측 인스턴스를 정확도 추정으로 식별.
  - **수정(Revision)**: 로컬(인접 인스턴스) + 글로벌(전체 컨텍스트) 관점의 컨텍스트 정보로 사후 수정.
- **Model-Agnostic**: 기존 TSF 모델의 훈련 변경 없이 사후에 적용 가능.
- **손실함수**: 기존 모델 손실 유지.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): backbone 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: backbone 모델 설정 따름
- Loss function: backbone 모델 손실 (PIR은 사후 수정)
- 기타: 실제 데이터셋(ETT, Electricity, Weather, Traffic 포함) 다수 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 절대값보다 backbone 대비 개선폭이 핵심.

## 4. 주요 기여
- 인스턴스 수준 편향 문제를 명시적으로 정의하고 사후 수정 프레임워크 제안.
- 로컬·글로벌 컨텍스트 이중 관점의 수정 메커니즘.
- Model-Agnostic으로 다양한 기존 TSF 모델에 적용 가능.
- 분포 이동, 결측, 롱테일 인스턴스에서 특히 효과적.

## 5. 한계 및 후속 연구 아이디어
- 편향 인스턴스 식별 기준의 신뢰성 확보 필요.
- 온라인 예측 환경으로의 확장 가능성.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.23583
- GitHub: https://github.com/icantnamemyself/PIR
- OpenReview: https://openreview.net/forum?id=H7e5RpeIi4
