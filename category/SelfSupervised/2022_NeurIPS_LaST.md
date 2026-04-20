---
title: "Learning Latent Seasonal-Trend Representations for Time Series Forecasting"
authors: "Zhiyuan Wang, Xovee Xu, Weifeng Zhang, Goce Trajcevski, Ting Zhong, Fan Zhou"
venue: NeurIPS
year: 2022
paper_url: "https://openreview.net/forum?id=C9yUwd72yy"
code_url: "https://github.com/zhycs/LaST"
tags: [self-supervised, representation-learning, variational-inference, seasonal-trend, forecasting]
primary_category: "SelfSupervised"
related_categories: ["Decomposition/TrendSeasonal"]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: "./2022_NeurIPS_LaST.pdf"
pdf_source: "none"
---

## TL;DR
LaST learns disentangled seasonal-trend representations in the latent space using variational inference, achieving SOTA on self-supervised time series representation learning. It outperforms representation baselines (CoST) by 25.6% MSE and 22.1% MAE, and also competes with end-to-end forecasting models, demonstrating the value of learned representations.

## 1. 기존 모델의 한계 / 가설
- 기존 분해 방식(classical trend-seasonal): 고정된 휴리스틱(예: STL) 기반으로 데이터 의존적 적응 부족
- 최근 self-supervised learning (CoST, TS2Vec 등): 시계열의 구조적 특성(seasonality, trend)을 명시적으로 모델링하지 않음
- 가설: 잠재 공간(latent space)에서 seasonal과 trend를 **명시적으로 분리(disentangle)**하는 표현을 학습하면, 더 효과적인 시계열 모델링이 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
Variational inference 기반으로 seasonal z_s와 trend z_t를 독립적으로 학습. 두 가지 제약:
1. **입력 재구성**: Seasonal + trend 성분의 조합으로 원본 시계열 복원
2. **표현 분리**: Seasonal과 trend 간 상호 정보(mutual information) 최소화

### 2.2 아키텍처 구성

**2.2.1 Variational Autoencoder (VAE) 기반 프레임워크**

```
Input → Encoder → q(z_s | X), q(z_t | X)  [Seasonal, Trend 인코더]
                ↓
         Sampling: z_s ~ q(z_s|X), z_t ~ q(z_t|X)
                ↓
         Decoder → Reconstructed X
```

**2.2.2 손실 함수 (Loss)**

1. **Reconstruction Loss**
   - L_recon = ||X - Decoder(z_s, z_t)||_2^2
   - Seasonal + Trend 성분의 합으로 원본 재구성

2. **KL Divergence (VAE)**
   - L_KL_s = KL(q(z_s|X) || p(z_s))  [Seasonal]
   - L_KL_t = KL(q(z_t|X) || p(z_t))  [Trend]
   - 선행 분포(prior)는 표준 정규분포

3. **Disentanglement Loss (정보 제약)**
   - L_disentangle = I(z_s, z_t)  [상호 정보]
   - Z_s와 z_t가 독립적이 되도록 강제

4. **보조 목적함수 (Auxiliary)**
   - Seasonal 제거 시 reconstruction 성능 저하 → trend 중요성 검증
   - Trend 제거 시 reconstruction 성능 저하 → seasonal 중요성 검증

**2.2.3 예측(Forecasting)**
- 학습된 표현 z_s, z_t를 특징으로 하여 간단한 선형/MLP 디코더로 미래값 예측
- 표현 레벨에서의 학습이 예측 성능에 직접 영향

### 2.3 주요 특징
- **명시적 분해**: Seasonal과 trend를 별도 latent variable로 모델링
- **비지도 학습**: 제목 자체 (seasonal-trend)만으로 표현 발견 (레이블 불필요)
- **전이 가능**: 한 데이터셋에서 학습한 표현으로 다른 데이터셋 예측 (few-shot 등)
- **해석가능성**: z_s, z_t 분석으로 시계열의 구조 파악 가능

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96, 192 등 다양
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standardization
- Train/Val/Test split: 8:1:1
- Batch size: 32
- Optimizer: Adam, lr = 0.001
- Epochs: 300 (조기 종료 patience = 30)
- Loss function: L_recon + L_KL_s + L_KL_t + λ * L_disentangle
  - λ (disentanglement weight): 0.1 등 (hyperparameter)
- 기타: Ablation study (seasonal/trend 제거 영향 분석)

### 3.2 Results
상세 수치는 원문 Table 4, 5, Supplementary C 참고.

**주요 성과:**

**Self-supervised Representation 성능 (CoST 대비):**
- MSE: 25.6% 상대 개선
- MAE: 22.1% 상대 개선

**Forecasting 성능 (5개 데이터셋: ETTh1, ETTh2, ETTm1, ETTm2, Exchange_rate):**
- ETTh1: SOTA 또는 준SOTA (DLinear, NLinear 등과 경쟁)
- ETTm1: 강한 성능
- Multi-scale 데이터셋에서 일관된 우수성

**Ablation Study 결과:**
- Seasonal component 제거: 성능 저하 확인
- Trend component 제거: 성능 저하 확인
- 양쪽 모두 필수 → disentanglement가 유효함을 입증

| 데이터셋   | 주요 결과                                |
|----------|------------------------------------------|
| ETTh1    | 강한 성능, SOTA 근처                    |
| ETTh2    | 우수                                    |
| ETTm1    | SOTA/준SOTA                             |
| ETTm2    | 안정적 성능                             |
| Exchange | 우수 성능                               |

*(자세한 MSE/MAE 수치는 원문 Table 4, 5 참고)*

## 4. 주요 기여
- Self-supervised learning에서 seasonal-trend를 명시적으로 분리하는 첫 시도
- Disentanglement 제약을 통한 해석 가능한 표현 학습
- 표현 학습으로 end-to-end 예측 모델과 경쟁하는 성능 달성
- 5개 벤치마크 데이터셋에서 일관된 우수성 입증

## 5. 한계 및 후속 연구 아이디어
- λ (disentanglement weight) 선택이 중요하지만 데이터셋에 따라 튜닝 필요
- 매우 복합적인 계절성(예: 여러 주기)이 있는 시계열에 대한 확장 필요
- 표현의 일반화 능력을 다양한 downstream task에서 더 광범위하게 검증
- ※ 메모: Ablation study에서 seasonal/trend 제거의 영향이 명확하므로, 이를 활용한 자동 분해 전략 개발 가능

## 6. 참고
- [GitHub Repository](https://github.com/zhycs/LaST)
- OpenReview: https://openreview.net/forum?id=C9yUwd72yy
- 관련 논문: CoST (ICLR 2023), TS2Vec, DLinear, NLinear, SimMTM
- ETT Dataset: Electricity Transformer Temperature (2016-2018)
  - ETTh1/h2: Hourly (1 hour interval)
  - ETTm1/m2: 15-minute interval
  - 모두 target (oil temperature) + 6 feature channels

※ 원본 PDF 미취득: arXiv ID 미공개, NeurIPS 2022 proceedings & xoveexu.com 프리프린트만 사용 가능. 추후 arXiv 공개 시 pdf_source 업데이트.
