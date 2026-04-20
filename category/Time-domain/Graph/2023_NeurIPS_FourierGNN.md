---
title: "FourierGNN: Rethinking Multivariate Time Series Forecasting from a Pure Graph Perspective"
authors: Kun Yi et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2311.06190
code_url: https://github.com/aikunyi/FourierGNN
tags: [graph, gnn, freq-domain, fft, multivariate, forecasting, time-domain]
primary_category: category/Time-domain/Graph
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_FourierGNN.pdf
pdf_source: arxiv
---

## TL;DR
FourierGNN은 다변량 시계열을 시공간 그래프로 모델링하고, 푸리에 공간에서 행렬 곱셈으로 GNN 연산을 수행하는 Fourier Graph Operator(FGO)를 제안한다. 별도의 시간 네트워크 없이 순수 그래프 관점에서 시·공간 의존성을 O(N) 복잡도로 동시 학습한다.

## 1. 기존 모델의 한계 / 가설
- 기존 GNN 기반 예측 모델들은 GCN(공간)과 LSTM(시간) 등을 별도로 구성하여 복잡도가 높고 두 모듈의 결합 최적화가 어렵다.
- 시·공간 의존성을 통합 처리하는 단순한 프레임워크가 필요하다.
- 가설: 푸리에 공간에서의 행렬 연산으로 GNN 연산을 수행하면 시·공간 의존성을 통합·효율적으로 모델링할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: Fourier Graph Operator(FGO)를 스택하여 순수 그래프 관점으로 다변량 시계열의 시·공간 의존성을 모델링.
- **아키텍처**:
  1. 다변량 시계열을 시공간 그래프로 변환 (변수=노드, 시간=엣지).
  2. **FGO**: 푸리에 변환 후 주파수 도메인 행렬 곱셈으로 그래프 컨볼루션 수행.
  3. 역푸리에 변환 후 예측 헤드 적용.
- **복잡도**: 입력 시퀀스 길이 N에 대해 선형(O(N)).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | ≥10% 평균 향상 주장 |
| 192     | see paper | see paper | see Table 1 |
| 336     | see paper | see paper | see Table 1 |
| 720     | see paper | see paper | see Table 1 |

출처: Table 1 of paper (ETTh1 포함).

## 4. 주요 기여
- 시계열 예측을 순수 그래프 문제로 재정의하는 새로운 관점 제시.
- 푸리에 공간 행렬 곱셈 기반 FGO로 GCN+LSTM 구조를 단순화.
- 선형 복잡도로 8개 실세계 MTS 데이터셋에서 SOTA 대비 평균 10% 이상 개선.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 그래프 구조가 명시적이지 않은 경우 그래프 생성 전략에 의존.
- ※ 메모: CrossGNN과 같은 NeurIPS 2023 GNN 계열 논문들이 다양한 노이즈 처리 전략을 채택함. 비교 분석 가치 있음.

## 6. 참고
- arXiv: https://arxiv.org/abs/2311.06190
- GitHub: https://github.com/aikunyi/FourierGNN
- 관련: CrossGNN (NeurIPS 2023), MTGNN
