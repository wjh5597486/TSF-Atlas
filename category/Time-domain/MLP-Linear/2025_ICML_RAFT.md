---
title: "Retrieval Augmented Time Series Forecasting"
authors: Sungwon Han et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2505.04163
code_url: https://github.com/archon159/RAFT
tags: [time-domain, mlp, retrieval-augmented, periodicity, multi-scale, multivariate-forecasting]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_RAFT.pdf
pdf_source: arxiv
---

## TL;DR
RAFT (Retrieval Augmented Time Series Forecasting) retrieves historical patterns similar to the current input from the training set and uses the future values of those retrieved patterns to augment the forecasting model. By downsampling at different periods to capture multi-scale patterns, it achieves an 86% average win ratio across 10 benchmark datasets.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 패턴을 모델 가중치에 암묵적으로 저장하여 추론 시 활용하기 어려움.
- 훈련 데이터에서 현재 입력과 유사한 과거 패턴을 명시적으로 검색(retrieve)하여 활용하면 학습 부담을 줄이고 일반화 성능을 향상시킬 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 훈련 데이터셋에서 현재 입력과 유사한 패턴을 검색하고, 해당 패턴의 미래값을 예측 보조 정보로 활용.
- **아키텍처**:
  - **Multi-scale Downsampling**: 다양한 주기로 입력 시리즈를 다운샘플링하여 다중 스케일 패턴 포착.
  - **Retrieval Module**: 각 스케일에서 유사 패턴 검색.
  - **Forecasting Module**: 검색된 패턴의 미래값과 원본 입력을 결합하여 예측.
  - 2가지 장점: (1) 모델 가중치 외 과거 패턴 직접 활용, (2) 일반화 성능 향상.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 10개 벤치마크 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic 등 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 86% avg win ratio |
| 192     | see paper | see paper | 86% avg win ratio |
| 336     | see paper | see paper | 86% avg win ratio |
| 720     | see paper | see paper | 86% avg win ratio |

출처: PMLR v267 (https://proceedings.mlr.press/v267/han25d.html).

## 4. 주요 기여
- Retrieval-augmented 접근으로 모델 가중치 외 외부 메모리 활용.
- Multi-scale downsampling으로 단기/장기 패턴 동시 포착.
- 10개 데이터셋에서 86% 평균 win ratio 달성.
- 모델 학습 부담 감소 및 일반화 성능 향상.

## 5. 한계 및 후속 연구 아이디어
- 검색 비용(retrieval overhead)이 추론 속도에 영향을 줄 수 있음.
- 훈련 데이터가 적은 경우(few-shot) 검색 효과 제한.
- ※ 메모: In-Context Fine-Tuning(ICML 2025)의 검색 기반 접근과 상보적.

## 6. 참고
- 관련 논문: In-Context Fine-Tuning (ICML 2025), TimesFM (ICML 2024), MOIRAI (ICML 2024)
- GitHub: https://github.com/archon159/RAFT
- arXiv: https://arxiv.org/abs/2505.04163
- PMLR: https://proceedings.mlr.press/v267/han25d.html
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/45826
