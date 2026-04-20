---
title: "Fredformer: Frequency Debiased Transformer for Time Series Forecasting"
authors: Xihao Piao et al.
venue: KDD 2024
year: 2024
paper_url: https://arxiv.org/abs/2406.09009
code_url: https://github.com/chenzRG/Fredformer
tags: [transformer, freq-domain, fft, frequency-bias, normalization, time-series-forecasting]
primary_category: category/Freq-domain/FFT/Architecture/Transformer
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_KDD_Fredformer.pdf
pdf_source: arxiv
---

## TL;DR
Fredformer는 Transformer 모델이 낮은 주파수 성분에 편향(frequency bias)되어 높은 주파수 특징을 놓치는 문제를 해결하는 주파수 균형화 Transformer이다. 다양한 주파수 대역에서 동등하게 학습하도록 설계되어 8개 데이터셋에서 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- Transformer는 에너지가 높은 저주파 성분에 편향되어 고주파 성분을 학습하지 못하는 frequency bias 문제가 있음.
- 기존 주파수 도메인 Transformer(FEDformer, Fedformer)들도 이 편향에서 자유롭지 않음.
- 주파수 대역 간 균등한 학습이 필요하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Frequency Debiased Attention (FDA) — 주파수 대역별 에너지 차이를 보정하여 균등한 주의(attention) 학습 유도.
- **아키텍처 구성요소**:
  - 시계열 입력을 FFT로 변환
  - 각 주파수 대역의 에너지 비율을 계산 후 역정규화(debiasing)
  - 주파수 공간에서 self-attention 수행
  - 역FFT로 시간 도메인으로 복원 후 예측
- **손실함수**: MSE loss
- **핵심 수식**: attention weight를 주파수 에너지의 역수로 가중치를 부여하여 편향 보정

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 또는 336 (상세는 원문 참고)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam 옵티마이저 사용; 상세 하이퍼파라미터는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 8개 데이터셋(ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, Solar)에서 기존 SOTA 대비 우수한 성능 보고. ETTh1 구체 수치는 원문 Table 참고.

출처: 원문 주요 비교 Table.

## 4. 주요 기여
- Transformer의 주파수 편향(frequency bias) 현상을 실험적으로 규명
- Frequency Debiased Attention(FDA) 메커니즘 제안
- 8개 벤치마크에서 SOTA 성능 달성
- 주파수 학습 다양성 시각화를 통한 해석 가능성 제공

## 5. 한계 및 후속 연구 아이디어
- 저자가 명시한 한계: 일부 데이터셋에서 computational overhead 증가
- ※ 메모: WaveletTransform 도메인과 결합 시 더 넓은 주파수 대역 처리 가능성 있음

## 6. 참고
- 관련 논문: FEDformer (ICML 2022), FiLM (NeurIPS 2022), FreTS
- ACM DL: https://dl.acm.org/doi/10.1145/3637528.3671928
- GitHub: https://github.com/chenzRG/Fredformer
