---
title: "Timer-XL: Long-Context Transformers for Unified Time Series Forecasting"
authors: Yong Liu et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2410.04803
code_url: https://github.com/thuml/Timer-XL
tags: [foundation-model, pretrained, transformer, decoder-only, causal, multivariate, unified, zero-shot]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_Timer-XL.pdf
pdf_source: arxiv
---

## TL;DR
Timer-XL은 causal transformer를 기반으로 한 unified 시계열 예측 파운데이션 모델로, multivariate next-token prediction을 통해 단변량·다변량·공변량 포함 예측을 단일 프레임워크로 처리한다. TimeAttention 메커니즘으로 세밀한 intra/inter-series 의존성을 포착하며 supervised 및 zero-shot 예측 모두에서 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 파운데이션 모델(Timer, MOIRAI 등)은 단변량 예측에 초점 → 다변량 상관관계 포착 한계
- Task-specific 모델과 파운데이션 모델 간 성능 격차 존재
- Long-context(긴 룩백 윈도우) 효율적 처리 방법 부족

## 2. 제안 방법론
- 핵심 아이디어: 1D next-token prediction을 다변량 next-token prediction으로 일반화
- **TimeAttention**: flattened time series token(patch)의 intra-series 및 inter-series 의존성을 세밀하게 포착
- **Position embedding**: temporal causality + variable equivalence 동시 인코딩
- Decoder-only causal transformer 구조로 varying-length context 지원
- 손실함수: Autoregressive MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 최대 4096 타임스텝
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 부록 참고
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic 등 표준 벤치마크

### 3.2 Results
ETTh1 및 다변량 표준 벤치마크에서 task-specific supervised 모델과 동급 또는 상회하는 성능. zero-shot 세팅에서도 SOTA. 상세 수치는 논문 Table 참고.

출처: 논문 Table (proceedings.iclr.cc Timer-XL).

## 4. 주요 기여
- 단변량→다변량 next-token prediction 일반화 via TimeAttention
- Causal transformer로 unified forecasting(단변량·다변량·공변량 포함)
- Large-scale pre-training 기반 zero-shot SOTA
- Task-specific 모델과 파운데이션 모델 간 성능 격차 해소

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: pre-training 데이터 규모 및 도메인 다양성에 의존
- 확장 아이디어: 온라인 파인튜닝 및 도메인 적응 전략

## 6. 참고
- arXiv: https://arxiv.org/abs/2410.04803
- OpenReview: https://openreview.net/forum?id=KMCJXjlDDr
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/30062
- GitHub: https://github.com/thuml/Timer-XL
