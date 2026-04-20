---
title: "Time-VLM: Exploring Multimodal Vision-Language Models for Augmented Time Series Forecasting"
authors: (see GitHub: CityMind-Lab)
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2502.04395
code_url: https://github.com/CityMind-Lab/ICML25-TimeVLM
tags: [foundation-model, vlm, multimodal, reprogramming, vision-language, few-shot, zero-shot]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimeVLM.pdf
pdf_source: arxiv
---

## TL;DR
Time-VLM leverages pre-trained Vision-Language Models (VLMs: CLIP, BLIP2, ViLT) for time series forecasting through three augmented learners: Retrieval-Augmented (temporal features via memory bank), Vision-Augmented (time series as informative images), and Text-Augmented (contextual descriptions). It shows superior performance especially in few-shot and zero-shot settings.

## 1. 기존 모델의 한계 / 가설
- LLM 기반 TSF(Time-LLM, GPT4TS 등)는 텍스트 모달리티만 활용하여 시각적 정보를 무시함.
- VLM의 시각-언어 표현 능력을 시계열 예측에 활용하면 더 풍부한 멀티모달 컨텍스트 제공 가능.
- Few-shot/zero-shot 설정에서 VLM의 강력한 사전지식이 특히 유용하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 세 가지 증강 학습기를 통해 VLM의 시각-언어 능력을 시계열 예측에 통합.
- **아키텍처**:
  - **Retrieval-Augmented Learner**: 메모리 뱅크 상호작용을 통해 풍부한 시간적 특징 추출.
  - **Vision-Augmented Learner**: 시계열을 정보가 풍부한 이미지로 인코딩 (그래프/플롯).
  - **Text-Augmented Learner**: 시계열에 대한 문맥적 텍스트 설명 생성.
  - VLM 백본: CLIP, BLIP2, ViLT 지원.
- **손실함수**: MSE.
- **학습 전략**: Fine-tuning with adapter.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (표준)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh, ETTm, Electricity, Traffic, Weather 포함

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | superior (especially few-shot) |
| 192     | see paper | see paper | superior (especially few-shot) |
| 336     | see paper | see paper | superior (especially few-shot) |
| 720     | see paper | see paper | superior (especially few-shot) |

출처: Table of paper.

## 4. 주요 기여
- VLM(CLIP, BLIP2, ViLT)을 시계열 예측에 최초 적용.
- 세 가지 증강 학습기(Retrieval/Vision/Text-Augmented)의 통합 프레임워크.
- Few-shot 및 zero-shot 설정에서 특히 우수한 성능.
- 멀티모달 시계열 예측의 새로운 방향 제시.

## 5. 한계 및 후속 연구 아이디어
- VLM 기반이어서 모델 크기가 상당히 큼.
- 이미지 변환 방식(시계열 → 이미지)의 최적화 여지.
- ※ 메모: Time-LLM(ICLR 2024)의 LLM reprogramming을 멀티모달 VLM으로 확장.

## 6. 참고
- 관련 논문: Time-LLM (ICLR 2024), GPT4TS (NeurIPS 2023), TEMPO (ICLR 2024)
- GitHub: https://github.com/CityMind-Lab/ICML25-TimeVLM
- arXiv: https://arxiv.org/abs/2502.04395
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44762
