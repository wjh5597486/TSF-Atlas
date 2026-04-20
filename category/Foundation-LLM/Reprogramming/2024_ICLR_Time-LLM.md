---
title: "Time-LLM: Time Series Forecasting by Reprogramming Large Language Models"
authors: Ming Jin, Shiyu Wang, Lintao Ma, Zhixuan Chu, James Y. Zhang, Xiaoming Shi, Pin-Yu Chen, Yuxuan Liang, Yuan-Fang Li, Shirui Pan, Qingsong Wen
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2310.01728
code_url: https://github.com/KimMeen/Time-LLM
tags: [foundation, llm, reprogramming, few-shot]
primary_category: category/Foundation-LLM/Reprogramming
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_Time-LLM.pdf
pdf_source: arxiv
---

## TL;DR
Frozen LLM(LLaMA 등) 앞단에 시계열 **reprogramming layer**를 붙여 LLM을 수정 없이 TSF에 사용. Text prototypes와 Prompt-as-Prefix(PaP)로 시계열 패치를 텍스트 공간에 projection.

## 1. 기존 모델의 한계 / 가설
- 시계열 전용 foundation 모델은 없거나 데이터가 부족.
- LLM의 풍부한 representation을 그대로 재사용할 수 있다면 zero/few-shot forecasting이 가능.

## 2. 제안 방법론
- Patch embedding → **Text prototype reprogramming**(cross-attention으로 시계열 패치를 LLM 토큰 공간으로 재표현).
- **Prompt-as-Prefix**: 도메인 정보를 자연어 프리픽스로 주입.
- LLM 자체는 frozen.

## 3. ETTh1 실험 결과
### 3.1 Setting
- LLM backbone: LLaMA-7B(주), GPT-2 (ablation).
- Horizons = {96, 192, 336, 720}, Lookback은 데이터별.

### 3.2 Results
ETTh1 포함 long-term forecasting에서 GPT4TS·PatchTST 대비 우수 보고. 상세 수치는 원문 Table 2 참고.

## 4. 주요 기여
- 프리트레인 LLM을 수정 없이 TSF로 reprogramming.
- Few-shot / zero-shot TSF 강점.

## 5. 참고
- 코드: https://github.com/KimMeen/Time-LLM
