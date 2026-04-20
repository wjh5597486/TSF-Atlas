---
title: "TimeEmb: A Lightweight Static-Dynamic Disentanglement Framework for Time Series Forecasting"
authors: Mingyuan Xia et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2510.00461
code_url: https://github.com/showmeon/TimeEmb
tags: [time-domain, mlp-linear, decomposition, normalization, frequency-domain, lightweight]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TimeEmb.pdf
pdf_source: arxiv
---

## TL;DR
TimeEmb은 시계열을 시불변(time-invariant) 성분과 시변(time-varying) 성분으로 분리하는 경량 프레임워크다. 글로벌 임베딩 모듈(시불변)과 주파수 도메인 필터링(시변)을 결합하여, 0.9M 파라미터만으로 iTransformer·FEDformer 대비 5배 이상 경량화하면서 SOTA 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 시계열의 시간적 비정상성(non-stationarity)은 시불변 패턴과 시변 패턴이 혼재하여 발생.
- 기존 모델들은 이 두 성분을 명시적으로 분리하지 않아 시간적 비정상성 처리에 한계.
- 가설: 시불변 성분을 글로벌 임베딩으로, 시변 성분을 주파수 필터링으로 분리 처리하면 경량화와 성능 향상을 동시에 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 시불변 + 시변 이중 성분 분리 처리.
- **아키텍처**:
  - **Global Embedding Module (시불변 성분)**: 시간에 걸쳐 일관된 장기 패턴을 학습 가능한 임베딩으로 캡처.
  - **Frequency-Domain Filtering Module (시변 성분)**: 전체 스펙트럼 분석에서 영감을 받은 효율적 주파수 도메인 필터링.
  - 두 성분을 합산하여 최종 예측.
- **파라미터 효율성**: 0.9M 파라미터, iTransformer·FEDformer 대비 5× 이상 적음.
- **통합성**: 기존 TSF 모델과 쉽게 통합 가능.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (표준 LTSF 설정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 인스턴스 정규화(표준)
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 다수 실제 데이터셋 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 최소 파라미터(0.9M)로 SOTA 달성이 핵심 주장.

## 4. 주요 기여
- 시불변·시변 이중 성분 분리를 통한 시간적 비정상성 처리.
- 글로벌 임베딩(시불변) + 주파수 도메인 필터링(시변)의 경량 프레임워크.
- 0.9M 파라미터로 SOTA 달성 (iTransformer·FEDformer 대비 5× 경량).
- 기존 TSF 모델과 간단한 통합 지원.

## 5. 한계 및 후속 연구 아이디어
- 시불변·시변 경계 설정의 하이퍼파라미터 의존성.
- 고차원 다변량(수백 채널)에서의 확장성 검증 필요.

## 6. 참고
- arxiv: https://arxiv.org/abs/2510.00461
- GitHub: https://github.com/showmeon/TimeEmb
- OpenReview: https://openreview.net/forum?id=sLfMvrkn6T
