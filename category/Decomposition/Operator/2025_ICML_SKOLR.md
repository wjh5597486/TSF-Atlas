---
title: "SKOLR: Structured Koopman Operator Linear RNN for Time-Series Forecasting"
authors: Yisen Zhang et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2506.14113
code_url: https://github.com/networkslab/SKOLR
tags: [time-domain, ssm, koopman, linear-rnn, structured, operator, long-term-forecasting]
primary_category: category/Decomposition/Operator
related_categories: [category/Time-domain/SSM-Mamba]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_SKOLR.pdf
pdf_source: arxiv
---

## TL;DR
SKOLR establishes a theoretical connection between Koopman operator approximation and linear RNNs, showing that a structured Koopman operator is equivalent to a linear RNN with lagged observations. It integrates learnable spectral decomposition with MLP measurement functions, achieving exceptional LTSF performance across 8 benchmarks including ILI.

## 1. 기존 모델의 한계 / 가설
- 기존 Koopman 기반 방법(Koopa 등)은 Koopman operator를 이산적으로 근사하여 연속적 동역학을 충분히 포착하지 못함.
- Linear RNN(Mamba, S4 등)과 Koopman operator 사이의 이론적 연결이 명확하지 않음.
- 시차(lagged) 관측값을 확장 상태로 고려하면 structured Koopman operator와 linear RNN 업데이트 간의 등가성이 성립한다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Koopman operator 근사와 linear RNN을 통합하여 이론적 연결을 확립하고 효율적 구현 제공.
- **아키텍처**:
  - **Learnable Spectral Decomposition**: 입력 신호의 스펙트럼 분해를 학습.
  - **MLP Measurement Functions**: 측정 함수로 MLP 사용하여 Koopman 임베딩.
  - **Structured Koopman Operator**: Highly parallel linear RNN stack으로 구현.
  - 선형 복잡도 달성.
- **손실함수**: MSE.
- **학습 전략**: Supervised LTSF.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (표준)
- Forecast horizons: 96, 192, 336, 720 (ETTh); 24, 36, 48, 60 (ILI)
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, ILI (8개 데이터셋)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | exceptional |
| 192     | see paper | see paper | exceptional |
| 336     | see paper | see paper | exceptional |
| 720     | see paper | see paper | exceptional |

출처: PMLR v267 (https://proceedings.mlr.press/v267/zhang25be.html).

## 4. 주요 기여
- Koopman operator ↔ linear RNN 이론적 등가성 확립.
- Learnable spectral decomposition + MLP measurement functions 통합.
- 8개 벤치마크에서 exceptional LTSF 성능.
- Highly parallel 구조로 선형 복잡도 달성.

## 5. 한계 및 후속 연구 아이디어
- Koopman 이론의 전역적 선형화 가정이 강한 비선형 시스템에서 제한적.
- ※ 메모: Koopa(NeurIPS 2023)와 Mamba의 교차점.

## 6. 참고
- 관련 논문: Koopa (NeurIPS 2023), S4 (ICLR 2022), Mamba (arxiv 2312)
- GitHub: https://github.com/networkslab/SKOLR
- arXiv: https://arxiv.org/abs/2506.14113
- PMLR: https://proceedings.mlr.press/v267/zhang25be.html
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44949
