---
title: "TimeKAN: KAN-based Frequency Decomposition Learning Architecture for Long-term Time Series Forecasting"
authors: Songtao Huang et al.
venue: ICLR 2025
year: 2025
paper_url: https://openreview.net/forum?id=wTLc79YNbh
code_url: https://github.com/huangst21/TimeKAN
tags: [freq-domain, kan, frequency-decomposition, mlp, lightweight, long-term-forecasting]
primary_category: category/Freq-domain/PeriodicityGuided
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_TimeKAN.pdf
pdf_source: arxiv
---

## TL;DR
TimeKAN은 Kolmogorov-Arnold Networks(KAN)을 시계열 예측에 적용하여 다중 주파수 혼합 문제를 해결한다. Cascaded Frequency Decomposition(CFD)으로 복잡한 주파수를 단일 성분으로 분리하고, Multi-order KAN(M-KAN)으로 주파수별 특성을 학습하며, 매우 적은 파라미터로 SOTA에 필적하는 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 실세계 시계열은 다중 주파수 성분이 혼재 → 균일한 모델링으로 부정확한 특성화 발생
- Transformer 기반 모델은 파라미터가 많고 해석성이 낮음
- KAN 파라미터를 심볼화하여 해석 가능한 시계열 예측 가능하다는 가설

## 2. 제안 방법론
- 핵심 아이디어: Cascaded Frequency Decomposition + KAN 기반 주파수별 표현 학습
- **CFD(Cascaded Frequency Decomposition)**: 혼합 주파수 성분을 단일 주파수로 계단식 분리
- **M-KAN(Multi-order KAN Representation Learning)**: 각 주파수 대역에 다중 차수 KAN으로 특성 학습
- **Frequency Mixing blocks**: 주파수별 표현을 통합
- KAN 파라미터의 심볼화를 통한 해석 가능성 제공
- 손실함수: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96~720 (증가할수록 성능 향상 확인)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity 포함 다수 벤치마크

### 3.2 Results
ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity에서 4개 horizon 평균. PatchTST 대비 파라미터 수 1/295, MACs 1/118 (Electricity 기준). TimeMixer 대비 파라미터 20.05% (Weather 기준). 90% 이상의 신뢰도로 2위 모델 대비 우수.

출처: 논문 Table (ICLR 2025).

## 4. 주요 기여
- KAN을 시계열 예측에 적용한 최초 주요 연구 중 하나
- Cascaded Frequency Decomposition으로 다중 주파수 혼합 문제 해결
- 극도로 경량화된 아키텍처로 SOTA급 성능
- KAN 파라미터 심볼화를 통한 예측 결과 해석 가능성

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: KAN의 훈련 속도가 MLP 대비 느릴 수 있음
- 확장 아이디어: KAN을 다른 TSF 아키텍처(Transformer, Mamba)와 결합

## 6. 참고
- OpenReview: https://openreview.net/forum?id=wTLc79YNbh
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/27844
- GitHub: https://github.com/huangst21/TimeKAN
- 논문 hash: 46362971bfc3a97e6a271f2eb90fba17
