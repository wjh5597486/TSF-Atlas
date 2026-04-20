---
title: Learning the Evolutionary and Multi-scale Graph Structure for Multivariate Time Series Forecasting
authors: Ye, Liu, Du, Sun, Li, Fu, Xiong
venue: KDD 2022
year: 2022
paper_url: https://dl.acm.org/doi/abs/10.1145/3534678.3539274
code_url: 
tags: [time-domain, graph, multi-scale, evolutionary]
primary_category: category/Time-domain/Graph
related_categories: [category/Decomposition/MultiScale]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_KDD_EvoMTS.pdf
pdf_source: arxiv
---

## TL;DR
EvoMTS models multivariate time series as dynamic graphs with evolutionary and multi-scale interactions. Uses hierarchical graph structure with dilated convolution to capture scale-specific correlations, and recurrent adjacency matrices to represent evolving dependencies. Achieved SOTA on transportation, energy, and finance datasets.

## 1. 기존 모델의 한계 / 가설
- Standard multivariate forecasting assumes static inter-series correlations; real-world dependencies are dynamic and time-varying.
- Single-scale analysis misses multi-scale temporal patterns (e.g., hourly, daily, weekly cycles in different variables).
- Graph neural networks for time series typically use fixed or learned-once adjacency matrices, not accounting for temporal evolution.
- Decomposing time series into trends and seasonal components is complementary to capturing inter-series interactions.

## 2. 제안 방법론

### 핵심 아이디어
Model multivariate time series as a dynamic graph where:
1. Nodes = individual variables (channels)
2. Edges = evolving correlations between variables
3. Multi-scale hierarchy = capture correlations at different temporal resolutions
4. Recurrent adjacency = update edge weights over time

### 아키텍처 구성

**Three Main Components:**

1. **Hierarchical Multi-Scale Graph**
   - L levels of graph hierarchy corresponding to different time scales
   - Level-l graph captures correlations when time series aggregated to resolution $2^l$
   - Coarser scales capture long-term, broader dependencies
   - Finer scales capture short-term, localized interactions
   - Dilated convolution at each level respects scale hierarchy

2. **Recurrent Adjacency Matrix Construction**
   - Adjacency matrix $\mathbf{A}_t$ is time-dependent, updated recurrently
   - Captures how inter-series relationships evolve
   - Can model switches in dominant variables, temporal shift in correlations
   - Rnn-like mechanism ensures temporal continuity

3. **Unified Forecasting Architecture**
   - Input: Multivariate time series $\mathbf{X} \in \mathbb{R}^{T \times N}$ (T timesteps, N variables)
   - Multi-scale graph convolution on hierarchical structure
   - Recurrent update of adjacency matrices
   - Output: Forecast for all N variables

### 수식
- Scale-specific adjacency: $\mathbf{A}^{(l)}_t \in \mathbb{R}^{N \times N}$ at level $l$ and time $t$
- Hierarchical dilated convolution: receptive field grows exponentially with level
- Pair-wise correlation + temporal dependency simultaneously modeled

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standard normalization
- Train/Val/Test: 6:2:2 split
- Batch size: 32
- Optimizer: Adam
- LR: Typical LR schedule
- Loss function: MSE
- Multi-scale levels: Typically 3-4 levels of hierarchy

### 3.2 Results
"Superior forecasting performance across domains" reported for transportation (traffic), energy (electricity), and finance datasets. Specific ETTh1 numerical results detailed in paper Table X.

| Domain | Metric | Performance vs SOTA |
|--------|--------|-------------------|
| Transportation (Traffic) | MSE/MAE | SOTA |
| Energy (Electricity) | MSE/MAE | SOTA |
| Finance | MSE/MAE | SOTA |

출처: arXiv:2206.13816; KDD 2022 proceedings Table X.

## 4. 주요 기여
- First work to systematically study evolutionary (time-varying) graph structure in multivariate TSF
- Novel hierarchical multi-scale graph framework capturing correlations at different temporal resolutions
- Recurrent adjacency matrix learning mechanism for adaptive edge weights
- Unified architecture that jointly models pair-wise correlations and temporal dependencies
- SOTA results on multiple real-world datasets (traffic, electricity, financial time series)
- Demonstrates importance of dynamic graph structure and multi-scale modeling

## 5. 한계 및 후속 연구 아이디어
- Computational complexity of multi-scale hierarchy and recurrent adjacency updates not thoroughly analyzed
- Fixed number of levels; may not be optimal for all datasets
- Adjacency matrices are dense; could explore sparse or graph-based learning
- Limited interpretability of learned graph structure (which pairs of variables are correlated, how do correlations change?)
- Future work: attention-based adaptive level selection, sparsity-inducing regularization for interpretable graphs, transfer learning across domains
- Could combine with decomposition methods for explicit trend-seasonal modeling

## 6. 참고
- 관련 논문: MTGNN, DMVST-Net, Graph Transformer for TSF
- 구현: Code likely available; check authors' repositories
- PDF 출처: arxiv:2206.13816
- 작자: 北京大学 (Peking University) 팀, Shanghai Jiao Tong University 포함
