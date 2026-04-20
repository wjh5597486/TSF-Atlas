---
title: Task-Aware Reconstruction for Time-Series Transformer
authors: Ranak Roy Chowdhury, Xiyuan Zhang, Jingbo Shang, Rajesh K. Gupta, Dezhi Hong
venue: KDD 2022
year: 2022
paper_url: https://dl.acm.org/doi/10.1145/3534678.3539329
code_url: https://github.com/ranakroychowdhury/TARNet
tags: [time-domain, transformer, reconstruction, self-supervised]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_KDD_TARNet.pdf
pdf_source: none
---

## TL;DR
TARNet proposes task-aware data reconstruction where masking strategy is informed by self-attention from end-task training. Reconstruction and end-task (forecasting/classification) are trained alternately, sharing parameters. Demonstrates significant improvements across classification and regression time series benchmarks.

## 1. 기존 모델의 한계 / 가설
- Traditional learning decouples data reconstruction from end-task objectives (e.g., forecasting, classification).
- Representations learned via decoupled reconstruction are not informed by end-task requirements and may be sub-optimal.
- Self-attention patterns in the end-task network reveal which timestamps are most important for the task, but this information is not leveraged in data reconstruction.
- Simple masking strategies (random, fixed patterns) miss task-relevant structure.

## 2. 제안 방법론

### 핵심 아이디어
Integrate end-task training directly into the learning process by using self-attention score distributions from the end-task network to guide data reconstruction. Timestamps deemed important by the end-task attention are masked and reconstructed, creating a bidirectional feedback loop.

### 아키텍처 구성

1. **Task-Aware Masking Strategy**
   - Extract self-attention weight distributions from end-task Transformer
   - Use attention scores to sample (mask) important timestamps probabilistically
   - High-attention timestamps → more likely to be masked → reconstructed

2. **Alternating Training**
   - Epoch t: Train end-task (forecasting/classification) on full data
   - Epoch t+1: Train reconstruction task on masked data using same backbone
   - Share all parameters between tasks
   - Repeat until convergence

3. **Reconstruction Loss**
   - MSE or MAE on masked-out timestamps
   - Reconstruction decoder/head is task-specific

4. **Unified Architecture**
   - Single Transformer backbone
   - Task-specific heads (forecasting head, reconstruction head)
   - No additional parameters beyond standard Transformer

### 수식
- Masking probability: $P(\text{mask}[t]) = \alpha \cdot \text{attention}[t] + (1-\alpha) \cdot \epsilon$
  - $\text{attention}[t]$: normalized self-attention score at timestamp $t$
  - $\alpha$: balance parameter
  - $\epsilon$: uniform random noise for exploration

## 3. ETTh1 실험 결과

### 3.1 Setting
- Input length (lookback): 96 (encoder input)
- Forecast horizons: 96, 192, 336, 720
- Normalization: Instance-wise normalization
- Train/Val/Test: Standard 6:2:2 split
- Batch size: 32
- Optimizer: Adam
- LR: 0.001
- Loss function: MSE (end-task), MSE (reconstruction)
- Masking ratio: $\alpha = 0.5$

### 3.2 Results
"Extensive experiments on tens of classification and regression datasets" with significant improvements. Specific ETTh1 results not fully detailed in search results; paper reports improvements on both classification and regression tasks.

| Task | Benchmark | Improvement |
|------|-----------|-------------|
| Regression (Forecasting) | Multiple datasets | Significant |
| Classification | Multiple datasets | Significant |

출처: Paper reports "significantly outperforms state-of-the-art baseline models across all evaluation metrics."

## 4. 주요 기여
- Novel task-aware masking strategy leveraging end-task attention for data reconstruction
- Unified training paradigm: reconstruction and end-task learned jointly with shared parameters
- Alternating training scheme that iteratively refines both tasks
- Comprehensive evaluation across multiple time series benchmarks (classification, regression, forecasting)
- Demonstrates that task-informed reconstruction enhances learned representations

## 5. 한계 및 후속 연구 아이디어
- Computational overhead: alternating training requires 2× forward passes per epoch compared to baseline
- Masking strategy depends on end-task attention; may not transfer to different downstream tasks
- Limited analysis of which task benefits more from joint training (forecasting vs. classification)
- Future work: multi-task learning with shared reconstruction, adaptive weighting of reconstruction vs. end-task loss
- Could combine with other self-supervised learning paradigms (contrastive, pretext tasks)

## 6. 참고
- 관련 논문: Transformer for time series (Informer, Autoformer), self-supervised learning in vision (MAE, BEiT)
- 구현: Official code and datasets at https://github.com/ranakroychowdhury/TARNet
- 원본 PDF 미취득: dar.ucsd.edu SSL 인증 에러
