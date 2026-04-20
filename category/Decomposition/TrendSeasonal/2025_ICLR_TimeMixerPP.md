---
title: "TimeMixer++: A General Time Series Pattern Machine for Universal Predictive Analysis"
authors: Shiyu Wang et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2410.16032
code_url: https://github.com/kwuking/TimeMixer
tags: [time-domain, mlp, decomposition, multi-scale, multi-resolution, universal]
primary_category: category/Decomposition/TrendSeasonal
related_categories:
  - category/Time-domain/MLP-Linear
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_TimeMixerPP.pdf
pdf_source: arxiv
---

## TL;DR
TimeMixer++는 multi-resolution time imaging(MRTI), time image decomposition(TID), multi-scale mixing(MCM), multi-resolution mixing(MRM)을 결합한 범용 시계열 패턴 머신(TSPM)이다. Long-term forecasting, classification, anomaly detection, imputation 등 8가지 분석 태스크에서 SOTA를 달성하며, ICLR 2025 Oral로 발표되었다.

## 1. 기존 모델의 한계 / 가설
- 기존 TimeMixer(ICLR 2024)는 분해 기반 멀티스케일 믹싱을 제안했지만 단일 태스크(forecasting) 위주
- 시계열 데이터의 다양한 분석 태스크(forecasting, classification, AD, imputation)를 하나의 아키텍처로 처리하기 어렵다는 문제
- 서로 다른 해상도(resolution)에서의 시간-주파수 패턴을 동시에 추출하고 통합하는 범용 프레임워크 부재

## 2. 제안 방법론
- 핵심 아이디어: 다중 스케일·다중 해상도의 "시간 이미지(time image)"로 변환 후 2D 분해 및 믹싱
- **MRTI(Multi-Resolution Time Imaging)**: 멀티스케일 시계열을 다중 해상도 2D 이미지로 변환
- **TID(Time Image Decomposition)**: dual-axis attention으로 시간 이미지에서 계절(seasonal)·추세(trend) 패턴 추출
- **MCM(Multi-scale Mixing)**: 서로 다른 스케일의 패턴을 계층적으로 집계
- **MRM(Multi-resolution Mixing)**: 모든 해상도의 표현을 적응적으로 통합
- 손실함수: 태스크별 MSE/Cross-Entropy 등 표준 목적함수

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96~720 (태스크별 상이)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 ETT 분할 (12:4:4 months)
- Batch size / Optimizer / LR: 논문 부록 참고
- Loss function: MSE
- 기타: 8가지 TSF 벤치마크 중 ETTh1/ETTh2/ETTm1/ETTm2/Weather/Electricity/Traffic 포함

### 3.2 Results
ETTh1 포함 8개 표준 벤치마크에서 평가. iTransformer 대비 평균 MSE 7.3% 감소(Electricity), TimeMixer 대비 ETT에서 평균 MSE 4.9% 감소. 상세 수치는 논문 Table 2 참고.

출처: Table 2 of paper (arXiv 2410.16032).

## 4. 주요 기여
- 단일 unified 아키텍처로 8가지 시계열 분석 태스크를 처리하는 범용 TSPM 제안
- Multi-resolution 2D time imaging으로 시간-주파수 도메인 정보 동시 포착
- Dual-axis attention 기반 TID로 seasonal/trend 패턴 분리
- ICLR 2025 Oral 발표 (상위 발표)

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 다양한 태스크 지원을 위한 추가 설계 복잡도
- 확장 아이디어: foundation model 형태로 대규모 사전학습 시 범용 TSPM의 제로샷 일반화 가능성

## 6. 참고
- arXiv: https://arxiv.org/abs/2410.16032
- OpenReview: https://openreview.net/forum?id=1CLzLXSFNn
- ICLR 2025 Oral: https://iclr.cc/virtual/2025/oral/31932
- 전작 TimeMixer (ICLR 2024): category/Decomposition/TrendSeasonal/2024_ICLR_TimeMixer.md
