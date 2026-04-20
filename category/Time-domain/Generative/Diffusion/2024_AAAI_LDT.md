---
title: "Latent Diffusion Transformer for Probabilistic Time Series Forecasting"
authors: Shibo Feng et al.
venue: AAAI
year: 2024
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/29085
code_url: ""
tags: [diffusion, transformer, latent-space, probabilistic, multivariate]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_LDT.pdf
pdf_source: arxiv
---

## TL;DR
LDT(Latent Diffusion Transformer)는 고차원 다변량 시계열 예측을 잠재 공간 시계열 생성 문제로 변환하는 확률적 예측 프레임워크이다. 통계 인식 오토인코더로 다변량 타임스탬프 패턴을 간결한 잠재 표현으로 압축하고, 이 잠재 공간에서 확산(diffusion) 기반 생성 모델로 예측 분포를 학습한다.

## 1. 기존 모델의 한계 / 가설
- 기존 확산 기반 TSF 모델(TimeGrad, CSDI 등)은 고차원 원본 공간에서 확산 과정을 수행하여 계산 비용이 크고 수렴이 느림.
- 결정론적(deterministic) 모델들은 불확실성 정량화 불가.
- 가설: 잠재 공간에서 확산 과정을 수행하면 계산 효율과 예측 품질을 동시에 향상시킬 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 잠재 공간(latent space)에서 확산 모델을 적용하여 고차원 MTSF를 효율적으로 수행.
- **LDT 구성요소**:
  1. **Statistics-Aware Autoencoder**: 대칭 구조의 오토인코더로 다변량 타임스탬프의 통계적 패턴(평균, 분산)을 인식하면서 잠재 표현으로 압축.
  2. **Diffusion-based Conditional Generator**: 압축된 잠재 공간에서 과거 조건부 확산 과정으로 미래 분포 생성.
  3. **Transformer backbone**: 잠재 공간의 시간적 의존성 모델링에 Transformer 사용.
- 손실함수: ELBO (Evidence Lower Bound) 기반 확산 손실.
- 학습 전략: 2단계 학습 (오토인코더 선학습 → 확산 모델 학습).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 통계 인식 정규화 (Statistics-Aware)
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: Diffusion ELBO + 재구성 손실
- 기타: 확률적 예측 지표 (CRPS, NLL 등) 사용

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | CSDI, TimeGrad 대비 우수 |
| 192     | see paper | see paper | 경쟁적 |
| 336     | see paper | see paper | 경쟁적 |
| 720     | see paper | see paper | 경쟁적 |

출처: Table 1/2 of paper (AAAI 2024 Proceedings, vol.38 no.11, pp.11979-11987).
ETTh1에서 MSE 26.2% 개선 보고(0.471 vs 0.638 비교 대비).

## 4. 주요 기여
- MTSF에 잠재 확산 변환기(Latent Diffusion Transformer) 도입.
- 통계 인식 오토인코더로 압축 효율과 표현 품질을 동시에 향상.
- 원본 공간 대비 낮은 계산 비용으로 고품질 확률적 예측 달성.
- 결정론적 및 확률적 지표 모두에서 경쟁적 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 2단계 학습으로 인한 최적화 복잡성.
- ※ 메모: VQ-VAE 기반 이산 잠재 공간 적용 가능성 탐색 필요.
- ※ 메모: Flow matching 기반 생성 모델로 교체 시 속도 향상 기대.

## 6. 참고
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/29085
- ACL Anthology/dl.acm.org: https://dl.acm.org/doi/10.1609/aaai.v38i11.29085
- ※ 원본 PDF 미취득: 공개 arXiv 버전 없음. AAAI proceedings에서 접근 가능.
