---
title: FreEformer - Frequency Enhanced Transformer for Multivariate Time Series Forecasting
authors: Wenzhen Yue, Yong Liu, Xianghua Ying, Bowei Xing, Ruohao Guo, Ji Shi
venue: IJCAI 2025
year: 2025
paper_url: https://arxiv.org/abs/2501.13989
code_url: https://github.com/jackyue1994/FreEformer
tags: [freq-domain, fft, transformer, frequency-enhanced]
primary_category: Freq-domain/FFT/Architecture/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_FreEformer.pdf
pdf_source: arxiv
---

## TL;DR
FreEformer converts time series to the complex frequency domain using Discrete Fourier Transform and applies a Transformer with enhanced attention mechanism to capture cross-variate dependencies. The learnable matrix addition and L1 normalization in attention improve representation diversity and gradient flow, achieving consistent improvements across 18 real-world benchmarks.

## 1. 기존 모델의 한계 / 가설
- Vanilla Transformer attention matrix exhibits low-rank characteristic, limiting representation diversity
- Low-rank issue stems from sparsity of frequency domain and strong-value-focused nature of Softmax in standard attention
- Frequency spectrum provides global perspective on series composition across various frequencies, suitable for robust representation learning
- Processing real and imaginary parts of complex frequency domain independently can better capture cross-variate dependencies

## 2. 제안 방법론
### Core Architecture
- Converts time series to complex frequency domain using Discrete Fourier Transform (DFT)
- Applies Transformer architecture to frequency spectra
- Processes real and imaginary parts independently for cross-variate dependencies

### Attention Enhancement
- Introduces learnable matrix to original attention mechanism
- Performs row-wise L1 normalization on enhanced attention matrix
- Addresses low-rank representation issues and improves gradient flow
- Enhances feature diversity in attention computation

### Key Innovation
The frequency-enhanced attention mechanism allows the model to leverage global frequency information while maintaining diversity in learned representations, avoiding collapse to low-rank solutions common in vanilla attention on sparse frequency data.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Input length: Not explicitly stated in available summaries
- Forecast horizons: 96, 192, 336, 720 steps
- Normalization: Standard normalization (frequency domain)
- Dataset: ETTh1 (hourly electricity transformer temperature with 7 variables)
- Test period: 2018년

※ Note: Complete hyperparameter details (batch size, optimizer, LR) not fully available in accessible summaries.

### 3.2 Results
Evaluation on 18 real-world benchmarks covering:
- Electricity domain (ETTh1, ETTh2)
- Traffic datasets
- Weather datasets
- Healthcare datasets
- Finance datasets

Model demonstrates "consistent improvements over state-of-the-art methods" on all tested benchmarks (specific numeric results not provided in accessible sources).

## 4. 주요 기여
- Proposes frequency-enhanced attention mechanism for time series forecasting
- Addresses representation diversity problems in vanilla Transformer attention on frequency domain data
- Simple yet effective approach that outperforms SoTA across diverse domains (electricity, traffic, weather, healthcare, finance)
- Demonstrates the value of combining frequency-domain representation with enhanced attention mechanisms

## 5. 한계 및 후속 연구 아이디어
- Frequency-domain processing may lose temporal ordering information
- L1 normalization adds computational overhead
- Future work could explore adaptive frequency band selection or multi-scale frequency analysis

## 6. 참고
- arXiv: https://arxiv.org/abs/2501.13989
- GitHub: https://github.com/jackyue1994/FreEformer
- IJCAI Proceedings: https://www.ijcai.org/proceedings/2025/401

※ 원본 PDF 미취득: Full paper content with detailed results tables requires direct PDF access from arxiv or IJCAI proceedings.
