---
title: "Deep Frequency Derivative Learning for Non-stationary Time Series Forecasting"
authors: Songyuan Niu et al.
venue: IJCAI 2024
year: 2024
paper_url: https://arxiv.org/abs/2407.00502
code_url: 
tags: [freq-domain, fft, non-stationary, derivative, convolution, normalization, mlp]
primary_category: category/Freq-domain/FFT/Architecture/MLP
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_IJCAI_DERITS.pdf
pdf_source: arxiv
---

## TL;DR
DERITS는 주파수 도메인에서의 미분(Frequency Derivative Transformation, FDT)을 통해 시계열의 비정상성을 효과적으로 처리하는 새로운 비정상 시계열 예측 프레임워크다. 기존 정규화 방법(RevIN 등)이 0 주파수 성분만 다루는 한계를 극복하고, Order-adaptive Fourier Convolution Network로 다차 미분 정보를 활용한다.

## 1. 기존 모델의 한계 / 가설
- RevIN, SAN 등 기존 normalization 방법은 0 주파수 성분(평균/분산)만 처리하여 전체 분포 정보를 충분히 활용하지 못함.
- 이 한계가 forecasting의 information utilization bottleneck을 유발함.
- 주파수 도메인 미분(FDT)이 더 정상적인(stationary) 주파수 표현을 제공한다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Frequency Derivative Transformation(FDT)으로 주파수 도메인 미분 수행 → 더욱 정상적인 표현 획득 → Order-adaptive Fourier Convolution Network로 다차 미분 정보 융합.
- **아키텍처 구성요소**:
  - **FDT (Frequency Derivative Transformation)**: 주파수 도메인에서 미분을 적용하여 비정상 성분 처리
  - **Order-adaptive Fourier Convolution Network (OFCN)**: 다차(multi-order) FDT를 병렬로 적용하고 adaptive 주파수 필터링으로 각 차수의 특징 추출
  - Parallel-stacked architecture: 다차 미분 결과를 융합하여 최종 예측
- **손실함수**: MSE
- **핵심 수식**: $\text{FDT}^{(k)}(X) = (j\omega)^k \cdot \mathcal{F}(X)$ (k차 주파수 미분)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: FDT 적용 후 정상화
- Train/Val/Test split: Exchange, Weather, Traffic, Electricity, ETTh1, ETTm1 데이터셋
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 Transformer 기반 모델들(Informer, Autoformer, FEDformer, PatchTST) 및 FreTS 대비 MAE/RMSE에서 평균 20% 이상 감소 보고. ETTh1 구체 수치는 원문 참고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- 기존 정규화 방법의 한계(0 주파수 성분만 처리)를 이론적으로 분석
- Frequency Derivative Transformation(FDT) 제안: 주파수 도메인에서 비정상성을 더 완전하게 처리
- Order-adaptive Fourier Convolution Network: 다차 FDT 정보를 적응적으로 융합
- 6개 벤치마크에서 SOTA 대비 20%+ MAE/RMSE 감소

## 5. 한계 및 후속 연구 아이디어
- FDT 차수(order) 선택에 대한 추가적인 가이드라인 필요
- ※ 메모: FDT는 신호처리의 전통적 미분 개념을 딥러닝 forecasting에 접목; 독창적인 관점

## 6. 참고
- 관련 논문: RevIN (ICLR 2022), FreTS (NeurIPS 2023), Non-stationary Transformer (NeurIPS 2022)
- IJCAI 2024 proceedings: https://www.ijcai.org/proceedings/2024/436
- arxiv: https://arxiv.org/abs/2407.00502
