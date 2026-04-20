---
title: Non-collective Calibrating Strategy for Time Series Forecasting (Socket+Plug)
authors: Bin Wang, Yongqi Han, Minbo Ma, Tianrui Li, Junbo Zhang, Feng Hong, Yanwei Yu
venue: IJCAI 2025
year: 2025
paper_url: https://arxiv.org/abs/2506.03176
code_url: ""
tags: [post-hoc-calibration, model-agnostic, non-collective-optimization]
primary_category: Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_SoP.pdf
pdf_source: arxiv
---

## TL;DR
Socket+Plug (SoP) is a model-agnostic post-training calibration strategy that addresses multi-target learning conflicts in time series forecasting. Rather than collective optimization across all targets, it assigns exclusive optimizers and early-stopping monitors to individual predicted targets, achieving up to 22% performance improvement with minimal architecture changes.

## 1. 기존 모델의 한계 / 가설
- Existing calibration approaches optimize variables collectively across time steps
- Multi-target learning conflict occurs when optimizing shared parameters for all targets simultaneously
- Different targets (variables and forecast horizons) have different learnability levels
- Collective calibration underutilizes model learning capacity for heterogeneous targets
- Standard calibration approach: maintain frozen inference model while retraining layers collectively for all targets

## 2. 제안 방법론
### Socket+Plug (SoP) Framework

#### Non-Collective Calibration Strategy
- Retains exclusive optimizer for each predicted target within Plug
- Assigns dedicated early-stopping monitor per target variable per prediction horizon
- Keeps fully trained Socket backbone frozen during calibration
- Enables independent, adequate training for each target with different learnability

#### Key Distinction from Collective Calibration
- **Collective**: Single optimizer and early-stopping for all targets across all horizons
- **Non-Collective**: Individual optimizer and early-stopping for each target or grouped targets

#### Technical Implementation
1. Socket: Backbone network (pre-trained, frozen during calibration)
2. Plug: Target-specific refinement modules
3. Exclusive optimization: Per-target learning with independent early-stopping
4. Allows targets with different learning curves to converge at optimal points

### Innovation
By treating each prediction target (variable × horizon) as an independent optimization problem rather than a collective one, the method achieves better utilization of model capacity and prevents slow-learning targets from being held back by fast-learning targets.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Datasets: ETTh1, ETTh2, ECL, Exchange, Weather, Solar-Energy
- Forecast horizons: 96, 192, 336, 720 steps
- Normalization: Standard normalization
- Train/Val/Test split: Following standard protocols
- Base models tested: iTransformer, DLinear, CALF, and others

※ Specific hyperparameter details (batch size, optimizer, LR) not fully detailed in accessible sources.

### 3.2 Results
**Performance Improvement with Non-collective SoP:**
- Up to 22% improvement with simple MLP as Plug
- Consistent improvements on ETTh1 and ETTh2
- Model-agnostic effectiveness across different base architectures
- On ETTh1/ETTh2: iTransformer+SoP reduces performance gap vs CALF baseline

Results show effective calibration across multiple benchmark datasets including electricity, traffic, weather, and exchange rate datasets.

## 4. 주요 기여
- Identifies multi-target learning conflict in forecasting calibration processes
- Proposes non-collective calibration strategy addressing independent target optimization
- Demonstrates model-agnostic nature applicable to any trained forecasting model
- Achieves significant improvements (up to 22%) with minimal additional parameters
- Works with simple Plug architectures (MLP sufficient) rather than complex designs

## 5. 한계 및 후속 연구 아이디어
- Computational cost of maintaining separate optimizers and monitors
- Hyperparameter sensitivity of target-specific early-stopping thresholds
- Scalability to very large numbers of targets
- Optimal grouping strategies for targets with similar characteristics

## 6. 참고
- arXiv: https://arxiv.org/abs/2506.03176
- IJCAI Proceedings: https://www.ijcai.org/proceedings/2025/371
- Paper PDF: https://www.ijcai.org/proceedings/2025/0371.pdf
