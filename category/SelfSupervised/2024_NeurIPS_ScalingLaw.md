---
title: "Scaling Law for Time Series Forecasting"
authors: Jingzhe Shi et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2405.15124
code_url: https://github.com/JingzheShi/ScalingLawForTimeSeriesForecasting
tags: [scaling-law, foundation-model, empirical-analysis, time-domain]
primary_category: category/SelfSupervised
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_ScalingLaw.pdf
pdf_source: arxiv
---

## TL;DR
TSF를 위한 스케일링 법칙(Scaling Law)을 최초 제안. 데이터셋 크기, 모델 복잡도, 시계열 데이터 세분성(granularity)이 성능에 미치는 영향을 이론적·경험적으로 분석하며 최적 모델 크기와 데이터 요구량을 예측.

## 1. 기존 모델의 한계 / 가설
- LLM 분야에서 확립된 스케일링 법칙이 TSF에는 적용·검증된 바 없음.
- 시계열의 특성(granularity, 데이터 크기)이 스케일링에 미치는 영향 불명.
- 가설: TSF도 모델 크기-데이터 크기-성능 간 멱함수 스케일링 법칙을 따름.

## 2. 제안 방법론
- **핵심 아이디어**: 데이터셋 크기와 모델 복잡도의 스케일링 효과를 다양한 granularity 조건에서 실험적으로 측정.
- **아키텍처**:
  - 다양한 크기의 Transformer 모델 실험
  - ETTh1, ETTh2, ETTm1, ETTm2, ECL 데이터셋 활용
  - 세분성(분단위~시간단위) 조건 실험
  - Chinchilla-style 최적 계산 예산 분석
- **이론적 기여**: TSF용 스케일링 법칙 수식화

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 다양한 설정 (스케일링 실험)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
ETTh1 포함 스케일링 실험 수행. 상세 결과는 원문 Figure/Table 참고.

## 4. 주요 기여
- TSF 최초 스케일링 법칙 제안 (데이터 크기 × 모델 크기 × granularity)
- 최적 계산 예산 배분 가이드라인 제공
- TSF 파운데이션 모델 설계를 위한 이론적 근거
- ETTh1 포함 다양한 벤치마크에서 스케일링 패턴 검증

## 5. 한계 및 후속 연구 아이디어
- 특정 아키텍처(Transformer)에 의존, 다른 아키텍처로의 일반화 미검토
- 멀티도메인 사전학습 스케일링 미분석

## 6. 참고
- [GitHub](https://github.com/JingzheShi/ScalingLawForTimeSeriesForecasting)
- [arXiv 2405.15124](https://arxiv.org/abs/2405.15124)
