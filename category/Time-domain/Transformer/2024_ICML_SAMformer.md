---
title: "SAMformer: Unlocking the Potential of Transformers in Time Series Forecasting with Sharpness-Aware Minimization and Channel-Wise Attention"
authors: Romain Ilbert et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.10198
code_url: https://github.com/romilbert/samformer
tags: [transformer, sam, sharpness-aware, channel-wise-attention, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/Loss]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_SAMformer.pdf
pdf_source: arxiv
---

## TL;DR
SAMformer는 Transformer가 LTSF에서 선형 모델보다 성능이 낮은 원인이 Attention의 낮은 일반화 때문임을 밝히고, SAM(Sharpness-Aware Minimization) 최적화 + 채널별 어텐션으로 이를 극복. ICML 2024 Oral.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델이 다변량 LTSF에서 DLinear 등 단순 선형 모델보다 성능이 낮음.
- 기존 연구는 아키텍처 수정으로 해결하려 했으나, 최적화 문제(bad local minima)가 근본 원인.
- 가설: SAM으로 날카로운 손실 지형(sharp loss landscape)을 평탄하게 하면 Transformer가 선형 모델을 능가 가능.

## 2. 제안 방법론
- **핵심 아이디어**: SAM 최적화 + Channel-Wise Attention으로 Transformer의 일반화 성능 개선.
- **아키텍처**:
  - 얕은 경량 Transformer (1~2 레이어)
  - Channel-Wise Attention: 채널 간 의존성 포착 (시간 차원이 아닌 채널 차원으로 어텐션)
  - SAM 최적화: perturbation으로 flat minima 탐색
- **손실함수**: MSE + SAM 정규화

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Optimizer: SAM (base: Adam)
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | DLinear, PatchTST 대비 우수 또는 동등 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- Transformer LTSF 성능 저하 원인: 최적화(sharp minima) 문제 규명
- SAM + Channel-Wise Attention으로 근본 원인 해결
- MOIRAI 등 대형 파운데이션 모델과 동등 성능 (훨씬 적은 파라미터)
- ICML 2024 Oral 수상

## 5. 한계 및 후속 연구 아이디어
- SAM 최적화로 인한 2× 훈련 비용 증가
- 더 깊은 Transformer에서의 효과 미검토

## 6. 참고
- [GitHub](https://github.com/romilbert/samformer)
- [arXiv 2402.10198](https://arxiv.org/abs/2402.10198)
