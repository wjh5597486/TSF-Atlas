---
title: "FreDF: Learning to Forecast in the Frequency Domain"
authors: Hao Wang et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2402.02399
code_url: https://github.com/Master-PLC/FreDF
tags: [freq-domain, fft, loss, direct-forecast, label-autocorrelation, architecture-agnostic]
primary_category: category/Freq-domain/FFT/Architecture/Linear
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_FreDF.pdf
pdf_source: arxiv
---

## TL;DR
FreDF(Frequency-enhanced Direct Forecast)는 기존 Direct Forecast(DF) 패러다임의 레이블 자기상관(label autocorrelation) 문제를 주파수 도메인에서의 학습으로 해결한다. 주파수 도메인에서 기저(basis)가 직교·독립적이어서 자기상관의 영향이 줄어들고 다양한 예측 모델과 호환되는 플러그인 방식으로 활용 가능하다.

## 1. 기존 모델의 한계 / 가설
- 현재 LTSF 모델들은 Direct Forecast(DF) 패러다임 채택 → 미래 레이블들의 상관관계(label autocorrelation) 무시
- DF의 학습 목표가 레이블 자기상관 존재 시 편향(biased)됨
- 주파수 도메인에서는 기저가 직교하여 자기상관 영향이 최소화된다는 관찰

## 2. 제안 방법론
- 핵심 아이디어: 예측 출력과 레이블 시퀀스를 주파수 도메인으로 변환하여 손실 계산
- **FreDF**: 시계열 및 레이블을 FFT 변환 후 주파수 도메인에서 MSE 최소화
- 어떤 forecasting backbone에도 플러그인 방식으로 적용 가능(아키텍처 무관)
- 이론적으로 주파수 도메인 기저의 직교성으로 DF 편향 제거 설명
- 손실함수: Frequency-domain MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 96~720
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: Frequency-domain MSE (FreDF) vs 표준 MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic 등 다양한 backbone 모델과 결합 평가

### 3.2 Results
다양한 backbone 모델(PatchTST, iTransformer, DLinear 등)에 FreDF 적용 시 일관된 성능 향상. 기존 SOTA 방법 대비 유의미한 MSE/MAE 개선. 상세 수치는 논문 Table 참고.

출처: 논문 Table (arXiv 2402.02399, ICLR 2025).

## 4. 주요 기여
- 레이블 자기상관이 Direct Forecast 목표를 편향시킴을 이론·실험으로 입증
- 주파수 도메인 기저의 직교성을 활용해 편향 제거 프레임워크 제안
- 기존 모든 forecasting 모델에 플러그인으로 적용 가능한 범용성
- 표준 벤치마크 전반에서 일관된 성능 개선

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: FFT 변환 추가로 인한 소폭 계산 오버헤드
- 확장 아이디어: Wavelet 변환 등 다른 직교 기저로의 확장

## 6. 참고
- arXiv: https://arxiv.org/abs/2402.02399
- OpenReview: https://openreview.net/forum?id=4A9IdSa1ul
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/31031
- GitHub: https://github.com/Master-PLC/FreDF
