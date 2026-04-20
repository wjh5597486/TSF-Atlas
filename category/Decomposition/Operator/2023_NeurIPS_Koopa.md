---
title: "Koopa: Learning Non-stationary Time Series Dynamics with Koopman Predictors"
authors: Yong Liu et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2305.18803
code_url: https://github.com/thuml/Koopa
tags: [koopman, decomposition, non-stationary, time-domain, mlp, forecasting]
primary_category: category/Decomposition/Operator
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_Koopa.pdf
pdf_source: arxiv
---

## TL;DR
Koopa는 비정상(non-stationary) 시계열을 시불변(time-invariant)과 시변(time-variant) 성분으로 분리한 뒤, Koopman 연산자 이론에 기반한 예측기를 통해 선형 공간에서 효율적으로 예측한다. Fourier 필터로 주기·트렌드를 분리하고, Koopman Predictor로 각 성분을 별도 처리함으로써 비정상성을 명시적으로 다룬다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 비정상 시계열에서 분포 변화(distribution shift)로 인해 성능이 저하된다.
- Transformer, MLP 계열 모두 시불변 가정 하에서 설계되어 시변 동역학 모델링에 한계가 있다.
- 가설: Koopman 연산자 이론을 통해 비선형 비정상 시계열을 선형 공간으로 리프팅하면 효율적인 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: Fourier 필터로 시계열을 시불변 성분(주기·트렌드)과 시변 성분으로 분리 후, Koopman 예측기로 각 성분을 독립 모델링.
- **아키텍처**:
  1. **Fourier Filter**: FFT 기반으로 시불변(주기/트렌드) vs. 시변 성분 분리.
  2. **Koopman Predictor**: 시변 성분을 고차원 선형 공간으로 임베딩하여 선형 전이 행렬로 예측.
  3. 두 예측 결과를 합산하여 최종 예측.
- **수식**: Koopman 연산자 K는 비선형 관측 함수 g를 통해 상태 전이를 선형화: $g(x_{t+1}) = K \cdot g(x_t)$.
- 스택형 블록으로 구성되어 다중 스케일 처리 가능.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam 옵티마이저
- Loss function: MSE
- 기타: 논문 Appendix 참조

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.399 | 0.416 | see Table 1 |
| 192     | 0.440 | 0.438 | see Table 1 |
| 336     | 0.468 | 0.455 | see Table 1 |
| 720     | 0.484 | 0.481 | see Table 1 |

출처: Table 1 of paper.

## 4. 주요 기여
- 비정상 시계열 분해 + Koopman 연산자 결합의 새로운 프레임워크.
- Fourier 필터로 시불변/시변 성분 명시적 분리.
- 선형 Koopman 예측기로 연산 효율과 해석 가능성 향상.
- 다양한 비정상 벤치마크에서 경쟁력 있는 성능 달성.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: Koopman 임베딩 차원 선택에 민감할 수 있음.
- ※ 메모: ContiFormer(Neural ODE)와 비교하면 동역학계 접근의 공통점이 있음. 확률적 Koopman 확장이 흥미로운 방향.

## 6. 참고
- arXiv: https://arxiv.org/abs/2305.18803
- GitHub: https://github.com/thuml/Koopa
- 관련: ContiFormer (Neural ODE 기반 동역학계 접근)
