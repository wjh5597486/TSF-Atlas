---
title: "TSMixer: Lightweight MLP-Mixer Model for Multivariate Time Series Forecasting"
authors: Vijay Ekambaram et al.
venue: KDD 2023
year: 2023
paper_url: https://arxiv.org/abs/2306.09364
code_url: https://github.com/ibm-granite/granite-timeseries-patchtsmixer
tags: [mlp, mixer, channel-independent, channel-dependent, multivariate, lightweight, patch]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/ChannelStrategy/ChannelIndependent, category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_KDD_TSMixer.pdf
pdf_source: arxiv
---

## TL;DR
TSMixer는 MLP-Mixer 아키텍처를 시계열에 적용한 경량 모델로, Channel-Independent(CI-TSMixer)와 Channel-Dependent(IC-TSMixer) 두 가지 백본을 제공한다. State-of-the-art MLP/Transformer 대비 8-60% 예측 성능 향상, 2-3배 메모리/속도 효율을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 MTSF 모델은 높은 계산 비용으로 인해 실용적 배포에 어려움이 있음.
- 단순한 MLP Mixer 구조가 시계열의 시간적/채널 간 상관관계를 효율적으로 학습할 수 있다는 가설.
- Channel-Independent와 Channel-Dependent 전략을 모두 지원하는 통합 MLP 백본이 필요함.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 패치(patch)를 토큰으로 사용하는 MLP-Mixer 아키텍처로 시간적 패턴과 채널 간 상관관계를 교대로 학습.
- **아키텍처 구성요소**:
  - **CI-TSMixer (Channel-Independent)**: PatchTST에서 영감받아 채널 간 가중치 공유; 채널 수와 무관한 파라미터 효율
  - **IC-TSMixer (Inter-Channel)**: 채널 간 의존성 명시적 모델링
  - Patch-based tokenization: 시계열을 겹치는/겹치지 않는 패치로 분할
  - Time-mixing과 Channel-mixing 레이어 교대 적용
- **손실함수**: MSE
- **학습 전략**: 사전학습 후 미세조정 (CI-TSMixer는 크로스-데이터셋 사전학습 가능)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 512 (사전학습) / 96 (표준)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. ETTh1 lookback=512, h=96 기준 MSE≈0.37 (HuggingFace PatchTSMixer 기반). 기존 MLP 대비 8-60% 성능 향상, Transformer 대비 1-2% 우세하면서 2-3배 효율 개선 보고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- 시계열 예측을 위한 MLP-Mixer 아키텍처 설계 및 검증
- CI/IC 두 가지 채널 전략을 통합하는 유연한 백본 제공
- 사전학습을 통한 크로스-데이터셋 전이학습 지원
- 2-3배 메모리/속도 효율로 실용적 배포 가능성 제시

## 5. 한계 및 후속 연구 아이디어
- Patch 길이와 stride에 대한 하이퍼파라미터 민감도
- 채널 수가 매우 많은 경우(IC-TSMixer) 확장성 문제
- ※ 메모: IBM Granite Foundation Model의 기반이 됨; HuggingFace에서 PatchTSMixer로 공개

## 6. 참고
- 관련 논문: PatchTST (ICLR 2023), TimeMixer (ICLR 2024), DLinear (AAAI 2023)
- ACM DL: https://dl.acm.org/doi/10.1145/3580305.3599533
- arxiv: https://arxiv.org/abs/2306.09364
- HuggingFace: https://huggingface.co/ibm-granite/granite-timeseries-patchtsmixer
