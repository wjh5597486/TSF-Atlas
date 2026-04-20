---
title: "SEMPO: Lightweight Foundation Models for Time Series Forecasting"
authors: Hui He et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2510.19710
code_url: https://github.com/mala-lab/SEMPO
tags: [foundation-model, pretrained-native, freq-domain, transformer, mixture-of-prompts, spectral-decomposition]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_SEMPO.pdf
pdf_source: arxiv
---

## TL;DR
SEMPO는 에너지 기반 스펙트럼 분해 모듈(고에너지·저에너지 주파수 신호 모두 캡처)과 Mixture-of-Prompts Transformer로 구성된 경량 시계열 Foundation Model이다. 소규모 사전학습 데이터로도 강한 일반화 성능을 달성하며, 기존 대규모 Foundation Model 대비 모델 크기와 사전학습 데이터를 대폭 줄인다.

## 1. 기존 모델의 한계 / 가설
- 기존 Foundation Model(Chronos, TimesFM 등)은 대규모 사전학습 데이터와 모델 크기가 필요하여 자원 소모가 큼.
- 기존 모델은 고에너지 주파수 성분만 집중하거나, 다양한 데이터셋 적응이 비효율적.
- 가설: 스펙트럼 분해를 통해 모든 에너지 대역의 주파수 정보를 활용하고, 경량 프롬프트 믹스처로 도메인 적응을 효율화하면 소규모 데이터로도 강한 일반화가 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 에너지 기반 스펙트럼 분해 + Mixture-of-Prompts Transformer.
- **아키텍처**:
  - **Energy-Aware Spectral Decomposition**: 시계열을 고에너지·저에너지 주파수 성분으로 분해하여 주파수 대역 전체에서 특징 추출.
  - **Mixture-of-Prompts (MoP) Transformer**: 파라미터 효율적 기술(PEFT 유사)을 이용해 서로 다른 데이터셋에 적응. 프롬프트 혼합으로 도메인 지식 내재화.
  - 전체 모델은 경량 설계로 기존 Foundation Model 대비 훨씬 작은 사전학습 데이터 필요.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체적 수치는 논문 PDF 참조 필요.

## 4. 주요 기여
- 에너지 기반 스펙트럼 분해로 고에너지·저에너지 주파수 모두 활용.
- Mixture-of-Prompts Transformer로 파라미터 효율적 다중 데이터셋 적응.
- 기존 Foundation Model 대비 훨씬 소규모 사전학습 데이터로 강한 일반화.
- 경량 모델 크기로 실용성 향상.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 사전학습 데이터 도메인에 따른 일반화 한계 존재.
- 확장 아이디어: 더 다양한 도메인(medical, finance)으로의 확장.

## 6. 참고
- arxiv: https://arxiv.org/abs/2510.19710
- GitHub: https://github.com/mala-lab/SEMPO
- OpenReview: https://openreview.net/forum?id=YngHXbJM8g
