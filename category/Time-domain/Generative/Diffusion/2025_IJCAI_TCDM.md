---
title: TCDM - A Temporal Correlation-Empowered Diffusion Model for Time Series Forecasting
authors: Huibo Xu, Likang Wu, Xianquan Wang, Zhiding Liu, Qi Liu
venue: IJCAI 2025
year: 2025
paper_url: https://www.ijcai.org/proceedings/2025/0749.pdf
code_url: ""
tags: [diffusion, temporal-correlation, decomposition, trend-residual]
primary_category: Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_TCDM.pdf
pdf_source: ijcai
---

## TL;DR
TCDM addresses the challenge of preserving temporal correlations in diffusion-based time series forecasting. By decomposing series into trend and residual components and introducing Maintaining Temporal Correlation Module (MTCM) and Redesigned Initial Module (RIM), the model captures short and mid-range temporal dependencies while avoiding corruption from independent noise.

## 1. 기존 모델의 한계 / 가설
- Diffusion models struggle to preserve intrinsic temporal correlations in time series
- Forward process adds i.i.d. noise, gradually diminishing temporal correlations
- Reverse process starts with i.i.d. noise without temporal priors, causing directional biases
- From frequency perspective, noise disrupts low-frequency-dominated trend structures
- Uniform diffusion treatment inappropriate for heterogeneous trend and residual components
- Independent noise assumption conflicts with sequential temporal dependencies

## 2. 제안 방법론
### Decomposition Prediction Framework
- Decomposes time series into **Trend** and **Residual** components
- Separate modeling strategy for each component type
- Trend captured by frequency-domain MLP (preserves sequence structure)
- Residual processed by specialized diffusion model

### Core Modules

#### Maintaining Temporal Correlation Module (MTCM)
- Captures short-range and mid-range temporal dependencies
- Preserves temporal ordering during diffusion process
- Prevents loss of sequential relationships through noise injection
- Ensures dependencies within neighborhoods are maintained

#### Redesigned Initial Module (RIM)
- Improves initialization of diffusion reverse process
- Incorporates temporal correlation priors
- Better alignment with temporal structure vs standard noise initialization
- Reduces directional biases in sampling

### Base Model Selection
- **Frequency-domain MLP** chosen as base model for trend
- Rationale: Does not distort original sequence structure
- Captures long-range temporal dependencies effectively
- Avoids additional sequence corruption

### Innovation
By explicitly maintaining temporal correlations through dedicated modules and decomposing trend/residual, the framework addresses fundamental challenges in applying diffusion models to sequential data where temporal ordering is critical.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Datasets: ETTh1, ETTh2 (hourly electricity transformer temperature data)
- Forecast horizons: 96, 192, 336, 720 steps
- Decomposition: Trend and residual separation
- Base model: Frequency-domain MLP for trend
- Diffusion model: For residual component refinement
- Normalization: Standard normalization

※ Specific hyperparameter details (batch size, optimizer, learning rate, diffusion steps) not fully detailed in accessible sources.

### 3.2 Results
Comprehensive evaluation on ETTh1 and ETTh2 datasets showing:
- Effective preservation of temporal correlations
- Competitive performance on standard benchmark horizons
- Ablation studies validating MTCM and RIM contributions
- Demonstrates advantages of decomposition + diffusion approach

(Specific numeric results table not accessible in current summaries; refer to paper Table results sections for detailed MSE/MAE values across horizons.)

## 4. 주요 기여
- Identifies and addresses temporal correlation preservation problem in diffusion-based TSF
- Proposes decomposition prediction framework separating trend and residual
- Introduces MTCM for maintaining short/mid-range temporal dependencies
- Introduces RIM for improved diffusion initialization with temporal priors
- Demonstrates frequency-domain MLP effectiveness for trend modeling
- Shows comprehensive evaluation on ETTh1/ETTh2 benchmarks

## 5. 한계 및 후속 연구 아이디어
- Computational cost of decomposition plus dual-model inference
- Decomposition quality dependency and parameter sensitivity
- Extension to multi-level hierarchical decomposition
- Adaptive component separation based on data characteristics
- Long-horizon performance beyond 720 steps

## 6. 참고
- IJCAI Proceedings: https://www.ijcai.org/proceedings/2025/749
- Paper PDF: https://www.ijcai.org/proceedings/2025/0749.pdf
- DOI: 10.24963/ijcai.2025/749
- Pages: 6732-6739
