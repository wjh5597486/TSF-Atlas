---
title: "TimeXer: Empowering Transformers for Time Series Forecasting with Exogenous Variables"
authors: Yuxuan Wang et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.19072
code_url: https://github.com/thuml/TimeXer
tags: [transformer, exogenous, multivariate, time-domain, patching]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/Patching]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_TimeXer.pdf
pdf_source: arxiv
---

## TL;DR
TimeXer empowers canonical Transformers to reconcile endogenous and exogenous (covariate) information via patch-wise self-attention and variate-wise cross-attention. It achieves state-of-the-art on 12 real-world benchmarks including ETT, Exchange, Weather.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 내생(endogenous) 변수만을 고려하며, 외생(exogenous) 변수를 효과적으로 활용하지 못함.
- 실제 응용에서는 외부 요인(날씨, 경제지표 등)이 예측에 핵심적인 역할을 하지만 기존 Transformer는 이를 무시하거나 단순 concatenation으로만 처리.

## 2. 제안 방법론
- **핵심 아이디어**: 내생 변수는 patch-level 토큰화, 외생 변수는 variate-level 토큰화로 처리 후 cross-attention으로 융합.
- **아키텍처**:
  - 내생 시계열 → 비중첩 패치 임베딩 → patch-wise self-attention
  - 외생 변수 → variate-level global token
  - Variate-wise cross-attention으로 내생 patch가 외생 global token에 attend
  - Standard Transformer encoder 상단에 예측 head
- **학습 전략**: 표준 MSE loss, teacher forcing 없음(비자기회귀).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (endogenous), full history (exogenous)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (instance normalization)
- Train/Val/Test split: standard ETT split
- Batch size / Optimizer / LR: Adam, 표준 설정
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | SOTA 수준 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 외생 변수를 효과적으로 활용하는 최초의 범용 Transformer TSF 프레임워크
- Patch-level + variate-level 이중 토큰화로 시간/변수 축 정보 분리
- 12개 벤치마크에서 일관된 SOTA 달성
- 외생 변수 유무에 관계없이 동작하는 범용성

## 5. 한계 및 후속 연구 아이디어
- 외생 변수의 품질/노이즈에 민감할 수 있음
- 실시간 외생 변수 스트리밍 시나리오 미검토

## 6. 참고
- [GitHub](https://github.com/thuml/TimeXer)
- [arXiv 2402.19072](https://arxiv.org/abs/2402.19072)
