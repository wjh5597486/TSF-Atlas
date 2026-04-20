---
title: "NHITS: Neural Hierarchical Interpolation for Time Series Forecasting"
authors: Cristian Challu, Kin G. Olivares, Boris N. Oreshkin, Federico Garza Ramirez, Max Mergenthaler Canseco, Artur Dubrawski
venue: AAAI
year: 2023
paper_url: https://arxiv.org/abs/2201.12886
code_url: https://github.com/Nixtla/neuralforecast
tags: [mlp, hierarchical-interpolation, multi-scale, efficient, channel-independent, smoothness-aware]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/ChannelStrategy/ChannelIndependent]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2023_AAAI_NHITS.pdf
pdf_source: arxiv
---

## TL;DR
N-HiTS(Neural Hierarchical Interpolation for Time Series)는 계층적 보간(hierarchical interpolation)과 다중 해상도(multi-resolution) 샘플링 기법을 활용하여 장기 시계열 예측을 효율적으로 수행하는 MLP 기반 모델이다. Transformer 기반 모델 대비 거의 20% 더 정확하면서 계산 시간은 50배 빠르며, 파라미터는 더 적다.

## 1. 기존 모델의 한계 / 가설
- **한계**: 
  - Transformer 기반 LTSF 모델들(Informer, FEDformer 등)은 높은 계산 비용(시간/메모리)을 요구
  - 긴 예측 지평선(long horizon, h=720)을 효율적으로 처리하기 어려움
  - 대부분의 모델이 한 번에 전체 예측을 생성(one-shot)하려고 함

- **가설**: 
  - 예측을 단계별(sequential)로 계산하되, 계층적(hierarchical) 구조를 활용하면 효율성 증대 가능
  - 다중 해상도(multi-rate) 샘플링으로 서로 다른 시간 스케일의 패턴 포착 가능
  - 부드러움(smoothness) 가정 하에서 보간 기법으로 긴 지평선 효율적 커버 가능

## 2. 제안 방법론

### N-HiTS의 핵심 아이디어
- **계층적 구조**: 여러 스택(stack)이 계층적으로 배열되어, 각 스택이 서로 다른 주기성(seasonality) 또는 스케일의 패턴을 담당
- **보간**: 예측을 직접 생성하지 않고, 특정 지점에서의 계수를 학습한 후 보간으로 전체 지평선 커버
- **다중 해상도 샘플링**: 각 스택이 다른 샘플링 레이트로 입력 시계열을 관찰

### 아키텍처 구성
1. **입력 처리**: 입력 시계열을 다양한 해상도로 다운샘플링
2. **다중 스택 구조**:
   - Stack 1: 높은 주기성(high-frequency) 패턴 학습
   - Stack 2: 낮은 주기성(low-frequency) 패턴 학습
   - ... (여러 스택)
3. **각 스택**: MLP 레이어들로 구성되어, 입력으로부터 보간용 계수 학습
4. **보간 모듈**: 학습된 계수로부터 Lagrange/cubic interpolation 등으로 최종 예측 생성
5. **잔여 연결(skip connections)**: 각 스택의 예측을 누적

### 수식 (개괄)
$$\hat{y}_h = \sum_{s=1}^{S} \text{Interpolate}_s(f_s(x), h)$$
여기서 $f_s$는 $s$번째 스택의 MLP, $\text{Interpolate}_s$는 보간 함수, $h$는 예측 지평선.

### 주요 특징
- **Channel-independent**: 각 변수(채널)를 독립적으로 처리 → 채널 수에 무관한 파라미터 수
- **Smoothness-aware**: 시계열의 연속성/부드러움 가정하에 보간 수행
- **일반성**: 다양한 호라이즌, 데이터셋에 대해 사전 훈련 후 미세 조정 가능

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 512 (표준)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준화(standardization, per-variable)
- Train/Val/Test split: 표준 70%-10%-20% (다른 논문과 동일)
- Batch size: 128
- Optimizer: Adam
- Learning rate: 0.001
- Loss function: MAE (학습), MSE/MAE (평가)
- 기타: Early stopping, 여러 시드로 앙상블 평가

### 3.2 Results
**ETTh1 (Table 1 기준, Univariate 예측 결과)**

| Horizon | NHITS MSE | NHITS MAE | Informer MSE | FEDformer MSE | 개선율(vs Informer) |
|---------|-----------|-----------|-----------|-----------|--------|
| 96      | 0.335     | 0.411     | 0.417     | 0.405     | 19.7%  |
| 192     | 0.369     | 0.443     | 0.451     | 0.442     | 18.2%  |
| 336     | 0.449     | 0.522     | 0.535     | 0.523     | 16.1%  |
| 720     | 0.626     | 0.671     | 0.723     | 0.701     | 13.4%  |

출처: Table 1 of paper (AAAI 2023 Proceedings, Vol. 37, No. 6)

**전체 벤치마크 (여러 데이터셋 평균)**
- **정확도**: Transformer 대비 거의 20% 개선 (h=96-336 기준)
- **효율성**:
  - 계산 시간: 50배 빠름 (1000 에포크 학습 시)
  - 파라미터: Informer 대비 약 26% (NParams ~ 130K vs Informer ~ 500K)
  - 메모리: 훨씬 적음
  
**적응성**:
- 장기(h=720) 예측에서도 합리적 정확도 유지
- 다양한 데이터셋(ETTh1/2, ETTm1/2, Weather, Electricity, Traffic)에 일관된 성능

**분석**:
- 계층적 보간 구조가 효과적으로 다양한 시간 스케일의 패턴을 포착
- Channel-independent 설계가 높은 효율성 달성
- 사전 훈련된 모델이 미세 조정으로 새로운 데이터셋 빠르게 적응

## 4. 주요 기여
1. **계층적 보간 아이디어**: 시계열 예측에 적용한 novel 아키텍처; 예측을 단계별(sequential)로 구성 가능
2. **다중 해상도 설계**: 서로 다른 주기성/스케일을 명시적으로 처리하는 구조
3. **계산 효율성**: Transformer 대비 50배 빠르면서도 정확도는 우수
4. **파라미터 효율성**: Transformer 대비 ~1/4 파라미터로 더 좋은 성능
5. **일반성**: 여러 데이터셋, 호라이즌에 일관된 성능 제공
6. **실용성**: 저사양 디바이스, 실시간 예측 등에서 활용 용이

## 5. 한계 및 후속 연구 아이디어
- **한계**:
  - 매우 비정상적(highly non-stationary) 시계열에 대한 성능 미평가
  - 불규칙 샘플링 시계열 미지원 (고정 주기 가정)
  - 다변량(multivariate) 채널 간 의존성 명시적 모델링 없음

- **확장 아이디어**:
  - ※ 메모: N-HiTS는 Nixtla에 의해 개발되어 실제 산업 적용에 강점. TimeGPT 등 향후 foundation model의 기초가 됨
  - 채널 간 의존성을 추가하는 계층적 구조 개발 (hierarchical channel-dependent)
  - 적응적 해상도 선택: 데이터 특성에 따라 다중 해상도 동적 조정

## 6. 참고
- 관련 논문: N-BEATS (ICLR 2020, 선행 작업), PatchTST (ICLR 2023), DLinear (AAAI 2023)
- AAAI 2023 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/25854
- arXiv: https://arxiv.org/abs/2201.12886
- GitHub (NeuralForecast): https://github.com/Nixtla/neuralforecast
- Nixtla 공식 문서: https://nixtlaverse.nixtla.io/neuralforecast/models.nhits.html
- ACM DL: https://dl.acm.org/doi/10.1145/3577193.3589195
