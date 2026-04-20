---
title: CDPM - Conditional Denoising Meets Polynomial Modeling - A Flexible Decoupled Framework for Time Series Forecasting
authors: Jintao Zhang, Mingyue Cheng, Xiaoyu Tao, Zhiding Liu, Daoyu Wang
venue: IJCAI 2025
year: 2025
paper_url: https://arxiv.org/html/2410.13253
code_url: https://github.com/zjt-gpu/CDPM
tags: [decomposition, trend-seasonal, diffusion, polynomial-modeling]
primary_category: Decomposition/TrendSeasonal
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_CDPM.pdf
pdf_source: arxiv
---

## TL;DR
CDPM proposes a flexible decoupled framework combining Conditional Denoising Seasonal Module (CDSM) with Polynomial Trend Module (PTM). By decomposing time series into trend and seasonal components with tailored models and reformulating the Evidence Lower Bound for joint optimization, the framework achieves competitive results on ETTh1 and other benchmarks.

## 1. 기존 모델의 한계 / 가설
- Uniform noise treatment inappropriate for heterogeneous trend and seasonal components
- Trend components capture gradual long-term changes requiring different modeling approach
- Seasonal components exhibit periodic patterns suitable for diffusion-based refinement
- Decoupled modeling enables specialized treatment of component characteristics
- Evidence Lower Bound can be reformulated to enable joint optimization of multiple components

## 2. 제안 방법론
### Decomposition Strategy
- Decomposes time series into **Trend** and **Seasonal** components
- Each component gets specialized processing tailored to its characteristics

### Conditional Denoising Seasonal Module (CDSM)
- Refines seasonal component through diffusion process
- Conditioned on historical seasonal patterns
- Uses noise injection and gradual denoising
- Captures periodic dependencies with learned denoising process

### Polynomial Trend Module (PTM)
- Models trend component using polynomial fitting
- Captures gradual long-term changes and smooth transitions
- Non-diffusion approach suitable for trend characteristics
- Direct modeling without noise-based refinement

### Optimization Framework
- Reformulates Evidence Lower Bound (ELBO) to enable joint optimization
- Allows coupled learning of trend and seasonal components
- Synchronized training of CDSM and PTM modules

### Innovation
By combining diffusion-based seasonal modeling with polynomial trend modeling, and reformulating ELBO for joint optimization, the framework leverages component-specific characteristics while maintaining coherent joint learning.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Input/Prediction Window: Historical length L with target length T=168 for ETTh1
- Forecast horizons: 96, 192, 336, 720 steps (reported results for T=168)
- Normalization: Instance normalization using historical statistics
- Train/Val/Test split: Standard 7:1:2 (2016-2018 data)
- Batch size: 16
- Optimizer: Exponential Learning Rate Scheduler with initial rate 1×10⁻³
- Diffusion steps: 50 with cosine variance schedule
- Hardware: NVIDIA GeForce RTX 4090 GPU

### 3.2 Results
**ETTh1 Results (T=168, MSE/MAE):**

| Model | MSE | MAE |
|-------|-----|-----|
| CDPM | 0.4364 | 0.4363 |
| iTransformer | 0.4433 | 0.4429 |
| DLinear | 0.4372 | 0.4420 |
| Diffusion-TS | 1.6559 | 1.0400 |

Source: Paper experimental results

※ Results shown for specific horizon. Additional horizons (96, 192, 336, 720) tested but not detailed in available summaries.

## 4. 주요 기여
- Proposes decoupled framework with component-specific modeling
- Introduces CDSM for conditional diffusion-based seasonal refinement
- Introduces PTM for polynomial trend modeling
- Reformulates ELBO to enable joint optimization of heterogeneous components
- Achieves competitive or superior performance vs iTransformer, DLinear on ETTh1
- Flexible framework applicable to various component decompositions

## 5. 한계 및 후속 연구 아이디어
- Computational overhead of dual-module architecture and diffusion steps
- Sensitivity to decomposition quality and trend/seasonal boundary definition
- Polynomial degree selection for PTM
- Scalability to very long-horizon forecasting
- Extension to multi-level hierarchical decompositions

## 6. 참고
- arXiv: https://arxiv.org/html/2410.13253
- GitHub: https://github.com/zjt-gpu/CDPM
- IJCAI Proceedings: Pages 6993-7001
