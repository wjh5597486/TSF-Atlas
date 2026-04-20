---
title: "ChatTime: A Unified Multimodal Time Series Foundation Model Bridging Numerical and Textual Data"
authors: Chengsen Wang et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2412.11376
code_url: https://github.com/forestsking/chattime
tags: [foundation-llm, pretrained-native, multimodal, zero-shot, time-series-as-language]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_ChatTime.pdf
pdf_source: arxiv
---

## TL;DR
ChatTime는 시계열을 "외국어"로 취급하여 수치 및 텍스트 데이터를 통합하는 멀티모달 시계열 파운데이션 모델이다. 이중 모달리티 입출력으로 zero-shot 예측과 맥락 기반 예측을 지원하며, AAAI 2025 Oral 논문으로 선정되었다.

## 1. 기존 모델의 한계 / 가설
- 기존 파운데이션 모델은 수치(시계열) 또는 텍스트 중 하나만 처리
- 시계열과 텍스트 컨텍스트를 함께 활용하는 멀티모달 예측이 부재
- 가설: 시계열을 언어처럼 취급하고 텍스트 컨텍스트를 결합하면 더 풍부한 예측 가능

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 = 외국어 패러다임, 이중 모달리티 입출력
- **아키텍처 구성요소**:
  1. **Unified Tokenization**: 시계열을 텍스트 토큰 공간으로 변환
  2. **Bimodal Input**: 수치 시계열 + 텍스트 컨텍스트 동시 입력
  3. **Bimodal Output**: 수치 예측 + 텍스트 설명 동시 출력
  4. **Foundation Pre-training**: 대규모 다도메인 데이터로 사전 학습
- **신규 데이터셋**: Melbourne Solar, London Electricity, Paris Traffic 등 4개 멀티모달 데이터셋 구축

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 다양한 설정 (zero-shot 평가)
- Forecast horizons: 다양한 horizon 설정
- Normalization: 표준
- Train/Val/Test split: zero-shot (미학습 데이터셋 평가)
- Batch size / Optimizer / LR: 파운데이션 모델 사전 학습 설정
- Loss function: 시계열 예측 + 언어 생성 joint loss
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electric, Exchange, Traffic, Weather 등

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| various | see Table 4 | 0.1372–0.1698 MAE | zero-shot 기준 이전 SOTA 99.9% 달성 (4% 사전학습 데이터) |

출처: Table 4 of paper (zero-shot forecasting evaluation).

## 4. 주요 기여
- 시계열을 외국어로 취급하는 새로운 패러다임
- 이중 모달리티 입출력 지원 (수치+텍스트)
- 4% 사전학습 데이터로 기존 SOTA 99.9% 달성 (zero-shot)
- 맥락 기반 예측에서 기존 SOTA 대비 3.7% 향상
- 시계열 질의응답(Q&A) 태스크 지원
- 4개 새로운 멀티모달 데이터셋 공개

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 수치 정밀도 손실 (텍스트 토큰화 과정)
- ※ 메모: zero-shot 위주 평가라 ETTh1 표준 LTSF 세팅과 직접 비교 어려움

## 6. 참고
- [arXiv 2412.11376](https://arxiv.org/abs/2412.11376)
- [GitHub](https://github.com/forestsking/chattime)
- 비교: Chronos, TimesFM, MOIRAI, LagLlama
