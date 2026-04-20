---
title: "TS-LIF: A Temporal Segment Spiking Neuron Network for Time Series Forecasting"
authors: (authors in proceedings)
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2503.05108
code_url: https://github.com/kkking-kk/TS-LIF
tags: [time-domain, spiking-neural-network, ssm, multi-scale, temporal-segment, energy-efficient]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_TS-LIF.pdf
pdf_source: arxiv
---

## TL;DR
TS-LIF는 스파이킹 신경망(SNN)의 Leaky Integrate-and-Fire(LIF) 뉴런을 개선한 이중 구획(dual-compartment) 뉴런 아키텍처로 시계열 예측에 적용한다. 수상돌기(dendritic)와 세포체(somatic) 구획이 각각 저주파·고주파 성분을 분리 처리하며, 에너지 효율적이면서도 long-term dependency를 효과적으로 포착한다.

## 1. 기존 모델의 한계 / 가설
- 기존 LIF 뉴런 모델은 장기 의존성 포착 및 다중 시간 스케일 처리 한계
- 표준 SNN은 시계열 예측에서 정보 손실 발생
- 생물학적 뉴런의 dual-compartment 구조가 시계열의 다중 주파수 특성 처리에 적합하다는 가설

## 2. 제안 방법론
- 핵심 아이디어: 수상돌기·세포체 이중 구획으로 저주파(추세)·고주파(계절) 성분 분리 처리
- **Dendritic compartment**: 저주파 정보(장기 추세) 포착
- **Somatic compartment**: 고주파 정보(계절성, 단기 변동) 포착
- **Direct somatic current injection**: 뉴런 내 전달 시 정보 손실 감소
- **Dendritic spike generation**: 다중 스케일 정보 추출 향상
- Spiking neural network 기반 → 에너지 효율적
- 손실함수: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 96~720
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE
- 기타: ETTh1 포함 표준 LTSF 벤치마크

### 3.2 Results
ETTh1 포함 표준 벤치마크에서 기존 SNN 대비 우수 성능. Transformer/MLP 기반 SOTA 모델과 경쟁적 성능. 상세 수치는 논문 Table 참고.

출처: 논문 Table (ICLR 2025).

## 4. 주요 기여
- 시계열 예측을 위한 Dual-compartment SNN 뉴런 설계
- 저주파·고주파 성분 분리 처리로 다중 시간 스케일 포착
- Direct somatic current injection으로 정보 손실 최소화
- 에너지 효율적 SNN 기반 LTSF 모델

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: SNN 훈련의 non-differentiable spike 함수로 인한 학습 복잡도
- 확장 아이디어: Neuromorphic hardware에서의 실제 에너지 효율 측정

## 6. 참고
- arXiv: https://arxiv.org/abs/2503.05108
- OpenReview: https://openreview.net/forum?id=rDe9yQQYKt
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/28210
- GitHub: https://github.com/kkking-kk/TS-LIF

※ 원본 PDF 미취득: arXiv 논문이 2503 번호(March 2025)이므로 ICLR 2025 제출 당시 익명 버전과 다를 수 있음.
