---
title: "TiRex: Zero-Shot Forecasting Across Long and Short Horizons with Enhanced In-Context Learning"
authors: Andreas Auer et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.23719
code_url: https://github.com/NX-AI/tirex
tags: [foundation-model, pretrained-native, xlstm, in-context-learning, zero-shot, ssm]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: false
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TiRex.pdf
pdf_source: arxiv
---

## TL;DR
TiRex는 xLSTM 기반의 향상된 in-context learning 능력을 갖춘 시계열 Foundation Model이다. CPM(Contextual Pattern Masking) 학습 전략을 통해 Transformer와 LSTM의 격차를 좁히고, HuggingFace GiftEval·Chronos-ZS 제로샷 벤치마크에서 Amazon·Google·Salesforce 등 더 큰 모델들을 능가한다.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 TSFM은 in-context learning 능력이 강하지만 상태 추적(state-tracking)에 약함.
- LSTM은 상태 추적이 강하지만 LLM 수준의 in-context learning 능력이 부족.
- 가설: 향상된 LSTM(xLSTM)과 CPM 학습 전략을 결합하면 Transformer의 in-context learning + LSTM의 상태 추적을 동시에 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: xLSTM + CPM(Contextual Pattern Masking)으로 Transformer-LSTM 격차 해소.
- **아키텍처**:
  - **xLSTM 기반**: 확장된 LSTM으로 강력한 상태 추적 + in-context learning 능력.
  - **CPM(Contextual Pattern Masking)**: 훈련 시 마스킹 전략으로 상태 추적 능력 향상.
  - 과거 값을 컨텍스트로 사용하는 in-context 예측(zero-shot prediction 지원).
- **평가 벤치마크**: GiftEval, Chronos-ZS(HuggingFace). Amazon·Google·Salesforce 모델 능가.

## 3. ETTh1 실험 결과
> reports_etth1: false. TiRex는 주로 GiftEval·Chronos-ZS 제로샷 벤치마크 평가. 표준 ETTh1 결정론적 LTSF 벤치마크 결과는 논문에서 주요 평가로 다루지 않음.

## 4. 주요 기여
- xLSTM 기반 시계열 in-context learning 모델 TiRex 제안.
- CPM(Contextual Pattern Masking)으로 상태 추적 능력 향상.
- HuggingFace GiftEval·Chronos-ZS에서 더 큰 모델들(Chronos Bolt, TimesFM, MOIRAI, TabPFN-TS) 능가.
- 장단기 horizon 모두에서 강력한 제로샷 예측 성능.

## 5. 한계 및 후속 연구 아이디어
- 제로샷 평가에 집중되어 있어 파인튜닝 성능 추가 검증 필요.
- xLSTM의 병렬 연산 효율성 개선 여지.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.23719
- GitHub: https://github.com/NX-AI/tirex
- OpenReview: https://openreview.net/forum?id=v7UqniC9pF
※ 메모: ETTh1 표준 벤치마크 결과 미보고. 제로샷(GiftEval, Chronos-ZS) 중심 평가.
