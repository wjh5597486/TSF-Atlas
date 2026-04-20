---
title: "Self-Interpretable Time Series Prediction with Counterfactual Explanations"
authors: Yan et al.
venue: ICML 2023
year: 2023
paper_url: https://arxiv.org/abs/2306.06024
code_url: ""
tags: [time-domain, transformer, interpretability, counterfactual, variational-bayes]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICML_CounTS.pdf
pdf_source: arxiv
---

## TL;DR
CounTS는 시계열 예측을 위한 self-interpretable 모델로, Variational Bayesian deep learning 기반으로 counterfactual 및 actionable 설명(어떤 과거 구간을 어떻게 바꾸면 미래 예측이 달라지는가)을 생성한다. 예측 정확도를 희생하지 않으면서도 기존 방법 대비 더 나은 counterfactual 설명을 제공한다.

## 1. 기존 모델의 한계 / 가설
- 의료, 자율주행 등 safety-critical 도메인에서 시계열 예측의 해석 가능성이 필수적임
- 기존 해석 방법(attention 가중치, feature importance 등)은 입력의 중요도만 사후적으로 할당하며, 예측 변화를 유도하는 actionable 설명이 불가능함
- Self-interpretable 모델이 counterfactual 설명과 예측 성능을 동시에 달성할 수 있다는 가설

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 counterfactual 설명 문제를 형식화하고, Variational Bayesian deep learning으로 abduction-action-prediction 프레임워크 구현
- **아키텍처 구성요소**:
  - **Encoder**: 관측된 시계열에서 잠재 변수 $z$를 추론 (variational inference)
  - **Counterfactual Generator**: 잠재 공간에서 counterfactual 시나리오 생성 (abduction → action)
  - **Decoder/Predictor**: counterfactual 잠재 변수에서 미래 예측 생성 (prediction)
  - Transformer-based encoder-decoder 구조 위에 variational 레이어 추가
- **손실함수**: ELBO (Evidence Lower BOund) + reconstruction + prediction loss
- **학습 전략**: variational Bayesian inference, reparameterization trick

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 실험 설정 참고
- Forecast horizons: 표준 long-term forecasting 벤치마크 (96, 192, 336, 720)
- Normalization: 표준 normalization 적용
- Train/Val/Test split: ETTh 표준 분할
- Batch size / Optimizer / LR: Adam optimizer, 상세는 논문 참고
- Loss function: ELBO + prediction MSE
- 기타: ETTh, ETTm, Weather, Electricity, Traffic, ILI 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 예측 정확도 comparable to SOTA |
| 192     | see paper | see paper | |
| 336     | see paper | see paper | |
| 720     | see paper | see paper | |

출처: Table 1-2 of paper (proceedings.mlr.press/v202/yan23d); **Oral** presentation at ICML 2023.

## 4. 주요 기여
- 시계열 counterfactual 설명 문제의 최초 형식화 및 평가 프로토콜 수립
- Variational Bayesian deep learning 기반 self-interpretable 예측 모델 제안
- abduction-action-prediction 프레임워크로 actionable counterfactual 설명 생성
- 예측 성능과 설명 가능성을 동시에 달성
- ICML 2023 Oral 채택 (상위 논문)

## 5. 한계 및 후속 연구 아이디어
- Variational inference로 인한 학습 복잡도 증가
- Counterfactual의 현실성(plausibility) 보장이 어려울 수 있음
- ※ 메모: healthcare 시계열 예측에서 실용적 적용 가능성이 높음; counterfactual 생성의 diversity-fidelity trade-off 연구 필요

## 6. 참고
- arXiv: https://arxiv.org/abs/2306.06024
- ICML 2023 oral: https://icml.cc/virtual/2023/oral/25470
- PMLR proceedings: https://proceedings.mlr.press/v202/yan23d.html
- 저자 페이지: http://www.wanghao.in/paper/ICML23_CounTS.pdf
