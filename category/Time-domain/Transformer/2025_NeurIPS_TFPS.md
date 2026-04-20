---
title: "Learning Pattern-Specific Experts for Time Series Forecasting Under Patch-level Distribution Shift"
authors: Yanru Sun et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2410.09836
code_url: https://github.com/syrGitHub/TFPS
tags: [time-domain, transformer, patching, mixture-of-experts, distribution-shift]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Patching
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TFPS.pdf
pdf_source: arxiv
---

## TL;DR
TFPS는 패치 수준의 분포 이동 문제를 해결하기 위해 이중 도메인 인코더(시간+주파수)로 패치 패턴을 파악하고, Mixture of Pattern Experts(MoPE)를 통해 패턴별 전문가 모델로 예측하는 Transformer 기반 TSF 모델이다. 다양한 패턴이 혼재하는 장기 예측에서 강점을 보인다.

## 1. 기존 모델의 한계 / 가설
- 시계열 데이터는 서로 다른 구간에서 다양하고 비균일한 패턴 분포를 보임.
- 기존 모델들은 모든 패치를 동일한 모델로 처리하여 패치 수준 분포 이동에 취약함.
- 가설: 패치별로 패턴을 동적으로 식별하고 패턴별 전문가 모델을 적용하면 장기 예측 성능이 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: 이중 도메인 인코더로 패턴을 식별하고, 패턴별 전문가 모델(MoPE)로 예측.
- **아키텍처**:
  - **Dual-Domain Encoder (DDE)**: 시간 도메인과 주파수 도메인의 특징을 동시에 추출.
  - **Pattern Identifier (PI)**: 부분공간 클러스터링(subspace clustering)으로 패치의 패턴을 동적 식별.
  - **Mixture of Pattern Experts (MoPE)**: 각 패턴에 특화된 전문가 모델이 패턴별 예측 담당.
- **학습 전략**: 동적 패턴 인식을 통한 패턴-적응적 학습.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 인스턴스 정규화
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather, Traffic, Exchange Rate 등 포함

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: ETTh2, ETTm2에서 각각 MSE 10.9%, 7.3% 향상 보고(논문).

## 4. 주요 기여
- 패치 수준 분포 이동 문제를 최초로 명시적으로 정의하고 해결 프레임워크 제안.
- Dual-Domain Encoder로 시간·주파수 특징 통합.
- Mixture of Pattern Experts(MoPE)를 통한 동적 패턴별 예측.
- 장기 예측(특히 분포 이동이 강한 ETTh2, ETTm2)에서 유의미한 성능 향상.

## 5. 한계 및 후속 연구 아이디어
- 전문가 수 및 클러스터링 방식의 하이퍼파라미터 민감도.
- 더 세밀한 패턴 분류 체계로의 확장 가능성.

## 6. 참고
- arxiv: https://arxiv.org/abs/2410.09836
- GitHub: https://github.com/syrGitHub/TFPS
- OpenReview: https://openreview.net/forum?id=qVyjN01x4P
