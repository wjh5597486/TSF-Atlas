---
title: "TimePerceiver: An Encoder-Decoder Framework for Generalized Time-Series Forecasting"
authors: Jaebin Lee et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2512.22550
code_url: https://github.com/efficient-learning-lab/TimePerceiver
tags: [time-domain, transformer, encoder-decoder, generalized, interpolation, imputation, extrapolation]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TimePerceiver.pdf
pdf_source: arxiv
---

## TL;DR
TimePerceiver는 외삽(extrapolation)·내삽(interpolation)·결측치 보완(imputation)을 단일 엔코더-디코더 프레임워크로 통합하는 범용 시계열 예측 모델이다. 잠재 병목 표현(latent bottleneck representation)과 학습 가능한 쿼리 메커니즘으로 시간적·채널 간 의존성을 포착한다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 미래 외삽(예측)에만 특화되어 내삽·결측치 보완 등 다양한 시간적 예측 목적을 통합하지 못함.
- 별도 사전학습-파인튜닝 파이프라인 없이 다양한 시간적 구조를 동시에 학습하기 어려움.
- 가설: 일반화된 태스크 포뮬레이션(외삽+내삽+결측치)을 단일 엔코더-디코더로 학습하면 더 깊은 시간적 역학 이해가 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 예측 태스크를 외삽·내삽·결측치 보완으로 일반화하여 단일 엔코더-디코더로 학습.
- **아키텍처**:
  - **Encoder**: 잠재 병목 표현(Latent Bottleneck Representation)으로 입력 세그먼트 전체와 상호작용하여 시간적·채널 간 의존성 공동 포착.
  - **Decoder**: 목표 타임스텝에 대응하는 학습 가능한 쿼리(Learnable Queries)로 관련 정보 검색.
  - 일반화된 태스크 포뮬레이션: 임의의 컨텍스트 정보 기반 타임스텝 예측.
- **특성**: 별도 사전학습-파인튜닝 파이프라인 불필요. End-to-end 학습.
- **결과**: MSE 순위 1.375, MAE 순위 1.550 (다양한 LTSF 설정에서 최신 모델 대비 1위).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (ETTh1에서 패치 1개가 12시간 span)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 인스턴스 정규화
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | MSE rank 1.375 average |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 다양한 LTSF 설정에서 평균 MSE 순위 1.375 달성.

## 4. 주요 기여
- 외삽·내삽·결측치 보완을 단일 엔코더-디코더로 통합하는 일반화된 TSF 프레임워크.
- 잠재 병목 표현으로 시간적·채널 간 의존성 공동 포착.
- 학습 가능한 쿼리 메커니즘으로 목표 타임스텝 관련 정보 검색.
- ETTh1에서 12시간 주기의 쿼리 쌍(~일간 리듬) 자연스러운 학습 확인.

## 5. 한계 및 후속 연구 아이디어
- 대규모 다변량(수백 채널) 확장성 추가 검증 필요.
- 온라인 예측 환경으로의 확장.

## 6. 참고
- arxiv: https://arxiv.org/abs/2512.22550
- GitHub: https://github.com/efficient-learning-lab/TimePerceiver
- OpenReview: https://openreview.net/forum?id=RCeZ063p33
