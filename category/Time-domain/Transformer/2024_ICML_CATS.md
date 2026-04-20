---
title: "CATS: Enhancing Multivariate Time Series Forecasting by Constructing Auxiliary Time Series as Exogenous Variables"
authors: Jiecheng Lu et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2403.01673
code_url: https://github.com/LJC-FVNR/CATS
tags: [transformer, exogenous, auxiliary-series, dual-attention, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_CATS.pdf
pdf_source: arxiv
---

## TL;DR
CATS는 원본 시계열에서 보조 시계열(ATS)을 구성하여 채널 간 공간 상관관계를 명시적 외생 변수로 활용. 연속성/희소성/다양성 원칙에 기반한 ATS 생성 + 이중 어텐션으로 23.4% MSE 감소.

## 1. 기존 모델의 한계 / 가설
- 고차원 다변량 예측에서 채널 간 정보 병목(information bottleneck) 발생.
- 기존 모델은 채널 간 상관관계를 암묵적으로만 처리하거나 완전히 무시.
- 가설: 원본 시계열에서 명시적 보조 시계열을 생성하여 외생 변수로 활용하면 예측 품질 향상.

## 2. 제안 방법론
- **핵심 아이디어**: 원본 시계열(OTS)에서 연속성/희소성/다양성 원칙으로 보조 시계열(ATS) 생성, 이중 어텐션으로 융합.
- **아키텍처**:
  - ATS 생성: 연속성(continuity), 희소성(sparsity), 다양성(variability) 원칙 적용
  - Dual Attention: OTS 특징-ATS 특징 간 공간 상관관계 별도 처리
  - 기본 예측기: 2층 MLP만으로도 SOTA 달성
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 기존 다변량 모델 대비 23.4% MSE 감소 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 보조 시계열(ATS) 구성 원칙 체계화 (연속성/희소성/다양성)
- 이중 어텐션으로 OTS-ATS 공간 상관관계 분리 처리
- 단순 MLP 백본으로도 SOTA 달성 (파라미터 효율)
- 9개 MTSF 데이터셋에서 23.4% MSE 감소

## 5. 한계 및 후속 연구 아이디어
- ATS 생성 원칙의 이론적 정당화 보강 필요
- ATS 생성 비용이 추론 시간 증가 유발

## 6. 참고
- [GitHub](https://github.com/LJC-FVNR/CATS)
- [arXiv 2403.01673](https://arxiv.org/abs/2403.01673)
