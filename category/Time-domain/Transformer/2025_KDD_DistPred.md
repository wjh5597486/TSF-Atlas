---
title: "DistPred: A Distribution-Free Probabilistic Inference Method for Regression and Forecasting"
authors: Daojun Liang et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2406.11397
code_url: https://github.com/Anoise/DistPred
tags: [time-domain, transformer, probabilistic, distribution-free, loss, channel-dependent]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_DistPred.pdf
pdf_source: arxiv
---

## TL;DR
DistPred는 proper scoring rule을 미분 가능한 형태로 변환하여 손실함수로 직접 사용하는 분포 가정 없는 확률적 예측 방법이다. iTransformer 백본을 기반으로 단일 forward pass에서 K개의 샘플을 동시에 생성하여 기존 SOTA 대비 180배 빠른 추론 속도를 달성한다. ETTh1을 포함한 11개 데이터셋에서 평가된다.

## 1. 기존 모델의 한계 / 가설
- 기존 확률적 예측 방법(diffusion, normalizing flow 등)은 특정 분포 가정(Gaussian 등)에 의존하거나 여러 번의 forward pass가 필요하여 추론이 느림.
- Proper scoring rule을 손실함수로 사용하면 분포 가정 없이 데이터 분포를 직접 학습할 수 있음.
- 저자들은 proper scoring rule을 미분 가능하게 이산화하여 single-pass 확률적 추론이 가능하다고 주장.

## 2. 제안 방법론
- **핵심 아이디어**: Proper scoring rule(분포 간 불일치를 측정하는 함수)을 미분 가능한 이산화 형태로 손실함수화하고, 단일 forward pass에서 K개 예측값(샘플)을 동시에 출력.
- **아키텍처 구성요소**:
  1. **Backbone**: iTransformer (채널 역전 Transformer) 기반.
  2. **K-Sample Output Head**: 단일 forward pass에서 K개의 독립 예측 생성 (마지막 선형 레이어를 K배로 확장).
  3. **Differentiable Proper Scoring Loss**: Continuous Ranked Probability Score(CRPS)의 이산화 버전을 손실함수로 사용.
- **손실함수**: Differentiable proper scoring rule (CRPS 이산화 버전).
- **평가 지표**: CRPS (확률적), MSE/MAE (결정론적 평균 예측 기준).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (iTransformer 기본 설정 추정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (iTransformer 기본)
- Train/Val/Test split: ETT 기본 설정
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: Differentiable proper scoring rule
- 기타: 확률적 예측 평가 (CRPS, QICE, PICP); 결정론적 MSE/MAE도 Table 3 보고

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.288 | 0.334 | iTransformer backbone |
| 192     | 0.348 | 0.371 | |
| 336     | 0.392 | 0.402 | |
| 720     | 0.437 | 0.436 | |

출처: Table 3 of paper (arxiv:2406.11397). ※ 확률적 모델이므로 CRPS도 주지표; MSE/MAE는 K 샘플의 평균으로 계산.

## 4. 주요 기여
- Proper scoring rule을 미분 가능 이산화 형태의 손실함수로 변환하는 일반적 프레임워크 제안.
- 단일 forward pass로 K개 샘플 동시 생성 → 180배 추론 속도 향상.
- 분포 가정 없이 임의 신뢰 구간 추정 가능.
- 11개 시계열 + UCI 회귀 데이터셋에서 SOTA 확률적 예측 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: K가 크면 메모리 및 추론 비용 증가; K 선택이 성능-효율 트레이드오프.
- ※ 메모: Proper scoring rule의 이산화 오차가 실제 분포 학습에 미치는 영향 분석 가능. 결정론적 LTSF 모델과 직접 비교 시 확률 모델로서의 MSE가 낮게 나타나는 이유도 흥미로운 연구 주제.

## 6. 참고
- [arxiv:2406.11397](https://arxiv.org/abs/2406.11397)
- [GitHub: Anoise/DistPred](https://github.com/Anoise/DistPred)
