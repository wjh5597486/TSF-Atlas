---
title: "TSLANet: Rethinking Transformers for Time Series Representation Learning"
authors: Emadeldeen Eldele et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2404.08472
code_url: https://github.com/emadeldeen24/TSLANet
tags: [freq-domain, adaptive-spectral, cnn, self-supervised, lightweight, time-domain]
primary_category: category/Freq-domain/LearnableFilter
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_TSLANet.pdf
pdf_source: arxiv
---

## TL;DR
TSLANet(Time Series Lightweight Adaptive Network)은 Adaptive Spectral Block으로 주파수 도메인 특징을 추출하고 Interactive Convolution Block으로 시간적 패턴을 포착하는 경량 범용 모델. Transformer 없이 다양한 시계열 태스크에서 SOTA.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델은 잡음 민감성, 계산 효율, 전체 주파수 스펙트럼 활용에 한계.
- CNN 기반 모델은 긴 시퀀스(ETTh1 등 시간 해상도)에서 장기 의존성 포착 어려움.
- 가설: 적응형 주파수 필터링 + 인터랙티브 컨볼루션으로 Transformer 없이 범용 시계열 표현 학습 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Adaptive Spectral Block(ASB) + Interactive Convolution Block(ICB) + 자기지도 학습.
- **아키텍처**:
  - ASB: FFT → 적응형 임계값 필터링 → iFFT (잡음 완화 + 주파수 특징 추출)
  - ICB: Depthwise separable conv로 시간 패턴 포착
  - 자기지도 학습으로 복잡한 시간 패턴 디코딩
- **손실함수**: MSE + 자기지도 재구성 손실

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | CNN 기반 모델 중 우수한 성능 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- Adaptive Spectral Block으로 적응형 잡음 제거 및 주파수 특징 추출
- Transformer 없이 분류, 예측, 이상탐지 다중 태스크 SOTA
- 자기지도 학습으로 범용 시계열 표현 향상
- 다양한 잡음 수준과 데이터 크기에서 강건성 확인

## 5. 한계 및 후속 연구 아이디어
- Transformer 없는 설계로 매우 장기 의존성 포착 한계
- 적응형 임계값의 학습 안정성 분석 필요

## 6. 참고
- [GitHub](https://github.com/emadeldeen24/TSLANet)
- [arXiv 2404.08472](https://arxiv.org/abs/2404.08472)
