---
title: "Reversible Instance Normalization for Accurate Time-Series Forecasting against Distribution Shift"
authors: Taesung Kim, Jinhee Kim, Yunwon Tae, Cheonbok Park, Jang-Ho Choi, Jaegul Choo
venue: ICLR 2022
year: 2022
paper_url: https://arxiv.org/abs/2106.11959
code_url: https://github.com/ts-kim/RevIN
tags: [normalization, instance-normalization, reversible, denormalization, distribution-shift, non-stationary, time-domain]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_ICLR_RevIN.pdf
pdf_source: arxiv
---

## TL;DR
RevIN은 시계열의 통계적 특성(평균, 분산) 변화(distribution shift)에 대응하기 위해 정규화(normalization)와 역정규화(denormalization)를 대칭적으로 적용하는 방법이다. 인스턴스별 정규화와 학습 가능한 아핀 변환으로 시간 변동성을 효과적으로 처리하며, 모든 시계열 예측 모델에 적용 가능하다.

## 1. 기존 모델의 한계 / 가설
- 시계열의 평균과 분산은 시간에 따라 지속적으로 변화(distribution shift)하는데, 이는 시계열 예측의 주요 도전 과제다.
- 전통적인 정규화(표준화)는 전체 시계열에 대해 고정 통계량을 사용하므로 시간 변동성에 대응하기 어렵다.
- 가설: 입력 단계에서 인스턴스별(per-instance) 정규화를 적용하고, 출력 단계에서 역으로 스케일을 복원하면, 모델이 안정적인 분포를 학습하면서도 본질적인 비정상성을 보존할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 정규화-모델 학습-역정규화의 대칭적 세 단계 프로세스로 분포 이동에 강건한 예측 실현.
  
- **방법**:
  1. **정규화 (Normalization)**
     - 각 인스턴스(시간 윈도우)에 대해 독립적으로 표준화
     - 평균: μ = mean(X)
     - 표준편차: σ = std(X)
     - 정규화된 X': (X - μ) / σ
  
  2. **학습 가능한 아핀 변환 (Learnable Affine)**
     - 스케일 감마(γ)와 편이 베타(β)는 학습 가능
     - X'' = γ·X' + β (정규화 후 적용 가능하나 일반적으로 후처리)
  
  3. **모델 학습**
     - 정규화된 입력으로 예측 모델 훈련
     - 모델은 안정적인 분포 범위 내에서 학습
  
  4. **역정규화 (Denormalization)**
     - 모델 출력을 원본 스케일로 복원
     - Ŷ = Ŷ'·σ + μ
     - 예측값이 원본 스케일을 가지므로 공정한 성능 평가 가능
  
- **핵심 특징**:
  - 모든 시계열 예측 아키텍처(Transformer, MLP, LSTM 등)에 적용 가능한 범용 기법
  - 모델 구조 변경 없이 전처리/후처리로 적용 가능
  - 크로스 도메인 예측 (cross-domain forecasting)에서 특히 강건함

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (제안 방법) 또는 표준 정규화 비교
- Train/Val/Test split: 표준 ETT 분할 (비율 0.6:0.2:0.2)
- Batch size / Optimizer / LR: 기본 설정 (모델별 동일)
- Loss function: MSE
- 기타: 다양한 백본 모델(Informer, Autoformer, Transformer) 테스트

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | RevIN 적용 시 일반 정규화 대비 개선 |
| 192     | see paper | see paper | see Table 2, 3 |
| 336     | see paper | see paper | see Table 2, 3 |
| 720     | see paper | see paper | see Table 2, 3 |

출처: Table 2, 3 of paper (ETTh1, ETTh2, ETTm1, ETTm2 및 Electricity, Traffic, Weather 벤치마크).

## 4. 주요 기여
- 시계열의 분포 이동(distribution shift)이 예측 성능 저하의 핵심 요인임을 실증적으로 입증.
- 간단하지만 효과적인 인스턴스 정규화-역정규화 기법으로 모든 모델의 성능 개선.
- 크로스 도메인 예측에서 특히 강건함을 보임 (한 데이터셋에서 학습, 다른 데이터셋에서 평가).
- ICLR 2022 포스터 발표.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 매우 극단적인 이상치(outlier)가 있는 데이터에서는 표준편차 계산이 불안정할 수 있음.
- ※ 메모: RevIN은 이후 거의 모든 최신 시계열 예측 논문의 표준 정규화 기법으로 채택됨. Non-Stationary Transformer, PatchTST, TimesNet 등 주요 모델들이 RevIN을 기본으로 사용. 이는 시계열 예측 커뮤니티에서 가장 영향력 있는 기법 중 하나가 됨.

## 6. 참고
- arXiv: https://arxiv.org/abs/2106.11959
- GitHub: https://github.com/ts-kim/RevIN
- ICLR 2022 포스터 발표
- 관련: Non-Stationary Transformer (NeurIPS 2022), PatchTST (ICLR 2023), TimesNet (ICLR 2023)
