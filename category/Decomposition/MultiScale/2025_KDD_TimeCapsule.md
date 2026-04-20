---
title: "TimeCapsule: Solving the Jigsaw Puzzle of Long-Term Time Series Forecasting with Compressed Predictive Representations"
authors: (authors from arxiv:2504.12721)
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2504.12721
code_url: ""
tags: [time-domain, mlp, decomposition, multi-scale, tensor, compression, channel-independent]
primary_category: category/Decomposition/MultiScale
related_categories:
  - category/Time-domain/MLP-Linear
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_TimeCapsule.pdf
pdf_source: arxiv
---

## TL;DR
TimeCapsule models time series as a 3D tensor (temporal × variate × level dimensions) and uses mode production to capture multi-mode dependencies while achieving dimensionality compression. It unifies key techniques (patching, decomposition, downsampling) in a generalized framework based on Tucker-style tensor operations. Evaluated on 10 diverse datasets including all ETT variants.

## 1. 기존 모델의 한계 / 가설
- 기존 LTSF 모델들은 patching, 분해(decomposition), 다운샘플링 등 각 기법을 독립적으로 적용하여 3차원 시계열 정보(시간, 채널, 레벨)를 통합 모델링하지 못함.
- 고차원 시계열을 단순 2D로 처리하면 temporal-variate-level 간 상호작용 정보가 손실됨.
- 저자들은 시계열을 3D 텐서로 모델링하고 Tucker 분해 기반 mode production으로 차원 압축을 적용하면 각 차원의 특성을 동시에 학습할 수 있다고 주장.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 temporal × variate × level 3D 텐서로 구성한 뒤 Tucker 텐서 분해(mode production)로 각 차원을 독립 압축하는 통합 프레임워크.
- **아키텍처 구성요소**:
  1. **3D Tensor Formulation**: 시계열 패치를 temporal(T), variate(V), level(L) 3축으로 배열.
  2. **Mode Production**: Tucker 분해 기반으로 각 모드(T, V, L)별 행렬을 학습하여 low-rank 압축 표현 생성.
  3. **JEPA Training (Joint-Embedding Predictive Architecture)**: 예측 표현 공간에서 학습하여 노이즈에 강건한 특성 학습.
  4. **Linear Forecasting Head**: 압축된 표현에서 최종 예측 생성.
- **손실함수**: MSE Loss (기본), JEPA 방식으로 표현 공간 학습 포함.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 기본 설정 (336 추정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 논문 기본 설정
- Train/Val/Test split: ETT 기본 설정 (12:4:4 months 추정)
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: MSE
- 기타: JEPA 학습 사용; 10개 데이터셋 평가 (ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather, Solar, PEMS04, Traffic, ILI)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.362 | 0.394 | JEPA 학습 사용 결과 |
| 192     | 0.399 | 0.418 | |
| 336     | 0.424 | 0.432 | |
| 720     | 0.424 | 0.447 | |

출처: 논문 Table (arxiv:2504.12721v3, JEPA 학습 결과).

## 4. 주요 기여
- 시계열을 temporal × variate × level 3D 텐서로 공식화하는 통합 프레임워크 제시.
- Tucker 분해 기반 mode production으로 기존 patching/decomposition 기법을 일반화.
- JEPA 학습으로 노이즈에 강건한 예측 표현 공간 학습.
- 다양한 주파수 및 variate 수(7~862)의 10개 데이터셋에서 SOTA 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: Tucker 분해의 rank 선택이 하이퍼파라미터로 남음.
- ※ 메모: JEPA 학습의 예측 공간 표현이 기존 masked prediction과 차별되는 정보론적 이점 분석 가능.

## 6. 참고
- [arxiv:2504.12721](https://arxiv.org/abs/2504.12721)
- [ACM KDD 2025 V.2 Proceedings](https://dl.acm.org/doi/proceedings/10.1145/3711896)
