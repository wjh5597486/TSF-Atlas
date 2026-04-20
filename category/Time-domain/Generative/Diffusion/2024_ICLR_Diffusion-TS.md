---
title: "Diffusion-TS: Interpretable Diffusion for General Time Series Generation"
authors: Xinyu Yuan, Yan Qiao
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2403.01742
code_url: https://github.com/Y-debug-sys/Diffusion-TS
tags: [time-domain, diffusion, generative, decomposition]
primary_category: category/Time-domain/Generative/Diffusion
related_categories:
  - category/CrossCutting/Loss
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_Diffusion-TS.pdf
pdf_source: arxiv
---

## TL;DR
Transformer 기반 encoder-decoder + **추세·계절 disentangled decomposition**을 diffusion의 각 단계 출력에 적용한 해석 가능한 시계열 생성 모델. 조건부 예측·imputation까지 지원.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion-TS 모델은 의미 있는 해석이 어려움 — 단순 노이즈→데이터 복원만 학습.
- 시계열의 내재 구조(추세·계절·잔차)를 명시적으로 보존하면 품질과 해석 가능성이 모두 향상된다.

## 2. 제안 방법론
- Backbone: Transformer encoder-decoder.
- 각 diffusion step에서 출력을 trend/season/residual로 분해.
- **Fourier-based reconstruction loss**로 주파수 구조 보존.

## 3. ETTh1 실험 결과
### 3.1 Setting
- 주 task는 generation/imputation, 일부 조건부 forecasting 실험.
- 상세 setting은 원문 §4 및 Appendix 참고.

### 3.2 Results
ETTh1 상에서 이전 diffusion-TS baselines 대비 향상, 구체 수치는 원문 Table 참고.

## 4. 주요 기여
- Disentangled 시계열 diffusion으로 해석 가능 생성.
- Fourier loss → 고품질·고주파 보존.

## 5. 참고
- 코드: https://github.com/Y-debug-sys/Diffusion-TS
