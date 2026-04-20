---
title: "SimpleTM: A Simple Baseline for Multivariate Time Series Forecasting"
authors: Hui Chen et al.
venue: ICLR 2025
year: 2025
paper_url: https://openreview.net/forum?id=oANkBaVci5
code_url: https://github.com/vsingh-group/SimpleTM
tags: [time-domain, mlp, transformer, attention, geometric-algebra, channel-independent]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_SimpleTM.pdf
pdf_source: arxiv
---

## TL;DR
SimpleTM은 신호 처리 기반 토크나이제이션과 기하 대수(geometric algebra) 기반 어텐션을 결합한 경량 다변량 시계열 예측 모델이다. 단층 또는 2층 구성에서 대형 transformer 기반 모델과 경쟁적 성능을 보이며, 자원 효율적 강력한 베이스라인을 제공한다.

## 1. 기존 모델의 한계 / 가설
- 대형 pre-trained LLM을 시계열에 적용하는 접근은 도메인 특화 파인튜닝이 필요하고 자원 집약적
- 기존 Transformer 기반 모델은 과도하게 복잡한 구조로 실제 필요 이상의 귀납적 편향 내포
- 신호 처리 관점의 간결한 토크나이제이션이 경쟁적 성능을 달성할 수 있다는 가설

## 2. 제안 방법론
- 핵심 아이디어: 신호 처리 원리 기반 토크나이제이션 + dot-product attention을 넘어선 기하 대수 어텐션
- **토크나이제이션**: 교재 신호 처리 아이디어로 시계열을 토큰화
- **어텐션**: self-attention + geometric algebra operations으로 더 일반적인 상호작용 모델링
- 1~2층으로도 경쟁적 성능 달성 가능한 극도로 경량화된 구조
- 손실함수: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 (96~720)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, PEMS 등 다수 벤치마크

### 3.2 Results
ETTh1 포함 표준 벤치마크에서 대형 transformer와 경쟁적 성능. 단층/2층 모델이 훨씬 큰 모델과 동등한 결과. 상세 수치는 논문 Table 참고.

출처: 논문 Table (ICLR 2025 proceedings).

## 4. 주요 기여
- 신호 처리 원리 + 기하 대수 어텐션을 결합한 단순 효율적 아키텍처
- 단층 모델로 대형 Transformer와 경쟁 가능함을 시연
- 경량 강력한 다변량 TSF 베이스라인 제공
- 자원 효율적 접근의 실효성 입증

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 실제 이종(heterogeneous) 시계열 데이터의 복잡한 특성에 대한 일반화
- 확장 아이디어: 기하 대수 어텐션을 다양한 도메인·태스크에 적용

## 6. 참고
- OpenReview: https://openreview.net/forum?id=oANkBaVci5
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/28373
- GitHub: https://github.com/vsingh-group/SimpleTM
- dblp: https://dblp.org/rec/conf/iclr/ChenLMS25.html
