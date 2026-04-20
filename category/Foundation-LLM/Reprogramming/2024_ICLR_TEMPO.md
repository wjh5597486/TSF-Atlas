---
title: "TEMPO: Prompt-based Generative Pre-trained Transformer for Time Series Forecasting"
authors: Defu Cao, Furong Jia, Sercan Ö. Arık, Tomas Pfister, Yixiang Zheng, Wen Ye, Yan Liu
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2310.04948
code_url: https://github.com/DC-research/TEMPO
tags: [foundation, llm, gpt, prompt, decomposition]
primary_category: category/Foundation-LLM/Reprogramming
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_TEMPO.pdf
pdf_source: publisher
---

## TL;DR
GPT(decoder LLM) 위에 **추세·계절·잔차 분해** + 학습 가능한 **prompt** 를 얹어 TSF로 finetune. 도메인/데이터별 soft prompt가 일반화를 돕는다.

## 1. 기존 모델의 한계 / 가설
- LLM을 TSF로 단순 fine-tune하면 시계열의 구조적 inductive bias를 활용 못 함.
- 분해 + prompt pool로 도메인 간 일반화를 높일 수 있다.

## 2. 제안 방법론
- 입력 시계열을 trend/season/residual로 분해.
- 각 컴포넌트를 토큰화 → **도메인별 soft prompt**를 결합 → GPT backbone.
- Parameter-efficient fine-tuning(LoRA 등) 적용 가능.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Backbone: GPT-2/3 계열. Lookback/horizon = 일반적 {96/96~720}.

### 3.2 Results
ETTh1 포함 다수 벤치마크에서 GPT4TS 대비 개선 보고. 상세는 원문 Table 참고.

## 4. 주요 기여
- 시계열 inductive bias(decomposition) + LLM 조합.
- Prompt pool로 few-shot / 도메인 간 전이 강화.

## 5. 참고
- 코드: https://github.com/DC-research/TEMPO
