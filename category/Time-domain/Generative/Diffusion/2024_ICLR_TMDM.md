---
title: "Transformer-Modulated Diffusion Models for Probabilistic Multivariate Time Series Forecasting"
authors: Yuxin Li, Wenchao Chen, Xinyue Hu, Bo Chen, Baolin Sun, Mingyuan Zhou
venue: ICLR
year: 2024
paper_url: https://openreview.net/forum?id=qae04YACHs
code_url: ""
tags: [time-domain, diffusion, transformer, probabilistic]
primary_category: category/Time-domain/Generative/Diffusion
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_TMDM.pdf
pdf_source: publisher
---

## TL;DR
Transformer가 추출한 시계열 표현을 **diffusion의 forward/reverse 과정을 modulate**하는 prior로 사용. 다변량 covariate 의존성을 Bayesian 틀에서 통합.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion은 공변량(covariate) 의존성을 암묵적으로만 다룸.
- Transformer의 조건부 표현력을 diffusion과 결합하면 probabilistic forecasting 품질 향상 가능.

## 2. 제안 방법론
- Transformer encoder로 과거 관측 요약 → 이 요약을 forward/reverse process의 parameter modulation에 주입.
- Plug-and-play — 기존 Transformer TSF 모델과 결합 가능.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Probabilistic MTS setting. 상세는 원문 §4 참고.

### 3.2 Results
ETTh1 포함 주요 벤치마크에서 CSDI·TimeGrad 대비 향상 보고. 상세 수치는 원문 Table 참고.

## 4. 주요 기여
- Transformer ↔ Diffusion의 Bayesian 통합 프레임.
- 기존 Transformer 모듈을 재사용 가능한 plug-and-play.

## 5. 참고
- OpenReview: https://openreview.net/forum?id=qae04YACHs
