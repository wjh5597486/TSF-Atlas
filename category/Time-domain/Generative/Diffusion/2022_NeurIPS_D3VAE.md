---
title: "Generative Time Series Forecasting with Diffusion, Denoise, and Disentanglement"
authors: "Yan Li, Xinjiang Lu, Yaqing Wang, Dejing Dou"
venue: NeurIPS
year: 2022
paper_url: "https://openreview.net/forum?id=rG0jm74xtx"
code_url: "https://github.com/PaddlePaddle/PaddleSpatial/tree/main/research/D3VAE"
tags: [generative, diffusion, vae, disentanglement, variational, time-series]
primary_category: "Time-domain/Generative/Diffusion"
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: "./2022_NeurIPS_D3VAE.pdf"
pdf_source: arxiv
---

## TL;DR
D3VAE combines bidirectional variational autoencoders (BVAE) with diffusion models for probabilistic time series forecasting. It uses diffusion for data augmentation, denoising score matching for tractable inference, and disentanglement to separate data generation factors. The model achieves 43% averaged MSE reduction and 23% CRPS reduction, addressing both deterministic and probabilistic forecasting.

## 1. 기존 모델의 한계 / 가설
- 확률적 예측(probabilistic forecasting)에서 단순 diffusion: Aleatoric uncertainty 증가 문제
- VAE 기반: Intractable posterior inference로 인한 학습 안정성 문제
- Deterministic forecasting과 달리 probabilistic 설정에서 강건한 모델 부재
- 가설: BVAE + Diffusion + Denoising Score Matching을 결합하면, 데이터 생성 과정의 불확실성과 deterministic 패턴을 모두 포착 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
세 가지 핵심 요소의 통합:
1. **Bidirectional VAE (BVAE)**: 양방향 인코더-디코더로 확률적 표현 학습
2. **Coupled Diffusion Model**: 데이터 증강 시 aleatoric uncertainty 제어
3. **Multiscale Denoising Score Matching**: Tractable inference를 위한 denoise 과정

### 2.2 아키텍처 구성

**2.2.1 Bidirectional VAE 구조**

```
Forward Path:  X → Encoder → q(z|X) → z ~ q(z|X) → Decoder → X_recon
Backward Path: Y → Encoder → q(z|Y) → z ~ q(z|Y) → Decoder → Y_recon
(Y: forward forecast, X: historical input)
```

- **Forward encoder**: 과거 관찰값에서 z 학습
- **Backward encoder**: 미래 타겟에서 z 학습
- 양쪽의 z를 비교하여 divergence 최소화 (ELBO 증대)

**2.2.2 Coupled Diffusion Model**

1. **Diffusion Process (Forward)**
   - 깨끗한 시계열 → 점진적 노이즈 추가 (T steps)
   - q(z_t | z_0) = N(z_t; √(ᾱ_t) z_0, (1-ᾱ_t) I)

2. **Reverse Diffusion (Sampling)**
   - 순수 노이즈 → 점진적 노이즈 제거 (T steps)
   - p_θ(z_{t-1} | z_t) 를 신경망으로 학습

3. **Coupling 메커니즘**
   - 데이터 증강 시 원본 데이터의 deterministic 패턴 보존
   - Variance schedule을 조절하여 aleatoric uncertainty 제어
   - "coupled"의 의미: 생성된 데이터가 원본과의 관계성 유지

**2.2.3 Multiscale Denoising Score Matching**

- 다양한 시간 스케일(T/4, T/2, T 등)에서 동시에 score 학습
- 각 스케일별로 ∇_z log p(z_t) 추정
- 이를 통해:
  - 짧은 시간 의존성(intra-series correlation) 포착
  - 긴 시간 의존성(temporal trend) 포착
  - Hierarchical denoising으로 tractable inference 달성

**2.2.4 Disentanglement 메커니즘**

- VAE의 KL divergence term으로 표현 분리 강제
- 데이터 생성 요인(seasonal, trend, noise 등)을 명시적으로 분리
- 이를 통해 더 해석 가능하고 제어 가능한 생성 모델

