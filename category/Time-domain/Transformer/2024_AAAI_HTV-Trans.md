---
title: "Considering Nonstationary within Multivariate Time Series with Variational Hierarchical Transformer for Forecasting"
authors: Zehao Liu et al.
venue: AAAI
year: 2024
paper_url: https://arxiv.org/abs/2403.05406
code_url: https://github.com/flare200020/HTV_Trans
tags: [time-domain, transformer, variational, non-stationary, hierarchical, multivariate]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_HTV-Trans.pdf
pdf_source: arxiv
---

## TL;DR
HTV-Trans(Hierarchical Time series Variational Transformer)는 다변량 시계열의 비정상성(non-stationarity)과 확률성(stochasticity)을 동시에 고려하는 변분 생성 동적 모델이다. 계층적 확률 생성 모듈을 통해 비정상 정보를 잠재 변수로 복원하고 이를 Transformer와 결합하여 시간적 의존성을 모델링한다.

## 1. 기존 모델의 한계 / 가설
- 기존 정상화(stationarization) 방법들(NS-Transformer, RevIN 등)은 학습 시 비정상 특성을 제거하여, 실제 테스트 시 분포 변화(distribution shift)에 취약함.
- 정상화 후 원래의 비정상 정보가 소실되어 복잡한 분포의 MTS 예측이 어려움.
- 가설: 비정상 정보를 잠재 변수로 명시적으로 모델링하면, 더 강건하고 표현력 있는 MTSF 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 비정상성을 제거하는 대신 계층적 변분 모듈로 비정상 정보를 잠재 표현에 담아 Transformer에 제공.
- **HTV-Trans 구성요소**:
  1. **Hierarchical Probabilistic Generative Module**: 계층적 VAE 구조로 MTS의 비정상 특성과 확률적 동적을 여러 스케일에서 포착.
  2. **Latent Variable Injection**: 생성된 잠재 변수를 Transformer 인코더에 주입하여 비정상 정보를 시간적 의존성 학습에 반영.
  3. **Transformer Backbone**: 잠재 변수가 보강된 시계열로 표준 인코더-디코더 어텐션.
- 손실함수: ELBO (재구성 손실 + KL divergence).
- 학습 전략: 표준 지도학습 + 변분 추론.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정 (논문 참조)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 비정상 정보를 잠재 변수로 처리 (별도 정규화 없음)
- Train/Val/Test split: 표준 ETTh1 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: ELBO
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, ECL, Exchange, Traffic 등 다양한 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | NS-Transformer, Stationary 대비 우수 |
| 192     | see paper | see paper | 경쟁적 |
| 336     | see paper | see paper | 경쟁적 |
| 720     | see paper | see paper | 경쟁적 |

출처: Table 1/2 of paper (AAAI 2024 Proceedings, vol.38). 다양한 데이터셋에서 HTV-Trans의 우수성 주장.

## 4. 주요 기여
- 다변량 시계열 예측에서 비정상성과 확률성을 동시에 처리하는 계층적 변분 프레임워크 제안.
- 비정상 정보를 제거하지 않고 잠재 변수로 명시적으로 인코딩하여 Transformer에 전달.
- 분포 변화에 강건한 확률적 예측 모델.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 변분 추론의 계산 복잡도 증가.
- ※ 메모: 비정상 잠재 변수를 확산 모델로 생성하는 하이브리드 방향 탐색 가능.
- ※ 메모: HTV-Trans의 계층 수를 데이터 특성에 따라 자동 결정하는 방법 연구 필요.

## 6. 참고
- arXiv: https://arxiv.org/abs/2403.05406
- GitHub: https://github.com/flare200020/HTV_Trans
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/29483
