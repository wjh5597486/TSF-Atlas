---
title: "This Time is Different: An Observability Perspective on Time Series Foundation Models"
authors: Ben Cohen et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.14766
code_url: ""
tags: [foundation-model, pretrained-native, decoder-only, observability, zero-shot, benchmark]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_Toto.pdf
pdf_source: arxiv
---

## TL;DR
Toto는 Datadog의 관측가능성(observability) 데이터에 특화된 151M 파라미터 디코더 전용 시계열 Foundation Model이다. 기존 TSFM 대비 4-10배 큰 사전학습 코퍼스와 새로운 대규모 벤치마크(BOOM: 350M 관측값, 2807개 시계열)를 함께 제안한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Foundation Model들은 관측가능성 데이터의 고유한 특성(불규칙성, 고차원, 시스템 메트릭 패턴)을 충분히 처리하지 못함.
- 평가 벤치마크가 작고 편향되어 실제 성능 비교가 어려움.
- 가설: 관측가능성 데이터에 특화된 아키텍처 혁신과 대규모 코퍼스로 관련 도메인 SOTA를 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 관측가능성 시계열의 특성에 맞게 설계된 디코더 전용 Foundation Model.
- **아키텍처**:
  - 현대적인 디코더 전용(decoder-only) 아키텍처.
  - 관측가능성 시계열의 고유 도전과제를 처리하는 아키텍처 혁신.
  - 사전학습 코퍼스: 관측가능성 데이터 + 오픈 데이터셋 + 합성 데이터 (기존 TSFM 대비 4-10× 규모).
- **BOOM 벤치마크**: 350M 관측값, 2807개 실제 시계열로 구성된 대규모 공개 벤치마크.
- **모델 크기**: 151M 파라미터.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 제로샷 설정
- Forecast horizons: 다양한 horizons
- Normalization: 표준
- Train/Val/Test split: 표준 split (zero-shot 평가)
- Batch size / Optimizer / LR: 사전학습 설정
- Loss function: 사전학습 손실
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather 포함 표준 벤치마크 + BOOM 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper (BOOM + 표준 벤치마크에서 SOTA) |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체 수치는 논문 참조 필요.

## 4. 주요 기여
- 관측가능성 데이터 특화 디코더 전용 TSFM(Toto, 151M) 제안.
- BOOM 대규모 벤치마크(350M 관측값, 2807개 시계열) 공개.
- 기존 TSFM 대비 4-10× 큰 사전학습 코퍼스.
- BOOM 및 표준 벤치마크에서 SOTA 제로샷 성능.
- Apache 2.0 오픈소스 공개.

## 5. 한계 및 후속 연구 아이디어
- 관측가능성 도메인 특화로 인한 범용성 제한 가능성.
- 사전학습 데이터가 Datadog 내부 데이터 포함으로 재현성 제한.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.14766
- Datadog 블로그: https://www.datadoghq.com/blog/datadog-time-series-foundation-model/
- OpenReview: https://openreview.net/forum?id=1jDAYXfcS2
