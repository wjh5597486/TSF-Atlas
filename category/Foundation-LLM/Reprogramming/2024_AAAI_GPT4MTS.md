---
title: "GPT4MTS: Prompt-based Large Language Model for Multimodal Time-series Forecasting"
authors: Furong Jia et al.
venue: AAAI
year: 2024
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/30383
code_url: https://github.com/Flora-jia-jfr/GPT4MTS-Prompt-based-Large-Language-Model-for-Multimodal-Time-series-Forecasting
tags: [foundation-llm, reprogramming, multimodal, gpt2, time-series, prompt]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_GPT4MTS.pdf
pdf_source: arxiv
---

## TL;DR
GPT4MTS는 수치 시계열 데이터와 텍스트 정보(뉴스 등)를 동시에 활용하는 프롬프트 기반 멀티모달 시계열 예측 프레임워크이다. GPT-2를 백본으로 사용하며, BERT 임베딩으로 텍스트를 인코딩하고 시계열은 패칭+RevIN으로 처리하여 두 모달리티를 통합 예측한다. 또한 GDELT 기반 뉴스-영향 시계열 데이터셋을 새로 소개한다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델들은 수치 시계열 데이터만 활용하고, 관련 텍스트(뉴스, 이벤트 보고서 등)를 무시함.
- LLM을 시계열에 적용한 기존 연구(GPT4TS 등)는 단일 모달리티만 처리.
- 가설: 수치 데이터와 텍스트 데이터를 결합하면 예측 성능, 특히 외부 사건의 영향을 받는 도메인에서 향상 가능.

## 2. 제안 방법론
- **핵심 아이디어**: GPT-2 기반 통합 프롬프트 레이어로 수치 시계열과 텍스트를 동일한 입력 공간에서 처리.
- **GPT4MTS 구성요소**:
  1. **텍스트 인코딩**: BERT로 관련 뉴스/텍스트를 임베딩.
  2. **시계열 인코딩**: RevIN + 패칭(patching) 후 GPT-2 입력 차원으로 투영.
  3. **프롬프트 레이어**: 시계열과 텍스트 임베딩을 GPT-2 입력으로 결합.
  4. **GPT-2 Backbone**: 동결(frozen) 또는 부분 파인튜닝.
  5. **예측 헤드**: 선형 레이어로 최종 예측 출력.
- 손실함수: MSE.
- 학습 전략: GPT-2를 동결하고 프롬프트 레이어와 예측 헤드만 학습.
- **새 데이터셋**: GDELT 기반 멀티모달 시계열 데이터셋 (뉴스 영향 예측용) 도입.

## 3. ETTh1 실험 결과
> ETTh1을 직접 평가 대상으로 삼지 않음. 주 실험은 GDELT 기반 멀티모달 데이터셋 및 표준 단변량/다변량 TSF 데이터셋(ETT 계열 포함 가능)에서 수행.
> `reports_etth1: false`

## 4. 주요 기여
- GPT-2를 멀티모달(수치+텍스트) 시계열 예측에 적용한 선구적 연구.
- LLM 텍스트 지식과 시계열 수치 정보를 통합하는 일반적인 원칙 제시.
- GDELT 기반 뉴스-영향 멀티모달 시계열 데이터셋 공개.
- FEDformer, PatchTST, DLinear, GPT4TS 대비 GDELT 데이터셋에서 4.14% MSE 감소.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 텍스트 관련성 자동 평가 및 선택 메커니즘 필요.
- ※ 메모: 더 강력한 LLM(LLaMA, Qwen 등)으로 교체 시 성능 향상 가능.
- ※ 메모: 멀티모달 정렬 방식을 교차 모달 어텐션으로 개선하는 방향 (cf. TimeCMA).

## 6. 참고
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/30383
- GitHub: https://github.com/Flora-jia-jfr/GPT4MTS-Prompt-based-Large-Language-Model-for-Multimodal-Time-series-Forecasting
- ※ 원본 PDF 미취득: 공개 arXiv 버전 없음.
