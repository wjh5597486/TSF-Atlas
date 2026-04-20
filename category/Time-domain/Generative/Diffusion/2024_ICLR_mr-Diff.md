---
title: "Multi-Resolution Diffusion Models for Time Series Forecasting"
authors: Lifeng Shen, Weiyu Chen, James T. Kwok
venue: ICLR
year: 2024
paper_url: https://openreview.net/forum?id=mmjnr0G8ZY
code_url: ""
tags: [time-domain, diffusion, multiscale, decomposition]
primary_category: category/Time-domain/Generative/Diffusion
related_categories:
  - category/Decomposition/TrendSeasonal
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_mr-Diff.pdf
pdf_source: publisher
---

## TL;DR
시계열의 **multi-resolution 추세 구조**를 활용한 diffusion 예측 모델. Seasonal-trend 분해로 coarse→fine 순차 생성(easy-to-hard), non-autoregressive.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion-TS는 단일 해상도에서 noise→data 복원만 수행.
- 시계열은 여러 해상도에 걸쳐 패턴을 가지므로 계층적 denoising이 유리.

## 2. 제안 방법론
- Forward diffusion의 target을 stage별로 **점점 더 coarse한 trend**로 설정.
- Reverse: 가장 coarse trend를 먼저 생성 → 이전 stage의 trend를 condition으로 더 fine 구조 추가.
- Non-autoregressive 샘플링.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Lookback L, forecast H, stage 수 S는 데이터별 튜닝.
- 9개 실세계 데이터셋 중 ETT 계열 포함.

### 3.2 Results
diffusion-TS 베이스라인 대비 개선 보고. 상세는 원문 Table 참고.

## 4. 주요 기여
- 첫 multi-resolution diffusion TSF 프레임워크.
- Easy-to-hard 생성 순서로 샘플 품질 향상.

## 5. 참고
- OpenReview: https://openreview.net/forum?id=mmjnr0G8ZY
