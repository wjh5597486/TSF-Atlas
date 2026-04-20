---
title: "CoMRes: Semi-Supervised Time Series Forecasting Utilizing Consensus Promotion of Multi-Resolution"
authors: (authors in proceedings)
venue: ICLR 2025
year: 2025
paper_url: https://openreview.net/forum?id=bRa4JLPzii
code_url: ""
tags: [semi-supervised, multi-resolution, consensus, self-supervised, augmentation]
primary_category: category/SelfSupervised
related_categories:
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_CoMRes.pdf
pdf_source: arxiv
---

## TL;DR
CoMRes는 레이블 없는 데이터를 활용하는 반지도학습(semi-supervised) 시계열 예측 방법이다. 다중 해상도로 증강된 데이터에 대한 단일 뷰 모델들의 합의(consensus)를 촉진하는 전략으로, 명시적 미래 값 레이블 없이도 학습하며 긴 예측 지평에서 강건한 성능을 보인다.

## 1. 기존 모델의 한계 / 가설
- 기존 지도 학습 기반 예측 모델은 레이블 데이터에만 의존 → 레이블 없는 시계열 데이터의 방대한 양 미활용
- 예측 지평이 길어질수록 지도 학습 모델 성능 급감
- 다중 해상도 증강 데이터의 일관성(consensus)을 활용하면 레이블 없이도 학습 가능하다는 가설

## 2. 제안 방법론
- 핵심 아이디어: 증강 데이터에 대한 multi-view consensus promotion으로 미래 레이블 없이 학습
- **Multi-Resolution 증강**: 다양한 해상도의 시계열 뷰 생성
- **Consensus Promotion**: 각 뷰에서의 단일 뷰 모델 예측 간 합의를 강화하는 학습 목표
- 반지도학습: 레이블 데이터와 레이블 없는 데이터 모두 활용
- 손실함수: 지도 손실 + consensus 촉진 손실

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 RevIN
- Train/Val/Test split: 표준 ETT 분할 (레이블 비율 조절 실험 포함)
- Loss function: MSE + consensus loss
- 기타: 다양한 레이블 비율(semi-supervised) 실험

### 3.2 Results
표준 지도 학습 모델 대비 정확도 향상 및 긴 예측 지평에서 강건성 개선. 상세 수치는 논문 Table 참고.

출처: 논문 Table (ICLR 2025, proceedings hash: 6ca155a091aedc939d289df8f16f6c75).

## 4. 주요 기여
- 시계열 예측에서의 semi-supervised learning 효과적 적용
- Multi-resolution consensus promotion으로 레이블 없는 데이터 활용
- 긴 예측 지평에서의 강건성 개선
- 레이블 비율이 낮을 때 특히 효과적

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 증강 전략의 하이퍼파라미터에 민감할 수 있음
- 확장 아이디어: foundation model 사전학습과 결합하여 더 강력한 semi-supervised 예측

## 6. 참고
- OpenReview: https://openreview.net/forum?id=bRa4JLPzii
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/29110
- Paper PDF: https://proceedings.iclr.cc/paper_files/paper/2025/file/6ca155a091aedc939d289df8f16f6c75-Paper-Conference.pdf
