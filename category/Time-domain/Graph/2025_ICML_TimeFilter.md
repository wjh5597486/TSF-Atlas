---
title: "TimeFilter: Patch-Specific Spatial-Temporal Graph Filtration for Time Series Forecasting"
authors: Yifan Hu et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2501.13041
code_url: https://github.com/TROUBADOUR000/TimeFilter
tags: [time-domain, graph, spatial-temporal, patch, filtration, channel-dependent, multivariate-forecasting]
primary_category: category/Time-domain/Graph
related_categories: [category/CrossCutting/Patching, category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimeFilter.pdf
pdf_source: arxiv
---

## TL;DR
TimeFilter proposes a patch-specific spatial-temporal graph filtration framework for multivariate time series forecasting. By constructing patch-level graphs and applying adaptive graph learning with graph signal filtration, it effectively captures both intra-variate temporal patterns and inter-variate spatial dependencies across 13 real-world datasets.

## 1. 기존 모델의 한계 / 가설
- 기존 그래프 기반 방법들은 전체 시퀀스 수준의 그래프를 구성하여 세밀한 패치 수준의 시공간 패턴을 놓침.
- 고정된 그래프 구조(pre-defined adjacency)는 데이터에 따라 유연하게 변하는 의존성을 포착하기 어려움.
- 패치 단위로 공간-시간 그래프를 구성하면 보다 풍부한 로컬 의존성을 학습할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 패치별 공간-시간 그래프를 구성하고 그래프 신호 필터링을 통해 시공간 의존성 학습.
- **아키텍처**:
  - **Spatial-Temporal Construction Module**: 패치 단위로 시공간 그래프 구성.
  - **Patch-Specific Filtration Module**: 각 패치에 대한 그래프 신호 필터링.
  - **Adaptive Graph Learning Module**: 데이터 기반 adaptive 그래프 구조 학습.
- **손실함수**: MSE.
- **학습 전략**: End-to-end supervised.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 (336 또는 512)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather, Traffic, Solar, PEMS03/04/07/08 등 13개 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | competitive |
| 192     | see paper | see paper | competitive |
| 336     | see paper | see paper | competitive |
| 720     | see paper | see paper | competitive |

출처: Table of paper (13 real-world datasets).

## 4. 주요 기여
- 패치 수준 공간-시간 그래프 구성으로 세밀한 시공간 의존성 포착.
- Adaptive graph learning으로 데이터 기반 그래프 구조 자동 학습.
- 13개 다양한 실제 데이터셋에서 검증.
- PEMS 교통 데이터를 포함한 광범위한 평가.

## 5. 한계 및 후속 연구 아이디어
- 그래프 구성 비용이 패치 수에 비례하여 증가.
- PEMS 데이터를 포함하므로 spatio-temporal 포커스가 있으나 ETT 벤치마크도 포함.
- ※ 메모: CrossGNN(NeurIPS 2023)의 그래프 기반 접근을 패치 단위로 세분화한 확장.

## 6. 참고
- 관련 논문: CrossGNN (NeurIPS 2023), FourierGNN (NeurIPS 2023), PatchTST (ICLR 2023)
- GitHub: https://github.com/TROUBADOUR000/TimeFilter
- arXiv: https://arxiv.org/abs/2501.13041
