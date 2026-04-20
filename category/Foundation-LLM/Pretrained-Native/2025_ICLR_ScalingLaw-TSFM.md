---
title: "Towards Neural Scaling Laws for Time Series Foundation Models"
authors: Qingren Yao et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2410.12360
code_url: https://github.com/Qingrenn/TSFM-ScalingLaws
tags: [foundation-model, pretrained, scaling-law, transformer, encoder, decoder, zero-shot]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_ScalingLaw-TSFM.pdf
pdf_source: arxiv
---

## TL;DR
시계열 파운데이션 모델(TSFM)에 대한 neural scaling law를 체계적으로 분석한 실증 연구. Encoder-only vs Decoder-only transformer의 in-distribution(ID) 및 out-of-distribution(OOD) 스케일링 행동을 비교하고, TSFM 설계를 위한 실용적 가이드라인을 제시한다.

## 1. 기존 모델의 한계 / 가설
- LLM의 스케일링 법칙이 시계열에도 적용되는지 불분명
- Encoder-only vs Decoder-only 아키텍처의 스케일링 특성 비교 부재
- 아키텍처 개선(귀납적 편향 추가)이 OOD 스케일링에 미치는 영향 미파악

## 2. 제안 방법론
- 핵심 아이디어: TSFM에서의 스케일링 행동 체계적 조사
- **실험 설계**: 파라미터 수, 계산 예산, 데이터셋 크기 변화에 따른 성능 측정
- **아키텍처 비교**: encoder-only vs decoder-only transformer의 ID·OOD 스케일링
- 주요 발견: encoder-only가 decoder-only보다 OOD 스케일링 우수; 아키텍처 개선이 OOD 스케일링 감소시킬 수 있음
- 손실함수: log-likelihood loss가 ID·OOD 모두 scaling behavior 보임

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 다양 (스케일링 실험)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할 포함
- Loss function: MSE / log-likelihood
- 기타: ETT 데이터셋 포함 다수 벤치마크

### 3.2 Results
스케일링 법칙 분석이 주요 기여. ETTh1 포함 다양한 데이터셋에서 스케일 조건별 성능 추이 제시. 상세 수치는 논문 참고.

출처: 논문 Figures and Tables.

## 4. 주요 기여
- TSFM에서의 neural scaling law 최초 체계적 실증 분석
- Encoder-only가 decoder-only 대비 더 나은 OOD 스케일링 시사
- 고급 귀납적 편향이 OOD 스케일링을 저해할 수 있음 발견
- TSFM 설계·스케일링을 위한 실용적 가이드라인 제공

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 특정 아키텍처/데이터셋에 대한 분석으로 범용성 제한 가능
- 확장 아이디어: MoE, SSM 등 다양한 아키텍처로 스케일링 법칙 확장

## 6. 참고
- arXiv: https://arxiv.org/abs/2410.12360
- OpenReview: https://openreview.net/forum?id=uCqxDfLYrB
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/27992
- GitHub: https://github.com/Qingrenn/TSFM-ScalingLaws
- NVIDIA Research: https://research.nvidia.com/publication/2025-04_towards-neural-scaling-laws-time-series-foundation-models
