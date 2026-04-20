---
title: "DAM: Towards A Foundation Model for Time Series Forecasting"
authors: Luke Nicholas Darlow, Qiwen Deng, Ahmed Hassan, Martin Asenov, Rajkarn Singh, Artjom Joosen, Adam Barker, Amos Storkey
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2407.17165
code_url: ""
tags: [foundation, decoder-only, basis-function, zero-shot]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_DAM.pdf
pdf_source: arxiv
---

## TL;DR
시계열 전용 decoder-only Transformer foundation 모델. **랜덤 히스토리 샘플링**으로 다양한 lookback을 학습하고 출력을 **연속 basis function**으로 파라미터화해 가변 horizon을 다룸.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 특정 lookback/horizon에 과적합.
- 연속 시간 basis로 미래를 파라미터화하면 zero-shot 전이 가능.

## 2. 제안 방법론
- 훈련 중 히스토리 길이를 무작위 샘플.
- 출력은 sin/cos 등 연속 basis의 가중합 계수.
- Decoder-only Transformer.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Zero-shot 평가 포함. Lookback은 학습 시 랜덤.

### 3.2 Results
ETTh1/ETTh2/ETTm1에서 finetuned 설정 기준 SOTA 및 zero-shot에서 경쟁적 성능 보고(원문 Table 2).

## 4. 주요 기여
- Lookback-agnostic 학습 + basis output으로 zero-shot TSF.
- 단일 foundation 모델로 다수 데이터셋 커버.

## 5. 참고
- 원문 Table 2, Appendix 참고.
