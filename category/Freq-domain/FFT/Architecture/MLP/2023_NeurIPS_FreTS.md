---
title: "Frequency-domain MLPs are More Effective Learners in Time Series Forecasting"
authors: Kun Yi et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2311.06184
code_url: https://github.com/aikunyi/FreTS
tags: [freq-domain, fft, mlp, time-series-forecasting, multivariate, channel-dependent]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_FreTS.pdf
pdf_source: arxiv
---

## TL;DR
FreTS는 시계열을 주파수 도메인의 복소수로 변환한 뒤 MLP를 적용하는 단순하면서도 효과적인 아키텍처다. 주파수 도메인에서의 채널·시간 MLP가 시간 도메인 MLP보다 에너지를 더 집중적으로 학습하여 뛰어난 예측 성능을 보인다.

## 1. 기존 모델의 한계 / 가설
- 기존 MLP 기반 시계열 모델들은 시간 도메인에서만 동작하여 전역 주파수 패턴을 학습하지 못한다.
- Transformer 계열은 복잡한 구조와 높은 연산 비용을 수반한다.
- 가설: 주파수 도메인 MLP는 에너지를 소수의 주파수 성분에 집중시켜 노이즈에 강건하고 전역 패턴을 효율적으로 포착할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: FFT로 시간 도메인 입력을 복소수 주파수 표현으로 변환 후, 채널 MLP와 주파수 MLP를 적용하여 채널 간 및 주파수 간 의존성을 동시 학습.
- **아키텍처**:
  1. **DFT 변환**: 입력 시계열 $X \in \mathbb{R}^{N \times T}$를 FFT로 주파수 표현 $\hat{X} \in \mathbb{C}^{N \times T/2}$으로 변환.
  2. **Channel MLP**: 주파수 도메인에서 변수 간 의존성 학습.
  3. **Frequency MLP**: 주파수 성분 간 상호작용 학습.
  4. IDFT로 시간 도메인으로 역변환 후 예측.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.375 | 0.399 | see Table 1 |
| 192     | 0.416 | 0.420 | see Table 1 |
| 336     | 0.440 | 0.437 | see Table 1 |
| 720     | 0.465 | 0.463 | see Table 1 |

출처: Table 1 of paper.

## 4. 주요 기여
- 주파수 도메인 MLP가 시간 도메인 MLP보다 에너지 집중 및 노이즈 필터링에 유리함을 이론·실험으로 입증.
- 채널 MLP와 주파수 MLP의 결합을 통한 다변량 시계열 효과적 처리.
- 단순한 구조로 Transformer 계열 대비 경쟁력 있는 성능과 효율성 달성.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 고차 비선형 주파수 상호작용을 MLP로 완전히 포착하기 어려울 수 있음.
- ※ 메모: FiLM (ICLR 2023), FITS (ICLR 2024)와 함께 주파수 도메인 접근의 흐름을 형성. WaveletTransform 계열과의 결합 연구 가능.

## 6. 참고
- arXiv: https://arxiv.org/abs/2311.06184
- GitHub: https://github.com/aikunyi/FreTS
- 관련: FiLM, FITS, Fredformer
