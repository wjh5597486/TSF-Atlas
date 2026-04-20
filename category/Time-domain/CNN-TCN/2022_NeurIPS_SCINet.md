---
title: "SCINet: Time Series Modeling and Forecasting with Sample Convolution and Interaction"
authors: "Liu Minhao et al."
venue: NeurIPS
year: 2022
paper_url: "https://arxiv.org/abs/2106.09305"
code_url: "https://github.com/cure-lab/SCINet"
tags: [time-domain, cnn, tcn, interaction, sample-convolution, forecasting]
primary_category: "Time-domain/CNN-TCN"
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: "./2022_NeurIPS_SCINet.pdf"
pdf_source: arxiv
---

## TL;DR
SCINet proposes a hierarchical recursive downsample-convolve-interact architecture that treats time series as a special sequence, enabling efficient temporal feature extraction and forecasting. The model achieves significant improvements over existing RNN/Transformer baselines across multiple benchmarks including ETTh1.

## 1. 기존 모델의 한계 / 가설
- RNN 기반 모델: 긴 시계열에서 gradient vanishing 문제
- Transformer 기반 모델: 이차 시간복잡도로 인한 계산 비효율
- 핵심 가설: 시계열의 특수한 특성(downsampling 후 두 부분수열로 분해해도 시간 관계 보존)을 활용하면 더 효율적인 모델링이 가능

## 2. 제안 방법론

### 2.1 핵심 아이디어
시계열을 특수한 수열로 인식하여 sample convolution과 interaction을 통해 계층적으로 특징을 추출.

### 2.2 아키텍처 구성

**주요 블록: Downsample-Convolve-Interact (DCI) 구조**
- Input을 downsampling으로 두 개 부분수열로 분해 (even indices, odd indices)
- 각 부분수열에 다양한 convolutional filters로 시간 특징 추출
- 추출된 features를 interaction layer에서 결합하여 종합 학습

**계층 구조**
- 첫 부분은 downsampling으로 granularity를 점진적으로 증가시킴
- 마지막 계층은 upsampling으로 원래 길이로 복원
- Skip connection을 통해 멀티 스케일 정보 통합

### 2.3 주요 특징
- 계산 복잡도: O(T) (선형), Transformer 대비 O(T²) 개선
- 수용장(receptive field)이 exponential하게 증가
- 메모리 효율성: 계층별 downsampling으로 중간 feature map 크기 감소

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standardization (mean, std)
- Train/Val/Test split: Standard split (按 8:1:1 or similar)
- Batch size: Mentioned as 32 in related works
- Optimizer: Adam
- Loss function: MSE
- 기타: 5 runs로 결과 보고, standard deviation 약 2~3%

### 3.2 Results
실험 결과 상세 수치는 원본 논문 Table 3 참고. 주요 성과:
- SCINet은 RNN/Transformer 계열 모델 대비 39.89% 평균 MSE 개선 달성
- ETTh1/ETTh2/ETTm1/ETTm2 모두에서 SOTA 또는 준SOTA 성능
- 각 horizon별로 안정적인 성능 유지

| Horizon | 성과                          |
|---------|-------------------------------|
| 96      | 주요 경쟁 모델(N-Beats 등) 대비 우수 |
| 192     | 동상 수준의 강한 성능            |
| 336     | SOTA 근처                    |
| 720     | SOTA 달성                    |

*(자세한 수치는 원문 Table 3 참고)*

## 4. 주요 기여
- 시계열의 특수한 구조(downsampling 불변성)를 활용한 새로운 CNN 아키텍처 제시
- 선형 복잡도로 장시간 시계열 처리 가능
- RNN과 Transformer 대비 계산 효율성과 성능의 trade-off 우수
- 광범위한 실험으로 CNN 기반 모델의 유효성 재입증

## 5. 한계 및 후속 연구 아이디어
- 상대적으로 작은 모델 크기로 인한 representation 제약 가능성
- 비정상(non-stationary) 시계열에 대한 강건성은 향후 개선 필요
- ※ 메모: Sample convolution의 receptive field 설계가 중요하며, downsampling factor 선택에 민감할 수 있음

## 6. 참고
- [GitHub Repository](https://github.com/cure-lab/SCINet)
- 관련 논문: N-Beats, Transformer, Informer, Autoformer
- arXiv preprint: 2106.09305 (2021년 6월 공개, NeurIPS 2022 수용)
