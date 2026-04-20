---
title: "MG-TSD: Multi-Granularity Time Series Diffusion Models with Guided Learning Process"
authors: Xinyao Fan, Yueying Wu, Chang Xu, Yuhao Huang, Weiqing Liu, Jiang Bian
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2403.05751
code_url: https://github.com/Hundredl/MG-TSD
tags: [time-domain, diffusion, multi-granularity, guided]
primary_category: category/Time-domain/Generative/Diffusion
related_categories:
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_MG-TSD.pdf
pdf_source: arxiv
---

## TL;DR
여러 시간 granularity(분/시/일 등)의 coarse-grained 데이터를 diffusion의 **중간 단계 target**으로 사용해 학습을 regularize. 다중 해상도가 자연스럽게 noise schedule에 매핑됨.

## 1. 기존 모델의 한계 / 가설
- 기존 diffusion-TS는 noise→data 경로가 무작위라 학습이 불안정.
- 시계열은 granular 구조를 가지므로 coarse 시점을 중간 감독 신호로 쓰면 학습이 안정화.

## 2. 제안 방법론
- Data의 다양한 granularity 버전을 생성(e.g., 1h → 4h → 1d).
- Diffusion step별 target을 coarse-to-fine으로 지정.
- Guided learning loss.

## 3. ETTh1 실험 결과
### 3.1 Setting
- 데이터셋: Solar, Electricity, Traffic, Taxi, ETT, ... Horizons는 원문 참고.

### 3.2 Results
CRPS·ND 등 probabilistic metric에서 기존 diffusion-TS 대비 향상.

## 4. 주요 기여
- "중간 감독"으로 diffusion 학습 안정화.
- Multi-granularity가 noise schedule과 자연스럽게 연결.

## 5. 참고
- 코드: https://github.com/Hundredl/MG-TSD
