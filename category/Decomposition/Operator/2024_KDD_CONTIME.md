---
title: "Addressing Prediction Delays in Time Series Forecasting: A Continuous GRU Approach with Derivative Regularization"
authors: Sheoyon Jhin et al.
venue: KDD 2024
year: 2024
paper_url: https://arxiv.org/abs/2407.01622
code_url: https://github.com/sheoyon-jhin/CONTIME
tags: [gru, ode, neural-ode, continuous-time, recurrent, time-domain, prediction-delay]
primary_category: category/Decomposition/Operator
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_KDD_CONTIME.pdf
pdf_source: arxiv
---

## TL;DR
CONTIME은 시계열 예측에서 발생하는 "예측 지연(prediction delay)" 문제를 해결하는 Neural ODE 기반 Continuous-time 양방향 GRU 모델이다. 시간 미분 정규화(derivative regularization)를 도입하여 ground-truth가 예측을 선행하는 현상을 억제한다.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 예측 모델들은 MSE 최소화에 초점을 맞추지만, 예측 결과가 실제 신호보다 시간적으로 뒤처지는 "prediction delay" 현상이 발생함.
- 이 현상은 금융·기상 예측 등 시간적 정확성이 중요한 분야에서 심각한 문제.
- 연속 시간 미분 방정식을 활용하면 이 편향을 억제할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Continuous ODE-based Bidirectional GRU + 시간 미분 정규화 손실.
- **아키텍처 구성요소**:
  - Neural ODE 기반 continuous-time GRU: 이산 시간 대신 연속 시간 궤적으로 숨겨진 상태 표현
  - Bidirectional 구조: 시간적 전후 정보 모두 활용
  - Derivative Regularization: 예측과 실제 값 사이의 시간적 정렬을 유도하는 미분 항 손실
- **손실함수**: $L = L_{MSE} + \lambda \cdot L_{deriv}$, 여기서 $L_{deriv}$는 미분 정규화 항
- **학습 전략**: Neural ODE solver로 연속 상태 궤적 적분

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE + Derivative Regularization

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 ETTh1, ETTh2에서 PatchTST, DLinear 대비 MSE가 유사하거나 약간 우수하지만, timing 정확도(TDI), shape 정확도(DTW) 지표에서 모든 baseline 대비 일관적으로 우수한 성능 보고.

출처: 원문 Table 참고.

## 4. 주요 기여
- Prediction delay 문제를 TSF 분야에서 명확히 정의하고 측정 지표(TDI, DTW) 제시
- Neural ODE 기반 continuous-time GRU 아키텍처 제안
- 시간 미분 정규화 손실로 timing accuracy 개선
- 금융/기상 등 시간 정렬이 중요한 응용에서의 유효성 실증

## 5. 한계 및 후속 연구 아이디어
- Neural ODE solver로 인한 훈련/추론 비용 증가
- MSE 기준 성능이 가장 최신 SOTA에 비해 경쟁적이지 않을 수 있음
- ※ 메모: Prediction delay 지표가 의료/금융 등 latency 민감 도메인에서의 모델 비교에 유용

## 6. 참고
- 관련 논문: ContiFormer (Neural ODE for TSF), Latent ODE (NeurIPS 2019)
- ACM DL: https://dl.acm.org/doi/10.1145/3637528.3671969
- GitHub: https://github.com/sheoyon-jhin/CONTIME
