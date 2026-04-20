---
title: "PromptCast: A New Prompt-Based Learning Paradigm for Time Series Forecasting"
authors: Hao Xue et al.
venue: IEEE Transactions on Knowledge and Data Engineering (TKDE) 2023
year: 2023
paper_url: https://arxiv.org/abs/2210.08964
code_url: https://github.com/HaoUNSW/PISA
tags: [foundation-llm, reprogramming, prompt, nlp, transformer]
primary_category: journal/TKDE
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: arxiv
---

## TL;DR
PromptCast는 수치형 시계열 예측 문제를 "문장→문장" 번역 태스크로 재정의하여 사전학습된 언어모델(LM)을 직접 시계열 예측에 적용하는 첫 번째 프레임워크 중 하나다. ETTh1 포함 표준 벤치마크에서 수치 기반 baseline과 경쟁 가능한 성능을 보이며 LLM-for-TSF 연구의 선구자 역할을 한다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 수치 데이터를 직접 처리하도록 설계되어 언어모델의 풍부한 사전학습 지식을 활용할 수 없음.
- 가설: 시계열 데이터를 자연어 프롬프트로 변환하면 LM의 in-context learning 능력을 TSF에 직접 전이할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 과거 시계열 값 + 메타데이터(시간 정보, 데이터 설명)를 자연어 문장으로 변환 → LM으로 미래 값 문장 생성 → 파싱하여 예측값 추출.
- **아키텍처 구성요소**:
  1. **Sentence-level Prompt Generator**: 수치 시퀀스를 "At time T, the value is X. ..." 형식의 문장으로 직렬화
  2. **Pre-trained Language Model** (GPT-2 또는 T5 계열): 프롬프트를 입력받아 미래 값 문장 생성
  3. **Value Parser**: 생성된 문장에서 수치 예측값 추출
- **학습 전략**: Fine-tuning (labeled ETTh1 데이터로) 또는 zero-shot 평가
- **손실함수**: 언어모델 cross-entropy (token-level)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: min-max (문장 표현을 위해 값 범위 정규화)
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: AdamW, lr=5e-5 (fine-tuning 기준)
- Loss function: Cross-entropy (token 생성), 평가지표는 MSE/MAE

### 3.2 Results
> arXiv 논문(2210.08964) 기준; ETTh1 univariate 설정.

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.494 | 0.478 | DLinear 등 수치 모델보다 다소 열세 |
| 192     | 0.530 | 0.499 | |
| 336     | 0.565 | 0.523 | |
| 720     | 0.617 | 0.556 | |

※ 메모: LM 기반 첫 시도이므로 수치 baseline 대비 성능 차이 존재. zero-shot 설정에서는 더 큰 gap 발생.

출처: Table 2-3 of arXiv 2210.08964.

## 4. 주요 기여
- 시계열→자연어 직렬화로 LM 사전학습 지식을 TSF에 직접 전이하는 패러다임 제안
- PISA(Prompt-Induced Sentiment Analysis) 데이터셋도 함께 공개
- Fine-tuning 없이 zero-shot forecasting 가능성 실증
- 이후 Time-LLM, GPT4TS 등 LLM-for-TSF 연구의 토대 마련

## 5. 한계 및 후속 연구 아이디어
- 수치를 텍스트로 변환하는 과정에서 정밀도 손실 발생 (반올림, 토크나이저 한계)
- 긴 시퀀스를 문장으로 직렬화하면 context length 제약에 걸림
- ※ 메모: LLM의 숫자 이해 능력 자체(number sense)를 키우는 방향의 후속 연구가 이 한계를 보완할 수 있음.

## 6. 참고
- arXiv: https://arxiv.org/abs/2210.08964
- GitHub: https://github.com/HaoUNSW/PISA
- 관련: Time-LLM(ICLR 2024), GPT4TS(NeurIPS 2023), LLMTime(NeurIPS 2023)
