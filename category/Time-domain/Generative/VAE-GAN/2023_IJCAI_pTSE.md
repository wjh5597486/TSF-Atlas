---
title: "pTSE: A Multi-model Ensemble Method for Probabilistic Time Series Forecasting"
authors: Yunyi Zhou et al.
venue: IJCAI 2023
year: 2023
paper_url: https://arxiv.org/abs/2305.11304
code_url: 
tags: [probabilistic, ensemble, hmm, distribution, multivariate, time-domain]
primary_category: category/Time-domain/Generative/VAE-GAN
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_IJCAI_pTSE.pdf
pdf_source: arxiv
---

## TL;DR
pTSE는 Hidden Markov Model(HMM) 기반 다중 모델 분포 앙상블(distribution ensemble) 방법으로, 서로 다른 확률 분포를 가정하는 예측 모델들의 출력을 결합하여 확률적 시계열 예측의 강건성과 정확도를 향상시킨다. ETTh1을 포함한 벤치마크에서 단일 멤버 모델 및 기존 앙상블 방법을 능가한다.

## 1. 기존 모델의 한계 / 가설
- 확률적 시계열 모델들은 서로 다른 분포 가정(정규분포, 학생-t 등)을 사용하여 분포를 직접 평균할 수 없음.
- 입력 시계열의 특성에 따라 최적 모델이 다르므로 단일 모델 선택이 어려움.
- HMM으로 시계열 상태를 모델링하여 상태에 따른 멤버 모델 가중치를 동적으로 결정할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: HMM으로 시계열의 잠재 상태(latent state)를 학습하고, 각 상태에서의 멤버 모델 성능을 기반으로 분포 앙상블 가중치를 계산.
- **아키텍처 구성요소**:
  - HMM: 시계열의 잠재 상태 시퀀스 추정
  - 멤버 모델 독립적 설계: 어떤 확률적 예측 모델도 멤버로 사용 가능
  - 상태 기반 가중치 계산: 각 잠재 상태에서의 멤버 모델 성능으로 가중치 결정
  - 분포 앙상블: 가중합으로 최종 예측 분포 생성
- **손실함수**: CRPS (Continuous Ranked Probability Score), NLL
- **학습 전략**: 두 단계 - (1) 멤버 모델 학습, (2) HMM 기반 앙상블 가중치 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 확률적 예측 horizons
- Normalization: 상세는 원문 참고
- Train/Val/Test split: ETTh1, ETTh2, ETTm1, ETTm2 등 표준 벤치마크
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: CRPS / NLL

### 3.2 Results
> 상세는 원문 Table 참고. 단일 멤버 모델 및 경쟁 앙상블 방법 대비 우수한 CRPS/NLL 성능 보고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- 확률적 TSF에 특화된 다중 모델 분포 앙상블 프레임워크 제안
- HMM 기반 시계열 상태 모델링으로 동적 가중치 결정
- 모델 독립적 설계: 임의의 확률적 예측 모델을 멤버로 사용 가능
- ETTh1 포함 표준 벤치마크에서 SOTA 앙상블 방법 능가

## 5. 한계 및 후속 연구 아이디어
- HMM의 잠재 상태 수 선택 기준 미명확
- 포인트 예측(MSE 기준)이 아닌 확률적 지표에 집중; MSE 비교 어려움
- ※ 메모: 확률적 예측 앙상블은 RATD, MG-TSD 등 diffusion 기반 확률적 모델과 상호보완적

## 6. 참고
- 관련 논문: MG-TSD (ICLR 2024), TimeGrad (ICML 2021)
- IJCAI 2023 proceedings: https://www.ijcai.org/proceedings/2023/521
- arxiv: https://arxiv.org/abs/2305.11304
