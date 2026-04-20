---
title: "Abstain Mask Retain Core: Time Series Prediction by Adaptive Masking Loss with Representation Consistency"
authors: Renzhao Liang et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2510.19980
code_url: https://github.com/MazelTovy/AMRC
tags: [loss-function, masking, representation-consistency, architecture-agnostic, plugin]
primary_category: category/SelfSupervised
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-20
pdf_local: ./2025_NeurIPS_AMRC.pdf
pdf_source: arxiv
---

## TL;DR
AMRC (Adaptive Masking Loss with Representation Consistency) proposes a plug-and-play training regularization that suppresses redundant feature learning in time series models. It combines adaptive prefix masking to identify discriminative temporal segments with embedding similarity penalties, achieving consistent improvements across diverse architectures (SOFTS, iTransformer, PatchTST, TSMixer, TimeMixer) without modifying model architecture.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 예측 모델들은 훈련 중 노이즈나 무관한 변동 등 **중복된 특성(redundant features)**을 학습하여 신호 추출 성능을 저하시킴.
- 가설: 과거 시계열 데이터를 적절히 **동적으로 마스킹(truncating)**하면 예측 정확도를 향상시킬 수 있다. 즉, 모든 과거 데이터가 동등하게 유용한 것이 아니라, 일부 고도로 판별력 있는 시간 세그먼트만이 핵심이라는 관찰.

## 2. 제안 방법론

### 핵심 아이디어
**Adaptive Masking Loss (AML)**: 훈련 시점에서 어떤 과거 입력 값이 정보가 풍부한지를 동적으로 식별하고, 그 외의 부분을 마스킹하여 모델이 중복된 특성을 학습하지 않도록 유도.

### 아키텍처 구성요소

1. **Adaptive Masking Loss (AML)**
   - Prefix masking: 입력 시계열의 앞부분(prefix)을 선택적으로 마스킹.
   - 동적 마스킹: 각 배치 및 에포크에서 학습 중인 모델의 상태에 따라 어떤 세그먼트를 마스킹할지 적응적으로 결정.
   - 목표: 모델의 그래디언트 하강이 판별력 높은 시간 세그먼트에만 집중하도록 재정의.

2. **Embedding Similarity Penalty (ESP)** / **Representation Consistency Constraint**
   - 입력과 레이블, 그리고 예측값 간 **임베딩 유사성(embedding similarity)**을 유지하도록 제약.
   - 목표: 표현 공간에서 모델의 매핑 관계(mapping relationships)를 안정화하여 과최적화 방지.

### 아키텍처-독립성
- **플러그-앤-플레이**: 기존 아키텍처를 수정하지 않고, 훈련 단계에서만 손실 함수 추가.
- 테스트 시점에서는 원래 손실 함수 사용 (추론 속도 영향 무).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): L = 48
- Forecast horizons: 48, 72, 96, 120, 144, 168, 192
- Normalization: (논문 명시 미흡, 기반 모델의 기본 설정 따름)
- Train/Val/Test split: 표준 ETT 분할
- Batch size: 32
- Optimizer: Adam with initial learning rate 1×10⁻⁴
- Learning rate scheduler: Cosine annealing
- Loss function: MSE (base) + AML + ESP (regularization)
- Early stopping: patience 20 epochs
- Maximum epochs: 100

### 3.2 Results

#### ETTh1 MSE Results (Table 5 appendix)

| Horizon | SOFTS | SOFTS+AMRC | TimeMixer | TimeMixer+AMRC | iTransformer | iTransformer+AMRC | PatchTST | PatchTST+AMRC | TSMixer | TSMixer+AMRC |
|---------|-------|-----------|-----------|----------------|--------------|-------------------|----------|---------------|---------|-------------|
| **48**  | 0.354 | 0.334     | 0.333     | 0.324          | 0.353        | 0.344             | 0.373    | 0.363         | 0.345   | 0.331       |
| **96**  | 0.394 | 0.377     | 0.376     | 0.372          | 0.401        | 0.393             | 0.411    | 0.397         | 0.389   | 0.376       |
| **192** | 0.450 | 0.427     | 0.439     | 0.435          | 0.458        | 0.449             | 0.468    | 0.455         | 0.446   | 0.427       |

- **Best MSE (horizon 96)**: 0.372 (TimeMixer+AMRC)
- **일관된 개선**: 모든 기반 모델과 모든 예측 길이에서 AMRC 적용 시 MSE 감소 (즉, 개선).
- 출처: Table 5 of paper (appendix).

## 4. 주요 기여
- **손실 함수 설계**: 적응형 마스킹 손실(AML)과 표현 일관성 제약(ESP)을 결합하여 중복된 특성 학습을 억제.
- **아키텍처 독립성**: Transformer, MLP, CNN 등 다양한 기반 아키텍처에 적용 가능한 훈련 시점 플러그-앤-플레이 정규화.
- **일관된 성능 향상**: SOFTS, iTransformer, PatchTST, TSMixer, TimeMixer 등 5개 기저 모델 모두에서 ETTh1 개선.
- **계산 효율성**: 추론 단계에서 추가 비용 없음 (훈련에만 적용).

## 5. 한계 및 후속 연구 아이디어
- 논문에서 마스킹 전략의 시간 복잡도 분석이 제한적.
- ※ 메모: Prefix masking의 최적 마스킹 위치나 비율 선택에 대한 학습 가능한 전략 탐색 가능.
- ※ 메모: 다른 시계열 작업(분류, 이상 탐지)으로의 확장 여부 미명시.

## 6. 참고
- GitHub: https://github.com/MazelTovy/AMRC
- arXiv: https://arxiv.org/abs/2510.19980
- OpenReview: https://openreview.net/forum?id=KrglRiOKYT
- 관련 논문: PSLoss (ICML 2025), SAMformer (ICML 2024), RobustTSF (ICLR 2024)
