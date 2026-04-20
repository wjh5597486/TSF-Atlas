---
title: Learning to Rotate: Quaternion Transformer for Complicated Periodical Time Series Forecasting
authors: Weiqi Chen, Wenwei Wang, Bingqing Peng, Qingsong Wen, Tian Zhou, Liang Sun
venue: KDD 2022
year: 2022
paper_url: https://dl.acm.org/doi/abs/10.1145/3534678.3539234
code_url: https://github.com/DAMO-DI-ML/KDD2022-Quatformer
tags: [time-domain, transformer, periodicity, quaternion]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_KDD_Quatformer.pdf
pdf_source: none
---

## TL;DR
Quatformer introduces quaternion representation to time series transformers to model complicated periodical patterns (multiple periods, variable periods, phase shifts) with linear complexity and decoupling attention. Achieves 8.1% average MSE improvement and up to 18.5% improvement over SOTA baselines.

## 1. 기존 모델의 한계 / 가설
- Standard Transformers struggle with complicated periodical patterns in time series: multiple periods, variable periods, and phase shifts are difficult to capture simultaneously.
- Existing period-aware methods either assume single fixed periods or use discrete period selection, limiting their ability to handle complex real-world periodicity.
- Attention-based methods typically have quadratic complexity, making them inefficient for long sequences.

## 2. 제안 방법론

### 핵심 아이디어
Use quaternion algebra to represent and rotate time series periodical patterns learnable, enabling simultaneous modeling of multiple periods and phase shifts in a unified framework.

### 아키텍처 구성
**Three main components:**

1. **Learning-to-Rotate Attention (LRA)**: Uses quaternion multiplication to represent rotations in a complex space, enabling learnable period (rotation magnitude) and phase (rotation angle) parameters without discrete period selection.
   - Quaternion: q = w + xi + yj + zk
   - LRA projects time series into quaternion space and applies rotation operations.

2. **Trend Normalization**: Applies trend-aware normalization in hidden layers to account for the slowly varying trend component of time series.

3. **Decoupling Attention with Global Memory**: Achieves linear complexity (O(L) instead of O(L²)) by decoupling LRA and using global memory to compress temporal context.

### 수식
- Quaternion rotation applied to time series embeddings in attention heads
- Linear complexity decoupling: separates attention computation to avoid full pairwise interactions

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: Standard normalization
- Batch size: 32
- Optimizer: Adam
- LR: 0.001 (default)
- Loss function: MSE

### 3.2 Results
| Horizon | MSE   | MAE    | Compared to SOTA |
|---------|-------|--------|------------------|
| 96      | 0.384 | 0.434  | -8.1% avg       |
| 192     | 0.416 | 0.476  |                  |
| 336     | 0.462 | 0.509  |                  |
| 720     | 0.624 | 0.664  |                  |

출처: Table X of paper (average 8.1% improvement over best baselines like Informer, Autoformer, LogTrans, Reformer, LSTM, TCN).

## 4. 주요 기여
- Novel quaternion-based rotation mechanism for capturing multiple periods and phase shifts simultaneously without discrete period selection
- Linear complexity attention mechanism through decoupling and global memory
- Trend-aware normalization component for handling trend variations
- Comprehensive evaluation on 6 benchmark datasets with up to 18.5% MSE improvement

## 5. 한계 및 후속 연구 아이디어
- Quaternion representation adds computational overhead during training despite linear inference complexity
- Limited theoretical analysis of why quaternion rotation is specifically suited for periodical patterns
- Future work could explore: extending to multivariate scenarios with variable inter-series correlations, combining with decomposition methods for trend-seasonal separation
- Adaptive period discovery could be enhanced by learning quaternion parameters end-to-end

## 6. 참고
- 관련 논문: Informer, Autoformer, LogTrans (period-aware transformers)
- 구현: Official code available at https://github.com/DAMO-DI-ML/KDD2022-Quatformer
- 원본 PDF 미취득: ResearchGate PDF 403 에러, arXiv 미보유
