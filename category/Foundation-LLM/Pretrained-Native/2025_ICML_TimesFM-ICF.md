---
title: "In-Context Fine-Tuning for Time-Series Foundation Models"
authors: Abhimanyu Das et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2410.24087
code_url: ""
tags: [foundation-model, in-context-learning, fine-tuning, zero-shot, few-shot, retrieval]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimesFM-ICF.pdf
pdf_source: arxiv
---

## TL;DR
In-Context Fine-Tuning (ICF) extends time series foundation models (specifically TimesFM) to utilize multiple related time series in the context window at inference time, enabling in-context adaptation without parameter updates. Evaluated on ETTh1/2 and ETTm1/2, it achieves at least 25% improvement over other baselines and 7% over vanilla TimesFM.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF foundation model은 추론 시 단일 시계열 입력만을 사용하여, 관련된 다른 시계열에서 얻을 수 있는 컨텍스트 정보를 활용하지 못함.
- 파라미터 업데이트 없이 컨텍스트 내 관련 시계열 예제를 제공하면 도메인 특화 적응이 가능하다는 가설 (in-context learning).

## 2. 제안 방법론
- **핵심 아이디어**: 추론 시 컨텍스트 윈도우에 여러 관련 시계열 예제를 제공하여 인-컨텍스트 파인튜닝(ICF) 수행.
- **아키텍처**:
  - TimesFM 백본 기반.
  - **In-context Fine-Tuning**: 컨텍스트 윈도우에 관련 시계열 예제를 배치.
  - Foundation model이 예제에서 패턴을 추출하여 타겟 시계열에 적응.
  - 파라미터 업데이트 없음.
- **손실함수**: 학습 시 MSE (사전훈련 단계에서 ICF 능력 학습).
- **학습 전략**: Pre-training 시 multi-series context 학습 후 zero-shot 사용.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): TimesFM 기본값
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할 (ETTh1, ETTh2, ETTm1, ETTm2)
- Batch size / Optimizer / LR: N/A (zero-shot, no parameter update)
- Loss function: N/A (inference only)
- 기타: ETT family 4개 데이터셋 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | +25% vs baselines, +7% vs TimesFM |
| 192     | see paper | see paper | +25% vs baselines |
| 336     | see paper | see paper | +25% vs baselines |
| 720     | see paper | see paper | +25% vs baselines |

출처: Table of paper (ETTh1, ETTh2, ETTm1, ETTm2).

## 4. 주요 기여
- In-context fine-tuning으로 파라미터 업데이트 없는 도메인 적응.
- ETT family에서 기존 baselines 대비 25% 이상, TimesFM 대비 7% 성능 향상.
- Foundation model에 multi-series context 학습 능력 부여.

## 5. 한계 및 후속 연구 아이디어
- 컨텍스트 내 시계열 선택(retrieval) 전략이 성능에 큰 영향.
- ※ 메모: RAFT(ICML 2025)와 상보적: ICF는 FM의 컨텍스트 창을 활용, RAFT는 외부 검색 엔진 활용.

## 6. 참고
- 관련 논문: TimesFM (ICML 2024), RAFT (ICML 2025), ELF (ICML 2025)
- arXiv: https://arxiv.org/abs/2410.24087
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/43707
