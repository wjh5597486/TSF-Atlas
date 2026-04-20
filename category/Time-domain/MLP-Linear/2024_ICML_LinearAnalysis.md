---
title: "An Analysis of Linear Time Series Forecasting Models"
authors: William Toner et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2403.14587
code_url: ""
tags: [linear, mlp, analysis, normalization, time-domain, theoretical]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_LinearAnalysis.pdf
pdf_source: arxiv
---

## TL;DR
DLinear, NLinear, RLinear, FITS 등 선형 TSF 모델의 표현 공간을 이론적으로 분석한 논문. 다양한 선형 모델 변형이 사실상 동일한 함수 집합을 표현하며, 피처 정규화가 핵심 일반화 역할을 한다는 것을 밝힘.

## 1. 기존 모델의 한계 / 가설
- 선형 모델이 LTSF에서 놀라운 성능을 보이는 이유에 대한 이론적 이해 부족.
- 다양한 선형 모델 변형이 왜 유사한 성능을 보이는지 설명 미비.
- 가설: 다양한 선형 모델들이 표현 가능한 함수 집합이 동일하거나 매우 유사함.

## 2. 제안 방법론
- **핵심 아이디어**: 선형 TSF 모델의 표현력 이론적 분석 + ETTh1 등에서 가중치 행렬 비교.
- **아키텍처 (분석 대상)**:
  - RLinear: RevIN + Linear
  - NLinear: 입력 정규화 + Linear
  - DLinear: 추세-계절 분해 + Linear
  - FITS+IN: 주파수 보간 + 인스턴스 정규화
- **분석 방법**:
  - 각 모델의 표현 가능 함수 집합 형식화
  - ETTh1 (lookback=720, horizon=336)에서 학습된 가중치 행렬 시각화
  - 모델 간 가중치 행렬의 유사성 분석

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: 336
- Normalization: 모델별 다름 (RevIN, IN 등)
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
논문 특성상 새 SOTA 달성이 아닌 기존 모델 분석이 목적. ETTh1에서 RLinear/NLinear/DLinear+IN/FITS+IN의 학습된 가중치 행렬이 거의 동일함을 시각적으로 확인.

## 4. 주요 기여
- 선형 TSF 모델의 함수 표현 공간 이론적 분석
- RLinear ≈ NLinear ≈ DLinear+IN ≈ FITS+IN 수학적 증명
- 피처 정규화가 선형 모델 일반화의 핵심임을 규명
- ETTh1에서 가중치 시각화를 통한 실증적 검증

## 5. 한계 및 후속 연구 아이디어
- 비선형 모델로의 분석 확장 필요
- 정규화 선택이 실제 적용에서 항상 명확하지 않음

## 6. 참고
- [arXiv 2403.14587](https://arxiv.org/abs/2403.14587)
