---
title: "Adaptive Normalization for Non-stationary Time Series Forecasting: A Temporal Slice Perspective"
authors: Zhiding Liu et al.
venue: NeurIPS
year: 2023
paper_url: https://openreview.net/forum?id=5BqDSw8r5j
code_url: https://github.com/icantnamemyself/SAN
tags: [normalization, non-stationary, time-domain, model-agnostic, distribution-shift, forecasting]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_SAN.pdf
pdf_source: publisher
---

## TL;DR
SAN(Slice-level Adaptive Normalization)은 전역 인스턴스 정규화 대신 시간 슬라이스 단위로 비정상성을 적응적으로 정규화하는 모델-비종속(model-agnostic) 프레임워크다. 슬라이스별 통계 분포를 명시적으로 추정하고 역정규화(denormalization)함으로써 분포 변화에 강건한 예측을 달성한다.

## 1. 기존 모델의 한계 / 가설
- RevIN 등 기존 정규화 방법은 전체 입력 인스턴스를 하나의 통계량으로 처리하여 세밀한 시간적 분포 변화를 무시한다.
- 비정상 시계열에서 입력과 출력 간 분포 불일치(distribution discrepancy)가 예측 성능을 저하시킨다.
- 가설: 시간 슬라이스 단위의 적응적 정규화가 분포 변화를 더 정밀하게 처리할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 입력 시계열을 시간 슬라이스로 분할하고, 각 슬라이스의 통계(평균·분산)를 경량 네트워크로 추정·정규화 후 예측 시 역정규화.
- **아키텍처**:
  1. **Temporal Slicing**: 입력 시계열을 오버래핑 슬라이스로 분할.
  2. **Adaptive Statistics Estimation**: 경량 MLP로 각 슬라이스의 미래 통계량 추정.
  3. **Normalization/Denormalization**: 슬라이스별 추정 통계로 정규화·역정규화.
- **모델 독립성**: 임의의 기반 모델(PatchTST, iTransformer 등)에 플러그인으로 적용 가능.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 기반 모델에 따라 다름 (기본 336 또는 512)
- Forecast horizons: 96, 192, 336, 720
- Normalization: SAN (슬라이스별 적응적 정규화)
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 기반 모델 설정 준용
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 기반 모델 대비 일관적 향상 |
| 192     | see paper | see paper | see Table 2/3 |
| 336     | see paper | see paper | see Table 2/3 |
| 720     | see paper | see paper | see Table 2/3 |

출처: Table 2, Table 3 of paper.

## 4. 주요 기여
- 슬라이스 단위 적응적 정규화로 비정상 시계열 처리 개선.
- 모델-비종속 플러그인으로 다양한 기반 모델에 적용 가능.
- 기존 정규화(RevIN, DishTS 등) 대비 분포 불일치 처리 개선.
- 경량 통계 추정 모듈로 오버헤드 최소화.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 슬라이스 크기·수 선택에 민감.
- ※ 메모: RevIN이 전역 정규화라면 SAN은 지역 적응적 정규화. 두 접근의 앙상블도 탐구할 만함.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=5BqDSw8r5j
- GitHub: https://github.com/icantnamemyself/SAN
- 관련: RevIN, DishTS, Non-Stationary Transformer (NeurIPS 2022)
- ※ 원본 PDF: NeurIPS proceedings 취득 (arXiv 없음)
