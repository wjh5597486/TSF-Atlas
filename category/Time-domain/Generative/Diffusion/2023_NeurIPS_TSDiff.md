---
title: "Predict, Refine, Synthesize: Self-Guiding Diffusion Models for Probabilistic Time Series Forecasting"
authors: Marcel Kollovieh et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2307.11494
code_url: https://github.com/amazon-science/unconditional-time-series-diffusion
tags: [diffusion, generative, probabilistic, forecasting, self-guidance, time-domain, unconditional]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_TSDiff.pdf
pdf_source: arxiv
---

## TL;DR
TSDiff는 무조건(unconditional) 방식으로 훈련된 확산 모델로, 자기 유도(self-guidance) 메커니즘을 통해 추론 시 조건부 예측·정제·합성의 세 가지 태스크를 수행한다. 별도의 보조 네트워크 없이 학습된 암묵적 확률 밀도를 활용해 기반 예측기를 반복적으로 정제할 수 있다.

## 1. 기존 모델의 한계 / 가설
- 기존 조건부 확산 모델은 각 태스크별로 조건부 훈련을 요구하여 범용성이 낮다.
- 훈련 후 새로운 조건에 적응하려면 재훈련이 필요하다.
- 가설: 무조건 확산 모델의 학습된 확률 밀도를 활용한 자기 유도로 다양한 태스크에 적응할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 무조건 확산 모델을 훈련한 후, 추론 시 관측값을 안내 신호로 활용하는 자기 유도 메커니즘으로 예측·정제·합성 수행.
- **아키텍처**:
  1. **무조건 훈련**: 표준 DDPM 방식으로 시계열 확산 모델 훈련.
  2. **자기 유도 (Predict)**: 역 확산 과정에서 관측된 과거값으로 미래를 안내.
  3. **정제 (Refine)**: 외부 예측기 출력을 확산 모델로 반복 정제.
  4. **합성 (Synthesize)**: 확산 모델로 학습 데이터 증강.
- **손실함수**: 표준 DDPM 손실.

## 3. ETTh1 실험 결과
> 본 논문은 주로 확률적 예측 태스크(CRPS, NRMSE 등)와 Exchange, Solar, Electricity, Traffic 데이터셋을 사용. ETTh1 표준 결정론적 벤치마크 결과를 직접 보고하지 않음.

## 4. 주요 기여
- 무조건 확산 모델 하나로 예측·정제·합성 세 가지 태스크 수행.
- 추론 시 자기 유도 메커니즘으로 보조 네트워크 불필요.
- 합성 데이터로 기반 예측기 훈련 시 성능 향상 실증.
- 확률적 예측에서 경쟁력 있는 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 역 확산의 반복 추론으로 인한 계산 비용.
- ※ 메모: Diffusion-TS(ICLR 2024), mr-Diff(ICLR 2024)와 함께 생성 기반 시계열 예측의 주요 흐름. 결정론적 벤치마크(ETT 등) 비교가 어렵다는 한계.

## 6. 참고
- arXiv: https://arxiv.org/abs/2307.11494
- GitHub: https://github.com/amazon-science/unconditional-time-series-diffusion
- 관련: Diffusion-TS (ICLR 2024), mr-Diff (ICLR 2024), CSDI
