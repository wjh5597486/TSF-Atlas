---
title: "TS-RAG: Retrieval-Augmented Generation based Time Series Foundation Models are Stronger Zero-Shot Forecaster"
authors: Kanghui Ning et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2503.07649
code_url: https://github.com/UConn-DSIS/TS-RAG
tags: [foundation-model, pretrained-native, rag, retrieval-augmented, zero-shot, distribution-shift]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TS-RAG.pdf
pdf_source: arxiv
---

## TL;DR
TS-RAG은 검색 증강 생성(RAG) 기법을 시계열 Foundation Model에 적용한 프레임워크다. 사전학습된 시계열 인코더로 지식 베이스에서 의미론적으로 유사한 세그먼트를 검색하고, Adaptive Retrieval Mixer(ARM)로 검색된 패턴을 내부 표현에 동적으로 융합하여 제로샷 예측 성능을 최대 6.84% 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- 기존 Time Series Foundation Model들은 비정상성(non-stationarity)과 분포 이동 처리에 어려움.
- 태스크별 파인튜닝 없이는 다양한 도메인에 적응하기 어려움.
- 가설: RAG 기법으로 유사 시계열 패턴을 검색하여 컨텍스트를 보강하면 파인튜닝 없이도 더 강한 일반화 성능을 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: RAG 패러다임을 시계열에 적용. 유사 패턴 검색 + Adaptive Retrieval Mixer.
- **아키텍처**:
  - **Time Series Foundation Model**: 사전학습된 TSFM 기반.
  - **Retriever**: 사전학습된 시계열 인코더로 지식 베이스에서 top-k 유사 컨텍스트 검색.
  - **Adaptive Retrieval Mixer (ARM)**: 검색된 패턴과 TSFM 내부 표현을 동적으로 융합. 태스크별 파인튜닝 없이 적용.
- **평가**: 제로샷 예측 성능 최대 6.84% 향상.
- **손실함수**: 기존 TSFM 손실 유지.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 사전학습 모델별 설정 따름
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 split
- Batch size / Optimizer / LR: 사전학습 모델 설정 따름
- Loss function: 기존 TSFM 손실
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가; 평균 MSE 0.1940 달성

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 평균 MSE 0.1940, RAF(0.2320) 대비 개선; 구체 수치 논문 참조.

## 4. 주요 기여
- RAG 패러다임을 시계열 Foundation Model에 최초 적용.
- Adaptive Retrieval Mixer(ARM)로 검색 패턴의 동적 융합.
- 태스크별 파인튜닝 없이 제로샷 예측 성능 최대 6.84% 향상.
- 지식 베이스 기반 컨텍스트 보강으로 비정상성·분포 이동 처리 개선.

## 5. 한계 및 후속 연구 아이디어
- 지식 베이스 구성 및 유지 비용.
- 검색 품질이 임베딩 인코더 성능에 의존.

## 6. 참고
- arxiv: https://arxiv.org/abs/2503.07649
- GitHub: https://github.com/UConn-DSIS/TS-RAG
- OpenReview: https://openreview.net/forum?id=TJuUelhGQr
