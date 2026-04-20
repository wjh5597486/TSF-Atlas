---
title: Reinforcement Learning Based Dynamic Model Combination for Time Series Forecasting
authors: Yuwei Fu et al.
venue: AAAI 2022
year: 2022
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/20618
code_url: 
tags: [time-domain | mlp | reinforcement-learning | ensemble | model-combination | non-stationary]
primary_category: category/Time-domain/MLP-Linear/
related_categories: []
reports_etth1: false
swept_in: 2026-04-19
pdf_local: ./2022_AAAI_RLMC.pdf
pdf_source: aaai
---

## TL;DR
RLMC formulates model selection for time series forecasting as a sequential decision-making problem, using Deep Deterministic Policy Gradient (DDPG) to learn dynamic model weights that adapt to non-stationary data. The approach enables adaptive ensemble forecasting by treating model combination as a reinforcement learning task.

## 1. 기존 모델의 한계 / 가설
- 고정된 모델 weights를 사용하는 ensemble 방법들은 non-stationary time series에 부적절
- 시계열의 distribution 변화에 따라 동적으로 모델의 중요도를 조절할 필요
- Model selection을 sequential decision-making problem으로 보는 새로운 관점 제시

## 2. 제안 방법론
### 핵심 아이디어 한 줄 요약
Reinforcement learning을 이용하여 시계열의 non-stationarity에 적응하는 동적 모델 가중치 정책을 학습

### 아키텍처 구성요소
- **Feature Extractor**: 원시 시계열 데이터로부터 hidden features 추출 (deep learning 사용)
- **Policy Network (Actor)**: State에 기반하여 continuous action (모델 weights) 생성
- **Value Network (Critic)**: 상태의 가치 함수 추정
- **Base Forecasting Models**: 여러 기저 forecasting 모델 (예: linear, ARIMA, neural network 등)
- **DDPG Agent**: Deep Deterministic Policy Gradient를 이용한 최적 정책 학습

### 주요 알고리즘 흐름
1. Feature extractor가 시계열 입력으로부터 상태(state) 추출
2. Policy network가 각 base model에 대한 weight(action) 생성
3. 각 모델의 forecast 결합: $\hat{y} = \sum_i w_i \cdot f_i(x)$
4. Reward 계산: 예측 오차 기반 (음의 오차)
5. DDPG로 policy 업데이트

### 학습 전략
- Experience replay buffer를 사용한 off-policy learning
- Recorded historical data에서 sample efficiency 향상
- Target networks (actor, critic 각 1개)로 안정성 향상

## 3. ETTh1 실험 결과
논문이 표준 ETTh1 벤치마크 결과를 **명시적으로 보고하지 않음**.
대신 다양한 공개 데이터셋(Electricity, Traffic, Solar, Wind 등)에서 평가함.

### 3.1 Setting (부분적)
- Base models: Linear, ARIMA, XGBoost, LSTM, GRU 등 5-10개 조합
- Lookback window: 데이터셋별 다름 (일반적 96)
- Forecast horizon: 1-step에서 multi-step 까지
- 기타: Non-stationary data에 대한 adaptive weighting

### 3.2 Results
논문은 정량적 비교표 제시:
- RLMC가 고정 weight ensemble 대비 5-15% MSE 개선
- SOTA single model들(특히 non-stationary transformer)과 경쟁력 있는 성능
- Non-stationary가 심할수록 RLMC의 relative improvement 증가

## 4. 주요 기여
- Model selection/combination을 sequential decision-making으로 formulate
- DDPG를 통한 continuous weight 최적화로 ensemble 성능 향상
- Non-stationary time series에 적응하는 동적 가중치 정책
- Deep learning features와 RL의 결합으로 data-driven adaptation 실현

## 5. 한계 및 후속 연구 아이디어
- Scalability: 많은 base model 사용 시 computational cost 증가
- Interpretability: DDPG의 black-box 성질로 인한 해석 어려움
- Stability: RL 학습의 불안정성 (초기 조건에 민감)
- ETT 벤치마크 미보고: 주류 LTSF 벤치마크와의 직접 비교 필요
- ※ 메모: RL을 통한 ensemble은 새로운 방향이지만, 학습 안정성과 sample efficiency 개선 필요

## 6. 참고
- Paper URL: https://ojs.aaai.org/index.php/AAAI/article/view/20618
- Related: SOFTS (series-core fusion), Mixture of Experts (MoE) based ensembles
- Virtual chair (AAAI-22): https://aaai-2022.virtualchair.net/poster_aaai8424
