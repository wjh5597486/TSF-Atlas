---
title: "Chimera: Effectively Modeling Multivariate Time Series with 2-Dimensional State Space Models"
authors: Ali Behrouz et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2406.04320
code_url: ""
tags: [ssm, mamba, 2d-ssm, multivariate, time-domain]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_Chimera.pdf
pdf_source: arxiv
---

## TL;DR
Chimera는 2차원 SSM(State Space Model)으로 시계열의 시간 축과 변수 축을 동시에 모델링. 두 SSM head로 장기 추세와 계절 패턴을 각각 학습하며, 2D prefix sum 재구성으로 병렬 훈련을 달성.

## 1. 기존 모델의 한계 / 가설
- 기존 1D Mamba 기반 모델은 시간 축만 모델링하거나, 변수 축과 시간 축을 독립 SSM으로 분리 처리.
- 진정한 2D 의존성(시간×변수) 포착이 어렵고 훈련 병렬화도 제한됨.
- 가설: 입력 의존적 파라미터화와 서로 다른 이산화 프로세스로 두 SSM 헤드가 각각 장기 추세와 계절 패턴을 학습 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 두 SSM head (다른 이산화)로 seasonal/long-term progression 분리 학습 + 2D parallel selective scan.
- **아키텍처**:
  - 입력 의존적 2D SSM 파라미터화
  - Head 1: 장기 추세/진행(long-term progression) 학습용 이산화
  - Head 2: 계절 패턴(seasonal) 학습용 이산화
  - 2D prefix sum 재구성으로 O(N·T) 복잡도로 병렬 훈련
  - 2D Mamba 및 Mamba-2가 특수 케이스임을 이론적으로 증명
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 또는 720 (표준 설정)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | Transformer 대비 경쟁력 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper (long-term forecasting).

## 4. 주요 기여
- 2D SSM 프레임워크 제안 및 이론적 표현력 분석
- 두 SSM 헤드로 추세/계절 패턴 분리 학습
- 빠른 2D parallel selective scan 알고리즘
- ECG, speech 분류 + LTSF + 이상 탐지 등 다양한 태스크에서 SOTA

## 5. 한계 및 후속 연구 아이디어
- 2D SSM의 이산화 파라미터 선택에 민감할 수 있음
- 변수 수가 매우 많은 경우 메모리 비용 증가

## 6. 참고
- [arXiv 2406.04320](https://arxiv.org/abs/2406.04320)
