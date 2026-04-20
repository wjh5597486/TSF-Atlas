---
title: "Are Language Models Actually Useful for Time Series Forecasting?"
authors: Mingtian Tan et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2406.16964
code_url: https://github.com/BennyTMT/LLMsForTimeSeries
tags: [llm, analysis, ablation, reprogramming, time-domain]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_AreLLMsUseful.pdf
pdf_source: arxiv
---

## TL;DR
LLM 기반 시계열 예측 방법에서 LLM 컴포넌트를 제거하거나 단순 어텐션으로 교체해도 성능이 저하되지 않으며 오히려 향상되는 경우도 있다는 분석 논문. PAttn이라는 단순 baseline이 대부분 LLM 기반 모델을 능가.

## 1. 기존 모델의 한계 / 가설
- Time-LLM, CALF, OFA 등 LLM 기반 TSF 모델의 핵심인 LLM 모듈이 실제로 기여하는지 검증 미흡.
- 단순히 LLM을 포함한다는 이유로 성능이 높다고 주장되지만 ablation 불충분.
- 가설: LLM 모듈을 단순 어텐션이나 제거로 대체해도 성능이 유지되면 LLM이 실제로 불필요한 것.

## 2. 제안 방법론
- **핵심 아이디어**: 3가지 LLM 기반 TSF 모델(OFA, Time-LLM, CALF)에서 LLM 제거/교체 ablation + PAttn 단순 baseline 제안.
- **아키텍처 (PAttn)**:
  - 단순 Patch Attention 레이어만 사용 (LLM 없음)
  - 경량 어텐션으로 시계열 패치 정보 집약
- **실험**:
  - LLM 완전 제거 → 동등하거나 향상된 성능
  - LLM을 랜덤 가중치 transformer로 교체 → 유사 성능
  - PAttn이 대부분 LLM 기반 모델 능가

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/512
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | LLM 기반 모델 대비 PAttn 우수 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1~3 of paper.

## 4. 주요 기여
- LLM 기반 TSF 모델에서 LLM 모듈의 실제 기여 부재 실증
- PAttn 단순 baseline이 대부분 LLM 기반 모델 능가
- LLM TSF 연구의 방향성에 대한 비판적 분석
- 학계에 LLM for TSF 연구의 엄격한 ablation 필요성 제기

## 5. 한계 및 후속 연구 아이디어
- 분석 대상이 3개 LLM 기반 모델로 제한 (더 최신 모델 미포함)
- LLM이 zero-shot이나 cross-modal 태스크에서는 유용할 가능성 남아있음

## 6. 참고
- [GitHub](https://github.com/BennyTMT/LLMsForTimeSeries)
- [arXiv 2406.16964](https://arxiv.org/abs/2406.16964)
