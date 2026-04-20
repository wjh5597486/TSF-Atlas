---
title: "TimeDART: A Diffusion Autoregressive Transformer for Self-Supervised Time Series Representation"
authors: (see GitHub: Melmaphother)
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2410.05711
code_url: https://github.com/Melmaphother/TimeDART
tags: [self-supervised, diffusion, autoregressive, transformer, pre-training, representation-learning]
primary_category: category/SelfSupervised
related_categories: [category/Time-domain/Generative/Diffusion]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimeDART.pdf
pdf_source: arxiv
---

## TL;DR
TimeDART unifies causal Transformer-based global modeling and denoising diffusion-based local pattern learning in a single self-supervised pre-training framework. The autoregressive mechanism captures long-range dependencies while the diffusion process improves local accuracy, enabling superior downstream forecasting via fine-tuning on ETTh/m and Electricity datasets.

## 1. 기존 모델의 한계 / 가설
- 기존 자기지도 방법(SimMTM, TS2Vec 등)은 전역적 모델링 또는 지역적 패턴 학습 중 하나에 초점을 맞추며, 두 능력을 동시에 개발하기 어려움.
- 인과 Transformer + 확산 노이즈 제거를 통합하면 전역/지역 특성을 동시에 학습 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 인과 Transformer(전역 모델링)와 확산 기반 노이즈 제거(지역 패턴)를 자기지도 사전훈련에 통합.
- **아키텍처**:
  - **Causal Transformer**: 자기회귀적 전역 시간 의존성 모델링.
  - **Denoising Diffusion**: 지역 패턴 학습을 위한 노이즈 제거.
  - 두 컴포넌트를 결합한 사전훈련 손실.
  - Fine-tuning 단계: 다운스트림 예측/분류 적용.
- **손실함수**: 자기회귀 손실 + 확산 손실.
- **학습 전략**: Self-supervised pre-training → supervised fine-tuning.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (표준 336)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE (fine-tuning)
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electricity 등 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA (self-supervised) |
| 192     | see paper | see paper | SOTA (self-supervised) |
| 336     | see paper | see paper | SOTA (self-supervised) |
| 720     | see paper | see paper | SOTA (self-supervised) |

출처: Table of paper.

## 4. 주요 기여
- 인과 Transformer + 확산 기반 노이즈 제거의 통합 자기지도 사전훈련.
- 전역(global) + 지역(local) 시간 패턴 동시 학습.
- ETTh/m, Electricity에서 기존 자기지도 방법 대비 우수한 성능.
- 다운스트림 예측 및 분류 태스크 모두 지원.

## 5. 한계 및 후속 연구 아이디어
- 사전훈련 + 파인튜닝 2단계 학습 과정의 복잡성.
- ※ 메모: SimMTM(NeurIPS 2023)의 자기지도 방향 + 확산 모델의 결합.

## 6. 참고
- 관련 논문: SimMTM (NeurIPS 2023), TS2Vec (AAAI 2022), TimeSiam (ICML 2024)
- GitHub: https://github.com/Melmaphother/TimeDART
- arXiv: https://arxiv.org/abs/2410.05711
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/43701
