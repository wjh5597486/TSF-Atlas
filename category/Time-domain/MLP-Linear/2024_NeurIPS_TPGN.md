---
title: "PGN: The RNN's New Successor is Effective for Long-Range Time Series Forecasting"
authors: Tianlong Chen et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2409.17703
code_url: https://github.com/Water2sea/TPGN
tags: [rnn, gated-network, time-domain, mlp, dual-branch, long-range]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_TPGN.pdf
pdf_source: arxiv
---

## TL;DR
PGN(Parallel Gated Network)은 RNN의 순차적 정보 전파 경로를 O(1)로 단축하는 새 패러다임. Temporal PGN(TPGN)은 장기/단기 두 브랜치로 시계열 정보를 통합하여 SOTA를 달성.

## 1. 기존 모델의 한계 / 가설
- RNN은 정보 전파 경로가 O(T)로 장기 의존성 포착에 취약하고 병렬화 불가.
- Transformer는 O(T²) 복잡도로 긴 시퀀스에 비효율.
- 가설: O(1) 전파 경로를 가진 게이트 메커니즘으로 RNN의 순차 의존성 한계 극복 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Historical Information Extraction(HIE) 레이어 + 게이트 메커니즘으로 O(1) 정보 전파.
- **아키텍처 (TPGN)**:
  - Branch 1 (Long-term): PGN으로 장기 주기 패턴 + 로컬 특성 보존
  - Branch 2 (Short-term): 패치 기반 글로벌 표현 집약
  - HIE 레이어: 이전 타임스텝 정보를 직접 추출 (순차 의존 없음)
  - 게이트 메커니즘으로 장기/단기 정보 선택적 융합
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 5개 벤치마크 SOTA |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- O(1) 정보 전파 경로를 가진 PGN 패러다임 제안
- 장기/단기 듀얼 브랜치 TPGN 프레임워크
- 5개 벤치마크에서 SOTA 및 높은 효율성
- RNN 대비 병렬화 가능한 훈련

## 5. 한계 및 후속 연구 아이디어
- 이론적 표현력 분석 미흡
- 매우 긴 시퀀스(>5000)에서의 성능 미검토

## 6. 참고
- [GitHub](https://github.com/Water2sea/TPGN)
- [arXiv 2409.17703](https://arxiv.org/abs/2409.17703)
