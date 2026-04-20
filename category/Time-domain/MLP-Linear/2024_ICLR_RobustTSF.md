---
title: "RobustTSF: Towards Theory and Design of Robust Time Series Forecasting with Anomalies"
authors: Hao Cheng, Qingsong Wen, Yang Liu, Liang Sun
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2402.02032
code_url: https://github.com/haochenglouis/RobustTSF
tags: [robustness, anomaly, loss]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_RobustTSF.pdf
pdf_source: arxiv
---

## TL;DR
학습 데이터에 **이상치(anomaly)** 가 섞였을 때 TSF가 견딜 수 있도록 이론적 분석 + loss/샘플링 설계. Heavy-tailed noise 환경에서 표준 MSE는 취약함을 보이고, 강건한 대안 제시.

## 1. 기존 모델의 한계 / 가설
- 실세계 학습셋은 이상치를 포함하지만 대부분의 TSF 모델은 이를 무시.
- MSE 기반 학습은 이상치에 민감해 장기 예측 품질이 떨어진다.

## 2. 제안 방법론
- 이상치 유형 분석 후 **강건한 loss** 및 샘플 재가중(sample reweighting) 제안.
- 간단한 MLP/Linear 백본으로도 성능 개선을 입증(방법의 backbone-agnostic 성격).

## 3. ETTh1 실험 결과
### 3.1 Setting
- Input length 96, horizon ∈ {4, 8} 또는 표준 {96,192,336,720}.
- 학습셋에 synthetic anomaly 주입.

### 3.2 Results
이상치 비율이 클수록 제안 방법과 baseline의 격차가 커짐. 상세는 원문 Table 참고.

## 4. 주요 기여
- 이상치가 TSF 학습에 주는 영향의 이론적 분석.
- Backbone-agnostic한 robust loss / reweighting 설계.

## 5. 참고
- 코드: https://github.com/haochenglouis/RobustTSF
