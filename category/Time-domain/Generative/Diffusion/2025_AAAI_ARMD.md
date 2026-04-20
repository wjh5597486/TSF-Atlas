---
title: "Auto-Regressive Moving Diffusion Models for Time Series Forecasting"
authors: Jiaxin Gao et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2412.09328
code_url: https://github.com/daxin007/ARMD
tags: [time-domain, diffusion, generative, auto-regressive, probabilistic]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_ARMD.pdf
pdf_source: arxiv
---

## TL;DR
ARMD는 ARMA 이론에서 영감을 받아 미래 시계열을 초기 상태로, 과거 시계열을 최종 상태로 설정하는 연속 순차 확산 기반 예측 모델이다. Gaussian 노이즈 대신 역사 데이터를 prior로 활용하여 예측 목표와 일치하는 diffusion 프레임워크를 구성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion 기반 TSF 모델은 Gaussian 노이즈를 시작점으로 사용하여 예측 목표와 정렬이 부자연스러움
- 순수 확률적 생성 모델은 결정론적 예측 정확도 면에서 제한적
- ARMA 모델의 자기회귀적 특성을 확산 과정에 통합하지 못함
- 가설: 미래 → 과거 방향의 확산 설계로 예측과 자연스럽게 정렬 가능

## 2. 제안 방법론
- **핵심 아이디어**: 미래 시계열을 initial state로, 과거를 final state로 하는 chain-based diffusion with historical priors
- **아키텍처 구성요소**:
  1. **Forward Process**: 미래 시계열이 점진적으로 역사 시계열로 확산
  2. **Reverse Process**: 역사 시계열을 조건으로 미래 시계열 복원 (denoising)
  3. **Auto-Regressive Prior**: ARMA 이론 기반 자기회귀 구조로 순차적 확산
  4. **Moving Average Component**: 이동 평균으로 추세/노이즈 처리
- **손실함수**: L1 loss (Adam optimizer, lr=1e-3)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 96 (input=output 동일 길이)
- Normalization: 논문 표준 설정
- Train/Val/Test split: 70/10/20 chronological split
- Batch size / Optimizer / LR: batch 128, Adam, lr=1e-3
- Loss function: L1
- 기타: 7개 데이터셋 (Solar Energy, Exchange, ETTh1, ETTh2, ETTm1, ETTm2, Stock)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | Solar Energy, ETTh1, Exchange에서 10%+ 개선 |

출처: Tables 1, 2 of paper.

## 4. 주요 기여
- ARMA 이론 기반의 새로운 확산 방향 설정 (미래→과거)
- 역사 시계열을 diffusion prior로 활용하는 새로운 패러다임
- chain-based sequential diffusion으로 자기회귀 특성 통합
- 7개 데이터셋에서 Solar Energy, ETTh1, Exchange 대비 10% 이상 MSE/MAE 감소

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: prediction 및 history length가 둘 다 96으로 고정되어 다양한 horizon 평가 제한
- ※ 메모: 단일 horizon (96) 평가가 한계. 장기 다중 horizon 평가 필요

## 6. 참고
- [arXiv 2412.09328](https://arxiv.org/abs/2412.09328)
- [GitHub](https://github.com/daxin007/ARMD)
- 비교: TimeGrad, CSDI, TimeDiff 등 기존 diffusion TSF 모델
