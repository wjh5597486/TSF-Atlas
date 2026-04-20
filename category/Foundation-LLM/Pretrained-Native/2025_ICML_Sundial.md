---
title: "Sundial: A Family of Highly Capable Time Series Foundation Models"
authors: Yong Liu et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2502.00816
code_url: https://github.com/thuml/Sundial
tags: [foundation-model, generative, flow-matching, pre-training, probabilistic-forecasting, zero-shot]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: false
swept_in: 2026-04-19
pdf_local: ./2025_ICML_Sundial.pdf
pdf_source: arxiv
---

## TL;DR
Sundial is an ICML 2025 Oral paper from Tsinghua University that introduces a family of native, flexible, and scalable time series foundation models. Pre-trained on TimeBench (1 trillion time points), it uses a novel TimeFlow Loss based on flow-matching for continuous-valued time series without discrete tokenization, achieving SOTA on both point and probabilistic forecasting benchmarks in zero-shot settings.

## 1. 기존 모델의 한계 / 가설
- 기존 LLM 기반 TSF(GPT4TS, Time-LLM 등)는 LLM 아키텍처를 그대로 차용하여 연속값 시계열에 최적화되지 않음.
- 이산 토큰화(discrete tokenization)는 연속값 시계열에서 정밀도 손실을 유발.
- TimeFlow Loss (flow-matching 기반)로 이산화 없이 연속값 시계열을 직접 사전훈련 가능하다는 가설.
- 1조(1T) 시간 포인트 규모의 사전훈련 데이터(TimeBench) 필요성.

## 2. 제안 방법론
- **핵심 아이디어**: Flow-matching 기반 TimeFlow Loss로 연속값 시계열을 이산화 없이 네이티브 사전훈련.
- **아키텍처**:
  - Transformer 기반 backbone (최소 수정).
  - **TimeFlow Loss**: Flow-matching 기반 손실로 continuous 시계열의 생성 분포 학습.
  - Mode collapse 방지를 위한 정규화.
  - 다양한 크기의 Sundial family (tiny, small, base 등).
- **사전훈련 데이터**: TimeBench — 1조 시간 포인트, 실제 데이터 + 합성 데이터.
- **손실함수**: TimeFlow Loss (flow-matching 기반).
- **추론**: Just-in-time inference (수 밀리초 내 zero-shot 예측).

## 3. ETTh1 실험 결과
> reports_etth1: false. Sundial은 주로 zero-shot 확률적 예측 벤치마크에서 평가되며, 표준 결정론적 ETTh1 LTSF 벤치마크를 직접 보고하지 않음.
> 관련 결과는 논문 내 GiftEval, Monash 등 zero-shot 벤치마크 참조.

## 4. 주요 기여
- TimeFlow Loss: 이산화 없는 연속값 시계열 flow-matching 사전훈련 (ICML 2025 Oral).
- TimeBench: 1조 시간 포인트 사전훈련 데이터셋 구축.
- 점 예측(point) 및 확률 예측(probabilistic) 모두에서 SOTA zero-shot 성능.
- Just-in-time 추론: 수 밀리초 내 zero-shot 예측.

## 5. 한계 및 후속 연구 아이디어
- 1조 포인트 사전훈련 데이터의 구성 bias가 특정 도메인에 유리할 수 있음.
- ※ 메모: Timer(ICML 2024)의 후속작으로, Tsinghua thuml 그룹의 대규모 foundation model 방향.

## 6. 참고
- 관련 논문: Timer (ICML 2024), TimesFM (ICML 2024), MOIRAI (ICML 2024)
- GitHub: https://github.com/thuml/Sundial
- arXiv: https://arxiv.org/abs/2502.00816
- ICML 2025 Oral: https://icml.cc/virtual/2025/oral/47235
