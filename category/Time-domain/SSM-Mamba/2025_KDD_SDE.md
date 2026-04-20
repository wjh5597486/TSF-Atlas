---
title: "SDE: A Simplified and Disentangled Dependency Encoding Framework for State Space Models in Time Series Forecasting"
authors: Zixuan Weng et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2408.12068
code_url: ""
tags: [time-domain, ssm, mamba, channel-dependent, channel-independent, disentangled]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_SDE.pdf
pdf_source: arxiv
---

## TL;DR
SDE는 Mamba 기반 SSM에서 불필요한 비선형 활성함수를 제거하여 S-Mamba를 만들고, 시간 의존성과 채널 간 의존성을 병렬로 분리하여 인코딩하는 Disentangled Dependency Encoding 프레임워크를 제안한다. 9개 실세계 데이터셋에서 기존 SSM/Transformer 기반 모델 대비 일관된 성능 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Mamba 기반 모델(Bi-Mamba, S-Mamba 등)은 Conv1D와 SSM 사이에 불필요한 비선형 활성함수를 포함하여 semantically sparse 시계열 데이터에서 과적합이 발생함.
- 채널 간 의존성과 시간 의존성을 순차적으로 처리하면 두 유형의 의존성이 서로 간섭함.
- 저자들은 (1) 비선형성 제거로 과적합 완화, (2) 두 의존성의 병렬 독립 인코딩으로 간섭 제거가 가능하다고 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Mamba에서 비선형 활성함수를 제거한 S-Mamba 구조 + temporal dependency와 cross-variate dependency를 병렬로 처리하는 Disentangled Encoding.
- **아키텍처 구성요소**:
  1. **Simplification (S-Mamba)**: Conv1D와 SSM 사이의 비선형 활성함수 제거; gating 메커니즘은 유지.
  2. **Dependency-Specific Embedding**: Patch 기반 표현으로 의존성별 임베딩 생성.
  3. **Temporal Dependency Encoding**: 각 채널별로 S-Mamba 독립 적용.
  4. **Cross-Variate Dependency Encoding**: 채널을 무작위 순열한 후 S-Mamba 적용하여 채널 간 관계 학습.
  5. **FFN-based Aggregation**: 두 의존성 표현을 결합하여 최종 예측.
- **시간 복잡도**: patch 수와 채널 수에 대해 near-linear.
- **손실함수**: MSE Loss.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 기본 설정 (336 추정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (논문 기본)
- Train/Val/Test split: ETT 기본 설정
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: MSE
- 기타: 9개 데이터셋 평가 (ECL, ETTh1, ETTh2, ETTm1, ETTm2, Exchange, Traffic, Weather, Solar-Energy)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg(96-720) | 0.443 | 0.432 | 다수 baseline 대비 개선 |

출처: Table 4 of paper (arxiv:2408.12068). 개별 horizon 결과는 Appendix L.2 참조.

※ 메모: 논문 main table에서 4개 horizon 평균값만 보고; 개별 horizon 결과는 appendix에 수록.

## 4. 주요 기여
- Mamba에서 비선형 활성함수 제거가 시계열에서 과적합을 줄인다는 실증적 발견 (S-Mamba 도입).
- temporal dependency와 cross-variate dependency를 병렬로 분리 인코딩하는 Disentangled Dependency Encoding 제안.
- near-linear 시간 복잡도 유지하면서 9개 데이터셋 SOTA.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 채널 수가 매우 클 경우 cross-variate encoding의 random permutation 전략이 최적이 아닐 수 있음.
- ※ 메모: 비선형성 제거가 효과적인 이유를 시계열 데이터의 sparsity 관점에서 이론적으로 뒷받침할 연구 가능.

## 6. 참고
- [arxiv:2408.12068](https://arxiv.org/abs/2408.12068)
- [ACM KDD 2025](https://dl.acm.org/doi/10.1145/3690624)
