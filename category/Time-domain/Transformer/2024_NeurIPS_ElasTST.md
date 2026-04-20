---
title: "ElasTST: Towards Robust Varied-Horizon Forecasting with Elastic Time-Series Transformer"
authors: Jiawen Zhang et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2411.01842
code_url: ""
tags: [transformer, varied-horizon, non-autoregressive, patching, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/Patching]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_ElasTST.pdf
pdf_source: arxiv
---

## TL;DR
ElasTST는 단일 모델로 다양한 예측 지평선(varied horizon)에 견고하게 대응하는 비자기회귀 Transformer. Placeholder 기반 설계와 구조화된 self-attention 마스크로 추론 시 horizon 변경에 불변.

## 1. 기존 모델의 한계 / 가설
- 기존 모델은 학습 시 고정 horizon에 최적화되어, 다른 horizon에서는 별도 재학습 필요.
- 실제 산업 환경에서는 다양한 horizon 예측이 필요하지만 대부분 모델이 이를 지원하지 않음.
- 가설: Placeholder + 구조화 attention mask로 horizon에 불변한 출력 생성 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 추론 horizon 변경에 불변한 비자기회귀 설계 — Placeholder와 structured attention mask.
- **아키텍처**:
  - 입력 패치 + Placeholder 토큰 (미래 위치 대리)
  - Structured self-attention mask: 과거↔미래 attention 패턴 제어
  - Tunable RoPE(Rotary Position Embedding): 시계열 주기 포착 및 horizon 적응
  - Multi-scale patch: 세밀/거친 정보 통합
  - Horizon reweighting 훈련 전략
- **손실함수**: MSE + horizon reweighting

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336/720
- Forecast horizons: 96, 192, 336, 720 (다중 horizon 동시)
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 다양한 horizon에서 일관된 성능 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 단일 모델로 varied-horizon 예측 가능한 비자기회귀 Transformer
- Placeholder + structured attention mask로 horizon 불변성 보장
- Tunable RoPE로 시계열 특화 주기/위치 인코딩
- Multi-scale patch + horizon reweighting 훈련 전략

## 5. 한계 및 후속 연구 아이디어
- 매우 긴 horizon에서의 성능 저하 가능성
- Placeholder 수 결정이 실용적 어려움

## 6. 참고
- [arXiv 2411.01842](https://arxiv.org/abs/2411.01842)
