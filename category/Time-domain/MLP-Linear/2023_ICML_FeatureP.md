---
title: "Feature Programming for Multivariate Time Series Prediction"
authors: Reneau et al.
venue: ICML 2023
year: 2023
paper_url: https://arxiv.org/abs/2306.06252
code_url: ""
tags: [time-domain, mlp, feature-engineering, multivariate, programmable]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICML_FeatureP.pdf
pdf_source: arxiv
---

## TL;DR
Feature Programming은 사용자가 inductive bias를 최소한의 노력으로 반영할 수 있는 프로그래머블 특성 공학 프레임워크를 multivariate 시계열 예측에 도입한다. 시계열을 spin-gas dynamical Ising model로 모델링하여 대량의 예측적 특성을 생성하고, 이를 통해 노이즈가 많은 다변량 시계열에서도 강건한 예측을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 딥러닝 기반 시계열 모델은 end-to-end 학습에 의존해 도메인 지식을 명시적으로 활용하기 어려움
- 노이즈가 많은 다변량 시계열에서 특성 공학 없이 원본 시계열을 입력으로 쓰면 성능 저하
- 사용자가 도메인 지식을 inductive bias로 반영하는 프로그래머블 특성 공학이 예측 성능을 높일 수 있다는 가설

## 2. 제안 방법론
- **핵심 아이디어**: multivariate 시계열을 누적합(cumulative sum of trajectory increments)으로 보고, 각 increment를 spin-gas dynamical Ising model로 표현; 이로부터 다양한 예측적 특성을 프로그래매틱하게 생성
- **아키텍처 구성요소**:
  - **Feature Programmer**: 사용자 지정 규칙(domain knowledge)과 Ising model 기반 stochastic dynamics로 대량의 특성 생성
  - **Feature Selector**: 생성된 특성 중 예측에 유용한 것 선택 (regularized regression 또는 attention 기반)
  - **Predictor**: 선택된 특성을 입력으로 최종 예측 수행 (MLP 또는 linear model)
- **손실함수**: MSE
- **학습 전략**: feature generation → selection → prediction 파이프라인, end-to-end 가능

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 실험 설정 참고
- Forecast horizons: 96, 192, 336, 720 (long-term 표준 벤치마크)
- Normalization: 표준 정규화 적용
- Train/Val/Test split: ETT 표준 분할
- Batch size / Optimizer / LR: 논문 Appendix 참고
- Loss function: MSE
- 기타: Synthetic, Taxi, Electricity, Traffic, ETTh1, ETTh2, ETTm1, ETTm2 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | Ising 기반 특성 공학으로 노이즈 강건성 향상 |
| 192     | see paper | see paper | |
| 336     | see paper | see paper | |
| 720     | see paper | see paper | |

출처: Table 1-2 of paper (proceedings.mlr.press/v202/reneau23a).

## 4. 주요 기여
- Multivariate 시계열 예측에 프로그래머블 특성 공학 프레임워크 최초 도입
- 시계열을 spin-gas dynamical Ising model의 누적합으로 모델링하는 새로운 관점
- 도메인 지식을 명시적으로 반영할 수 있는 사용자 친화적 인터페이스
- 노이즈가 많은 다변량 데이터에서 강건한 예측 성능

## 5. 한계 및 후속 연구 아이디어
- Ising model 기반 특성 생성이 복잡하여 실용성이 제한될 수 있음
- 특성 공간의 크기가 커지면 선택 과정의 계산 비용 증가
- ※ 메모: 도메인 지식이 풍부한 산업 시계열 예측(에너지, 금융)에서 특히 유용할 수 있음

## 6. 참고
- arXiv: https://arxiv.org/abs/2306.06252
- PMLR proceedings: https://proceedings.mlr.press/v202/reneau23a.html
- ※ 원본 PDF 취득: arxiv에서 다운로드 시도 필요
