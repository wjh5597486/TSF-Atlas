---
title: "A Physics-Informed Koopman Network for Time Series Prediction of Dynamical Systems"
authors: N. Benjamin Erichson, et al.
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2402.14966
code_url: ""
tags: [koopman, operator, physics-informed]
primary_category: category/Decomposition/Operator
related_categories:
  - category/CrossCutting/Loss
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_PIKAN.pdf
pdf_source: arxiv
---

## TL;DR
Koopman operator 이론과 **physics-informed 제약**(보존 법칙 등)을 결합한 신경망으로 동역학계 시계열을 예측. 해석 가능한 선형 latent dynamics를 학습.

## 1. 기존 모델의 한계 / 가설
- 블랙박스 시계열 모델은 해석 가능성·외삽(extrapolation)에 취약.
- Koopman linearization + 물리 제약으로 안정적 장기 예측 가능.

## 2. 제안 방법론
- Encoder → Koopman latent → linear dynamics → decoder.
- 물리 보존 법칙을 loss로 부여(physics-informed regularization).
- Automatic differentiation으로 constraint 미분.

## 3. ETTh1 실험 결과
본 논문은 주로 **동역학계(Pendulum, Lorenz 등)** 를 다루며 ETTh1 결과는 보고하지 않음. (`reports_etth1: false`)

## 4. 주요 기여
- Koopman 기반 해석 가능 장기 예측.
- 물리 제약 통합으로 안정성 강화.

## 5. 참고
- arXiv: https://arxiv.org/abs/2402.14966
- ※ ETTh1 미보고 — 본 저장소 수집 기준상 참고용.
