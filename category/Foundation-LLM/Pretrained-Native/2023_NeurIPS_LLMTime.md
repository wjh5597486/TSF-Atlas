---
title: "Large Language Models Are Zero-Shot Time Series Forecasters"
authors: Nate Gruver et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2310.07820
code_url: https://github.com/ngruver/llmtime
tags: [foundation-model, llm, zero-shot, forecasting, tokenization, gpt, llama, pretrained]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_LLMTime.pdf
pdf_source: arxiv
---

## TL;DR
LLMTime은 시계열을 숫자 문자열로 인코딩하여 LLM(GPT-3, LLaMA-2)의 다음 토큰 예측으로 시계열 예측을 수행하는 제로샷 프레임워크다. 별도의 파인튜닝 없이도 특수 목적 시계열 모델과 비슷하거나 더 나은 성능을 달성하며, LLM이 단순성·반복성 편향을 통해 시계열의 특성을 자연스럽게 포착함을 보인다.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 예측 모델은 각 태스크에 특화된 훈련이 필요하며, 제로샷 일반화 능력이 제한적이다.
- LLM의 강력한 시퀀스 모델링 능력이 시계열 도메인에서 활용되지 않고 있다.
- 가설: 시계열을 숫자 텍스트로 표현하면 LLM이 사전학습에서 습득한 패턴 인식 능력으로 제로샷 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 값을 숫자 문자열로 직렬화하고 LLM의 다음 토큰 예측을 활용. 이산 분포를 연속 분포로 변환하는 토크나이징 절차 포함.
- **아키텍처**:
  1. **시계열 직렬화**: 수치값을 소수점 포함 숫자 문자열로 변환.
  2. **LLM 추론**: GPT-3, LLaMA-2 등의 다음 토큰 예측 확률분포 활용.
  3. **연속 분포 변환**: 이산 토큰 확률을 연속 밀도 함수로 변환.
- **모델**: GPT-3, GPT-4, LLaMA-2 (파인튜닝 없음, 프롬프트만 사용).

## 3. ETTh1 실험 결과
> 본 논문은 ETTh1 표준 벤치마크 결과를 보고하지 않음. 주로 M4, Weather, 금융 데이터 등에서 제로샷 평가 수행.

## 4. 주요 기여
- LLM을 시계열 예측에 제로샷으로 활용하는 최초의 체계적 연구.
- 숫자 직렬화 및 토크나이징 전략 제안.
- LLM의 단순성·반복성 편향이 시계열 예측에 유리하다는 통찰 제공.
- 목적 모델 없이 GPT-3/LLaMA-2로 경쟁력 있는 성능 달성.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 장기 예측에서는 목적 모델 대비 성능 격차 발생. 숫자 토크나이징의 정밀도 한계.
- ※ 메모: GPT4TS(NeurIPS 2023)가 파인튜닝 기반이라면, LLMTime은 제로샷으로 대비되는 접근. Lag-Llama, Chronos 등 후속 작업의 동기 제공.

## 6. 참고
- arXiv: https://arxiv.org/abs/2310.07820
- GitHub: https://github.com/ngruver/llmtime
- 관련: GPT4TS (NeurIPS 2023), Chronos (ICML 2024), Lag-Llama
