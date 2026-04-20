---
title: "FITS: Modeling Time Series with 10k Parameters"
authors: Zhijian Xu, Ailing Zeng, Qiang Xu
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2307.03756
code_url: https://github.com/VEWOXIC/FITS
tags: [freq-domain, fft, linear, lightweight]
primary_category: category/Freq-domain/FFT/Architecture/Linear
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_FITS.pdf
pdf_source: arxiv
---

## TL;DR
복소수 linear layer 하나로 시계열의 **주파수 도메인 보간(frequency interpolation)** 을 수행. 전체 파라미터 ~10k로 극단적으로 가볍지만 Transformer 계열과 경쟁.

## 1. 기존 모델의 한계 / 가설
- 대형 모델이 필수인가? — 저자들은 주기적 시계열은 주파수 도메인에서 소수 파라미터로 표현 가능하다고 가설.

## 2. 제안 방법론
- Input → rFFT → low-pass filter → **Complex-valued linear layer** → rFFT⁻¹.
- Amplitude·phase를 동시에 학습.
- Channel-independent, Instance Normalization(RevIN-style) 적용.

## 3. ETTh1 실험 결과

### 3.1 Setting
- Lookback = 720 (기본), Horizons = {96, 192, 336, 720}.
- Normalization: RevIN-style.
- 파라미터 수 ~10k.

### 3.2 Results
구체적 수치는 원문 Table 4 참고 (DLinear·PatchTST와 동급 수준).

## 4. 주요 기여
- 10k 파라미터로 SOTA급 성능 → TSF에서 "파라미터 vs 성능" 가정을 재검토하게 함.
- 주파수 보간이라는 극히 단순한 프레임으로 reproducibility 보장.

## 5. 참고
- 코드: https://github.com/VEWOXIC/FITS