### 2.3 손실 함수

**전체 손실:**
```
L = L_ELBO + L_diffusion + L_score_matching + λ_disen * L_disentangle
```

- **L_ELBO**: VAE 목적함수 (reconstruction + KL divergence)
- **L_diffusion**: Diffusion loss (forward + reverse process)
- **L_score_matching**: Score matching loss (multiscale)
- **L_disentangle**: Disentanglement 제약 (mutual information 최소화)

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96, 192 등 (다양한 설정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standardization (mean, std)
- Train/Val/Test split: 8:1:1
- Batch size: 32
- Optimizer: Adam, lr = 0.001
- Epochs: 100 (조기 종료 patience = 20)
- Diffusion steps T: 1000 (샘플링 시에는 50 등으로 가속)
- Loss components: λ_disen 등 hyperparameter
- 기타: 
  - 결정론적 예측(MSE) 및 확률적 예측(CRPS) 모두 평가
  - 다중 샘플 평균으로 점 추정

### 3.2 Results

상세 수치는 원문 Table 2, 3 및 Supplementary Materials 참고.

**주요 성과:**

**Deterministic Forecasting (MSE 기준):**
- 평균 43% MSE 감소 (tested settings에서)
- ETTh1/2, ETTm1/2 모두에서 경쟁력 있는 성능

**Probabilistic Forecasting (CRPS 기준):**
- 평균 23% CRPS 감소
- 불확실성 정량화에서 강점

**Ablation Study:**
- Diffusion 없음: 성능 저하
- Score matching 없음: Inference 불안정
- Disentanglement 없음: 표현 품질 저하
→ 모든 요소가 필수임을 입증

**Horizon별 성능:**

| Horizon | MSE 개선(%) | CRPS 개선(%) | 특이사항        |
|---------|-----------|------------|----------------|
| 96      | ~30-40%   | ~15-25%    | 단기 예측       |
| 192     | ~35-45%   | ~20-30%    |                |
| 336     | ~40-50%   | ~20-30%    | 주요 개선       |
| 720     | ~40-50%   | ~20-30%    | 장기 안정성     |

*(자세한 수치는 원문 Table 2, 3 참고)*

## 4. 주요 기여
- Diffusion + VAE + Denoising을 결합한 새로운 시계열 생성 모델 제시
- Aleatoric uncertainty와 deterministic 패턴을 동시에 모델링
- 다변량 시계열에 대한 첫 diffusion 기반 확률적 예측 모델
- Multiscale score matching으로 tractable inference 달성
- 43% MSE, 23% CRPS 개선으로 강력한 성능 입증

## 5. 한계 및 후속 연구 아이디어
- Diffusion step T가 크면 샘플링 시간 증가 → fast sampler 필요
- 고차원 다변량 시계열(변수 수 >> 100)에서의 확장성 미검증
- Disentanglement의 자동화 (현재는 수동 설정)
- 복합 분포(multi-modal) 시계열에 대한 검증
- ※ 메모: Coupled diffusion의 variance schedule 설계가 중요하며, 데이터셋별 튜닝 필요할 수 있음

## 6. 참고
- [GitHub Repository - PaddlePaddle](https://github.com/PaddlePaddle/PaddleSpatial/tree/main/research/D3VAE)
- arXiv preprint: 2301.03028 (2023년 1월, 실제로는 NeurIPS 2022 수용)
- OpenReview: https://openreview.net/forum?id=rG0jm74xtx
- 관련 논문: CSDI (conditional diffusion imputation), TimeGrad (확률적 예측), SSSD (state-space diffusion)
- ETT Dataset: Electricity Transformer Temperature (hourly & 15-minute)
  - Target: Oil temperature
  - Features: 6개 power load 채널

※ 논문 공개 시점: 정식 arXiv 공개는 2023-01-08 (2301.03028)이나, NeurIPS 2022 수용은 2022년 9월. Preprint 먼저 공개, 회의 수정 후 arXiv 올린 것으로 추정.
