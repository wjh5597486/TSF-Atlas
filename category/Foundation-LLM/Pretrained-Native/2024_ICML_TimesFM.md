---
title: "A Decoder-Only Foundation Model for Time-Series Forecasting"
authors: Abhimanyu Das et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2310.10688
code_url: https://github.com/google-research/timesfm
tags: [foundation-model, decoder-only, pretrained, transformer, zero-shot, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_TimesFM.pdf
pdf_source: arxiv
---

## TL;DR
TimesFM은 Google Research의 decoder-only Transformer 시계열 파운데이션 모델. 1000억 개 실세계 타임포인트로 사전학습하여 제로샷 예측에서 완전 지도 학습 모델과 경쟁. ICML 2024 발표.

## 1. 기존 모델의 한계 / 가설
- 기존 파운데이션 모델은 인코더 기반이거나 특정 도메인에 제한.
- 대규모 사전학습의 시계열 예측에의 효과가 NLP 수준으로 검증되지 않음.
- 가설: 100B 타임포인트로 학습한 decoder-only 모델이 제로샷으로 SOTA 수준 달성 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 자기회귀 decoder-only Transformer + 대규모 시계열 코퍼스 사전학습.
- **아키텍처**:
  - Decoder-only Transformer (GPT-스타일)
  - 시계열 패치 임베딩 → 자기회귀 예측
  - 1000억 타임포인트 사전학습 (다양한 도메인)
  - 임의 길이 입력/출력 지원
- **손실함수**: Autoregressive MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 512
- Forecast horizons: 96, 192, 336, 720
- 제로샷 평가 (ETTh1 미포함 사전학습)
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 제로샷에서 full-shot 모델 대비 경쟁력 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 100B 타임포인트 대규모 사전학습 TSF 파운데이션 모델
- Decoder-only 구조로 자기회귀 예측
- 제로샷에서 완전 지도 학습 모델 대비 경쟁력
- Google Research 공개 모델 (오픈소스)

## 5. 한계 및 후속 연구 아이디어
- 사전학습 데이터에 포함된 도메인 외 데이터에서 일반화 한계
- 긴 입력 컨텍스트 처리 비용

## 6. 참고
- [GitHub](https://github.com/google-research/timesfm)
- [arXiv 2310.10688](https://arxiv.org/abs/2310.10688)
