---
title: "Enhancing Foundation Models for Time Series Forecasting via Wavelet-based Tokenization"
authors: Luca Masserano et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2412.05244
code_url: ""
tags: [freq-domain, wavelet, tokenization, foundation-model, autoregressive, zero-shot]
primary_category: category/Freq-domain/WaveletTransform/Architecture/MLP
related_categories: [category/Foundation-LLM/Pretrained-Native]
reports_etth1: false
swept_in: 2026-04-19
pdf_local: ./2025_ICML_WaveToken.pdf
pdf_source: arxiv
---

## TL;DR
WaveToken introduces a wavelet-based tokenizer for time series foundation models that decomposes input signals into time-localized frequency coefficients before quantization and autoregressive pre-training. Evaluated on 42 datasets (in-domain + zero-shot), it achieves superior generalization with only 1024 tokens, outperforming recent foundation models while being competitive with supervised models.

## 1. 기존 모델의 한계 / 가설
- 기존 foundation model의 원시값(raw-value) 토큰화는 시계열의 다중 스케일 시간-주파수 구조를 직접 포착하기 어려움.
- Trend, 희소 스파이크, 시변 주파수 패턴은 원시값 토큰화로는 학습이 어려움.
- Wavelet 계수로 토큰화하면 시간-주파수 정보를 더 효율적으로 표현하고 일반화 성능을 높일 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Wavelet 분해 → 임계값 처리(thresholding) → 양자화(quantization) → 자기회귀 모델.
- **아키텍처**:
  - **Wavelet Decomposition**: 입력 시계열을 다중 해상도 wavelet 계수로 분해.
  - **Thresholding & Quantization**: 계수를 임계값 처리 후 1024-vocabulary 양자화.
  - **Autoregressive Model**: 양자화된 wavelet 계수에 대해 자기회귀 사전훈련.
  - 42개 데이터셋 (in-domain + zero-shot) 평가.
- **손실함수**: Cross-entropy (자기회귀).
- **학습 전략**: Pre-training on large corpus.

## 3. ETTh1 실험 결과
> reports_etth1: false. WaveToken은 주로 zero-shot/in-domain 혼합 벤치마크(42개 데이터셋)로 평가됨. 표준 결정론적 ETTh1 LTSF 4-horizon 비교를 직접 보고하지 않음.

## 4. 주요 기여
- Wavelet 기반 토큰화로 시간-주파수 구조를 효율적으로 표현 (1024 vocabulary).
- 42개 데이터셋에서 최고 평균 rank (3가지 상보적 메트릭).
- Trend, 스파이크, 비정상 시계열 등 복잡한 패턴 포착.
- 기존 FM 대비 작은 vocabulary로 더 우수한 일반화.

## 5. 한계 및 후속 연구 아이디어
- Wavelet 분해 파라미터(레벨, 기저 함수) 선택이 성능에 영향.
- ※ 메모: WPMixer(AAAI 2025)의 wavelet 특징 접근을 foundation model pre-training에 적용.

## 6. 참고
- 관련 논문: WPMixer (AAAI 2025), Sundial (ICML 2025), MOIRAI (ICML 2024)
- arXiv: https://arxiv.org/abs/2412.05244
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/46131
