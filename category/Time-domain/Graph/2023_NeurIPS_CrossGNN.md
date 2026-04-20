---
title: "CrossGNN: Confronting Noisy Multivariate Time Series Via Cross Interaction Refinement"
authors: Qihe Huang et al.
venue: NeurIPS
year: 2023
paper_url: https://openreview.net/forum?id=xOzlW2vUYc
code_url: https://github.com/hqh0728/CrossGNN
tags: [graph, gnn, multivariate, noise, multi-scale, cross-variable, forecasting, time-domain]
primary_category: category/Time-domain/Graph
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_CrossGNN.pdf
pdf_source: publisher
---

## TL;DR
CrossGNN은 노이즈가 많은 다변량 시계열 예측을 위해 적응적 다중 스케일 식별자(AMSI)로 멀티스케일 시계열을 구성하고, Cross-Scale GNN과 Cross-Variable GNN을 결합하여 선형 복잡도로 스케일 간·변수 간 상호작용을 정제한다.

## 1. 기존 모델의 한계 / 가설
- 기존 GNN 기반 시계열 모델은 노이즈가 많은 입력에 취약하며, 단일 스케일에서만 동작한다.
- 변수 간 이종성(heterogeneity)과 동종성(homogeneity)을 동시에 모델링하기 어렵다.
- 가설: 다중 스케일 분리로 노이즈를 줄이고, Cross-Scale/Variable GNN으로 계층적 상호작용을 정제하면 성능이 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: 적응적 다중 스케일 식별자로 멀티스케일 시계열 생성 후, Cross-Scale GNN과 Cross-Variable GNN을 통해 교차 상호작용 정제.
- **아키텍처**:
  1. **AMSI (Adaptive Multi-Scale Identifier)**: 노이즈 감소를 위한 다중 스케일 시계열 구성.
  2. **Cross-Scale GNN**: 스케일 간 트렌드·노이즈 필터링을 위한 그래프 정제.
  3. **Cross-Variable GNN**: 변수 간 동종성·이종성을 활용한 상호작용 학습.
- **복잡도**: 입력 길이 L에 대해 선형 O(L).

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
| 96      | see paper | see paper | see Table 1 |
| 192     | see paper | see paper | see Table 1 |
| 336     | see paper | see paper | see Table 1 |
| 720     | see paper | see paper | see Table 1 |

출처: Table 1 of paper (8개 MTS 데이터셋 포함).

## 4. 주요 기여
- 노이즈 대응을 위한 적응적 다중 스케일 식별자(AMSI) 제안.
- Cross-Scale GNN과 Cross-Variable GNN의 결합으로 계층적 의존성 학습.
- 8개 실세계 MTS 데이터셋에서 SOTA 대비 일관적 성능 향상.
- 선형 복잡도로 긴 시퀀스에도 적용 가능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 스케일 수 하이퍼파라미터 선택에 민감.
- ※ 메모: FourierGNN(NeurIPS 2023)과 같은 시기에 GNN 기반 MTS 예측의 다양한 접근이 제안됨. 두 모델 비교 실험 흥미로울 것.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=xOzlW2vUYc
- GitHub: https://github.com/hqh0728/CrossGNN
- 관련: FourierGNN (NeurIPS 2023), MTGNN
- ※ 원본 PDF: NeurIPS proceedings에서 취득 (arXiv 없음)
