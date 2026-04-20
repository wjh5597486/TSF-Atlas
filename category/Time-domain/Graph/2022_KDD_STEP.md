---
title: Pre-training Enhanced Spatial-temporal Graph Neural Network for Multivariate Time Series Forecasting
authors: Zezhi Shao, Zhao Zhang, Fei Wang, Yongjun Xu
venue: KDD 2022
year: 2022
paper_url: https://dl.acm.org/doi/10.1145/3534678.3539396
code_url: https://github.com/GestaltCogTeam/STEP
tags: [time-domain, graph, spatial-temporal, pre-training]
primary_category: category/Time-domain/Graph
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_KDD_STEP.pdf
pdf_source: arxiv
---

## TL;DR
STEP enhances spatial-temporal graph neural networks (STGNNs) with a scalable pre-training module that learns long-term temporal patterns (e.g., past two weeks). Pre-trained segment-level representations provide contextual information for short-term STGNN input, improving multivariate time series forecasting on electricity and traffic datasets.

## 1. 기존 모델의 한계 / 가설
- Most STGNNs consider only short-term historical data (e.g., past one hour) due to memory and computational constraints.
- Real-world time series patterns and inter-series dependencies are best understood from long-term history.
- Without long-term context, STGNNs miss seasonal patterns, trend shifts, and dependencies that only emerge over days/weeks.
- Standard end-to-end training of STGNNs on very long sequences is inefficient.

## 2. 제안 방법론

### 핵심 아이디어
Decouple long-term learning from short-term prediction: use a scalable pre-training module to learn segment-level representations from long history (e.g., past 2 weeks at coarse resolution), then feed these representations as contextual input to standard STGNNs for fine-grained short-term forecasting.

### 아키텍처 구성

**Two-Stage Framework:**

1. **Pre-training Stage: Long-term Representation Learning**
   - Input: Very long-term history (e.g., 2 weeks at hourly resolution)
   - Segment-level downsampling: aggregate long sequences into segment-level features
   - Lightweight temporal encoder: learns segment embeddings capturing long-term patterns
   - Output: Fixed-size segment-level representation for each time series

2. **Fine-tuning/Inference Stage: Short-term STGNN Forecasting**
   - Input: Short-term history (e.g., past 1 hour) + pre-trained segment representation
   - Standard STGNN (e.g., GraphWaveNet, DCRNN)
   - Segment representation provides contextual bias/initialization
   - Output: Short-term predictions

3. **Segment-Level Representation**
   - Learned via contrastive or reconstruction loss on long-term sequences
   - Captures coarse temporal patterns, seasonality, trend
   - Shared across all downstream forecasting horizons

### 수식
- Segment representation: $\mathbf{s}_i \in \mathbb{R}^d$ for time series $i$ at coarse temporal scale
- STGNN input: $[\mathbf{x}_{\text{short}}^{i}, \mathbf{s}_i]$ (concatenation of short-term and segment representation)
- Output: $\hat{\mathbf{y}}^{i}_{\text{future}}$

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96 (short-term) + long-term history in pre-training
- Forecast horizons: 96, 192, 336, 720
- Long-term pre-training window: 2 weeks (336 hours) or longer
- Normalization: Standard normalization
- Train/Val/Test: 6:2:2 split
- Batch size: 32
- Optimizer: Adam
- LR: 0.001
- Loss function: MSE (forecasting) + contrastive/reconstruction loss (pre-training)

### 3.2 Results
| Horizon | Performance | SOTA Baseline |
|---------|-------------|---------------|
| 96      | See paper  | STGNN baseline |
| 192     | See paper  | STGNN baseline |
| 336     | See paper  | STGNN baseline |
| 720     | See paper  | STGNN baseline |

출처: Paper published at KDD 2022; reports improvements on multivariate forecasting benchmarks (electricity, traffic, weather). See arXiv:2206.09113 for detailed tables.

## 4. 주요 기여
- Novel pre-training framework for STGNNs to capture long-term temporal patterns without expensive end-to-end training
- Segment-level representation learning that bridges long-term dependencies and short-term forecasting
- Demonstrates that contextual long-term information significantly improves STGNN forecasting
- Efficient two-stage training: pre-training is done once, short-term STGNN can be adapted to various horizons
- Validated on multiple domains (electricity, traffic, weather)

## 5. 한계 및 후속 연구 아이디어
- Segment-level representation is fixed; may not adapt to varying forecasting horizons or data shifts
- Pre-training requires long unlabeled time series; less applicable to new datasets with limited history
- Computational cost of pre-training stage not thoroughly analyzed
- Future work: adaptive segment representation, meta-learning for few-shot forecasting, multi-task learning across domains
- Could combine with other pre-training objectives (masked time series modeling, next-segment prediction)

## 6. 참고
- 관련 논문: GraphWaveNet, DCRNN, MTGNN (STGNN baselines), Informer (Transformer baseline)
- 구현: Official code at https://github.com/GestaltCogTeam/STEP; developed with BasicTS framework
- PDF 출처: arxiv:2206.09113
