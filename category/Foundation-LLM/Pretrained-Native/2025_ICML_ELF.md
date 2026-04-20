---
title: "Lightweight Online Adaption for Time Series Foundation Model Forecasts"
authors: Thomas L. Lee et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2502.12920
code_url: ""
tags: [foundation-model, online-adaptation, lightweight, test-time, forecasting, zero-shot]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_ELF.pdf
pdf_source: arxiv
---

## TL;DR
ELF (Efficient Lightweight Forecasting) proposes a lightweight online adaptation mechanism for time series foundation model forecasts. It combines an ELF-Forecaster (learns current data distribution online) and ELF-Weighter (combines FM forecasts with local adaptation), improving deployed foundation models without retraining. Evaluated on ETTh/m, Weather, Traffic, ECL, Solar.

## 1. 기존 모델의 한계 / 가설
- Foundation model(Chronos, TTM, TimesFM, Moirai 등)은 배포 후 고정되어 새로 도착하는 데이터의 분포 변화에 적응하지 못함.
- Online retraining은 계산 비용이 너무 높음.
- 경량 온라인 적응 메커니즘으로 배포된 FM의 성능을 향상시킬 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: FM 예측과 온라인으로 학습된 로컬 예측기를 결합하는 ELF 메커니즘.
- **아키텍처**:
  - **ELF-Forecaster**: 현재 데이터 분포를 온라인으로 학습하는 경량 예측기.
  - **ELF-Weighter**: FM 예측과 ELF-Forecaster 예측의 최적 결합 가중치 동적 결정.
  - FM 자체는 수정하지 않음 (비침습적).
- **손실함수**: 예측 MSE (온라인).
- **학습 전략**: Online learning (배포 중 연속 업데이트).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): FM 기본값 (Chronos/TTM/TimesFM/Moirai/VisionTS)
- Forecast horizons: 96, 192, 336, 720
- Normalization: FM 기본값
- Train/Val/Test split: 표준 분할 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Traffic, ECL, Solar, US Weather)
- Batch size / Optimizer / LR: 온라인 경량 (논문 참조)
- Loss function: MSE
- 기타: Chronos (tiny), TTM, TimesFM, Moirai (small), VisionTS와 결합 실험

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | consistent improvement over FM |
| 192     | see paper | see paper | consistent improvement over FM |
| 336     | see paper | see paper | consistent improvement over FM |
| 720     | see paper | see paper | consistent improvement over FM |

출처: Table of paper (9 datasets × 5 FMs).

## 4. 주요 기여
- 경량 온라인 적응 메커니즘 ELF로 기존 FM 예측 일관 향상.
- FM 재훈련 불필요: 비침습적 플러그-앤-플레이.
- 9개 데이터셋 × 5개 FM 조합에서 성능 향상 확인.
- ELF-Forecaster + ELF-Weighter의 분리된 모듈 설계.

## 5. 한계 및 후속 연구 아이디어
- 온라인 적응이 급격한 분포 변화(distribution shift)에서 지연 반응 가능.
- ※ 메모: RAFT(ICML 2025)의 검색 기반과 달리 온라인 분포 추적 접근.

## 6. 참고
- 관련 논문: Chronos (ICML 2024), TimesFM (ICML 2024), MOIRAI (ICML 2024), TTMs (NeurIPS 2024)
- arXiv: https://arxiv.org/abs/2502.12920
- ICML 2025 Poster: https://icml.cc/virtual/2025/poster/44485
