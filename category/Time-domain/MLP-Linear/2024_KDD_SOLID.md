---
title: "Calibration of Time-Series Forecasting: Detecting and Adapting Context-Driven Distribution Shift"
authors: Mouxiang Chen et al.
venue: KDD 2024
year: 2024
paper_url: https://arxiv.org/abs/2310.14838
code_url: https://github.com/HALF111/calibration_cds
tags: [normalization, distribution-shift, calibration, non-stationary, context-driven, plug-and-play]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_KDD_SOLID.pdf
pdf_source: arxiv
---

## TL;DR
SOLID는 학습된 예측 모델의 결과를 사후 보정(calibration)하여 context-driven distribution shift(CDS)를 감지하고 적응하는 범용 방법론이다. ETTh1/h2 포함 8개 표준 TSF 벤치마크에서 기존 모델의 성능을 일관적으로 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델들은 특정 temporal context(시간대, 계절 등)에서 편향된 예측(CDS)을 보임.
- CDS는 observable context(시간 인덱스 등)와 unobservable context(숨겨진 원인)에 의해 발생.
- 예측 모델을 재학습하지 않고도 사후 보정으로 이를 해결할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 학습된 모델의 잔차(residual)와 context 간 mutual information을 계량화하여 CDS를 감지하고, context-specific 경량 보정 레이어(linear calibration)를 추가하여 적응.
- **아키텍처 구성요소**:
  - Residual-based CDS detector ("Reconditioner"): 예측 잔차와 context의 상호정보량 계산
  - Context-specific calibration: 감지된 CDS에 맞춰 예측 결과 후처리 보정 (경량 linear layer)
  - Plug-and-play 구조: 임의의 기학습 모델에 적용 가능
- **손실함수**: 기존 모델 loss + CDS 보정 목적함수
- **학습 전략**: Pretrained 모델 고정 후 calibration 레이어만 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Traffic, Illness, Weather 8개 데이터셋
- Batch size / Optimizer / LR: 상세는 원문 참고
- Loss function: MSE + Calibration 목적함수

### 3.2 Results
> 상세는 원문 Table 참고. SOLID는 PatchTST, iTransformer, TimesNet 등 다양한 베이스 모델에 적용 시 ETTh1을 포함한 8개 데이터셋 전반에서 MSE/MAE 감소 효과 보고.

출처: 원문 Table 참고.

## 4. 주요 기여
- Context-Driven Distribution Shift(CDS) 개념 정의 및 감지 방법 제시
- Plug-and-play 사후 보정(calibration) 프레임워크 SOLID 제안
- 다양한 베이스 모델과 데이터셋에서 범용성 실증
- Observable/Unobservable context 모두 처리 가능

## 5. 한계 및 후속 연구 아이디어
- Calibration 효과가 베이스 모델 종류에 따라 달라질 수 있음
- 온라인 적응(online adaptation) 시나리오에서의 효율성 미검증
- ※ 메모: RevIN, SAN 등 기존 normalization 방법과 비교 시 보완적 관계 가능성

## 6. 참고
- 관련 논문: RevIN (ICLR 2022), Non-stationary Transformer (NeurIPS 2022)
- ACM DL: https://dl.acm.org/doi/10.1145/3637528.3671926
- GitHub: https://github.com/HALF111/calibration_cds
- 저자 프리프린트: https://me.keytoix.vip/assets/KDD24-Calibration-Full.pdf
