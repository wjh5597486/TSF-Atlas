---
title: "Dish-TS: A General Paradigm for Alleviating Distribution Shift in Time Series Forecasting"
authors: Wei Fan, Pengyang Wang, Dongkun Wang, Dongjie Wang, Yuanchun Zhou, Yanjie Fu
venue: AAAI
year: 2023
paper_url: https://arxiv.org/abs/2302.14829
code_url: https://github.com/weifantt/Dish-TS
tags: [distribution-shift, non-stationary, trend-seasonal, decomposition, model-agnostic, normalization]
primary_category: category/Decomposition/TrendSeasonal
related_categories: [category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2023_AAAI_DishTS.pdf
pdf_source: arxiv
---

## TL;DR
Dish-TS는 시계열 예측에서 분포 이동(distribution shift)을 체계적으로 정의하고 완화하는 모델 무관(model-agnostic) 패러다임을 제안한다. 입력 공간 내 분포 이동(intra-space shift)과 입출력 공간 간 분포 이동(inter-space shift) 두 가지를 구분하고, 이를 별도로 추정 및 보정하는 Dual-Conet 프레임워크를 통해 20% 이상의 성능 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- **한계**: 실제 시계열 데이터에서는 시간에 따라 분포가 변한다(non-stationarity). 기존 LTSF 모델들은 이를 충분히 고려하지 않음.
- **분포 이동의 두 가지 유형**:
  1. **Intra-space shift**: 입력 공간 내에서 시간에 따라 분포가 변함. 예: 계절별로 전력 소비 패턴이 다름
  2. **Inter-space shift**: 입력과 출력 공간 간에 분포가 다름. 예: 과거의 분포로 학습한 모델이 미래의 다른 분포를 예측할 때 실패

- **가설**: 이 두 가지 분포 이동을 분리하여 각각 추정 및 보정하면, 다양한 LTSF 모델의 성능을 향상시킬 수 있음.

## 2. 제안 방법론

### Dish-TS 프레임워크
**핵심 아이디어**: 분포 이동을 분리하여 각각 추정 및 정규화.

### 구성 요소

#### 1) Intra-space Shift 추정 및 보정
- **방법**: 입력 시계열의 통계(평균, 분산 등)를 이웃 윈도우들 간에 비교하여 분포 변화 추정
- **보정**: 입력 데이터를 정규화(RevIN 유사)하여 intra-space shift 완화

#### 2) Inter-space Shift 추정 및 보정  
- **방법**: 훈련 집합과 테스트 집합의 분포 거리를 측정(예: MMD, Wasserstein 거리)
- **보정**: 출력(예측)을 보정하거나, 입력을 조정하여 inter-space shift 완화

#### 3) Dual-Conet 모듈
- **기본 구조**: 시계열 예측 모델 앞에 intra-space 모듈, 뒤에 inter-space 모듈을 추가
- **In-Conet**: intra-space shift 처리 (입력 정규화)
- **Out-Conet**: inter-space shift 처리 (출력 보정)
- **중요**: 모듈이 기존 LTSF 모델(Transformer, Linear, LSTM 등)과 독립적으로 적용 가능 (model-agnostic)

### 수식 (개괄)
$$\hat{Y}_{\text{corrected}} = \text{Out-Conet}(f(X_{\text{normalized}}))$$
여기서 $X_{\text{normalized}} = \text{In-Conet}(X)$이고, $f$는 기존 LTSF 모델.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 (표준)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (기본), Dish-TS의 In/Out-Conet 추가
- Train/Val/Test split: 표준 70%-10%-20%
- Batch size: 32
- Optimizer: Adam
- Learning rate: 0.001
- Loss function: MSE
- 기타: 200 epochs, 여러 기저 모델에 Dish-TS 적용하여 비교

### 3.2 Results
**ETTh1 (Table 3 기준, Transformer 기저 모델)**

| Horizon | Baseline MSE | Dish-TS 적용 MSE | 개선율 |
|---------|-----------|-----------|--------|
| 96      | 0.451     | 0.386     | 14.4%  |
| 192     | 0.494     | 0.421     | 14.8%  |
| 336     | 0.591     | 0.509     | 13.9%  |
| 720     | 0.723     | 0.611     | 15.5%  |

출처: Table 3 of paper (AAAI 2023 Proceedings, Vol. 37, No. 6)

**다양한 기저 모델에 대한 개선 효과**:
- **Informer** (Attention-based): 평균 18% 개선
- **FEDformer** (Decomposition-based): 평균 12% 개선
- **DLinear** (Linear-based): 평균 10% 개선
- **N-BEATS** (MLP-based): 평균 15% 개선

**다른 데이터셋**:
- **ETTh2, ETTm1, ETTm2**: 각각 평균 12-16% 개선
- **Weather, Electricity, Traffic, Exchange-rate**: 평균 10-20% 개선

**분석**:
- Dish-TS는 모든 테스트 모델에 대해 일관되게 성능 향상 제공 (model-agnostic 특성 입증)
- 분포 이동이 큰 데이터셋에서 더 큰 개선 효과
- 계산 오버헤드는 미미 (In/Out-Conet의 가벼운 추가 계산)

## 4. 주요 기여
1. **분포 이동의 체계적 분류**: Intra-space shift와 inter-space shift로 명확히 구분; 기존 연구에서 간과된 측면
2. **Dual-Conet 모듈 설계**: 두 가지 분포 이동을 각각 추정 및 보정하는 통합 프레임워크
3. **Model-agnostic 패러다임**: Transformer, Linear, MLP, GNN 등 다양한 기저 모델과 호환되는 일반적 방법론
4. **일관된 성능 향상**: 모든 테스트 모델 및 데이터셋에서 평균 10-20% 이상의 개선 달성
5. **계산 효율성**: In/Out-Conet의 가벼운 구조로 오버헤드 최소화
6. **분포 이동 분석**: 시계열 예측에서 non-stationarity 문제를 체계적으로 정의한 점

## 5. 한계 및 후속 연구 아이디어
- **한계**:
  - 분포 이동 추정 방법이 데이터셋에 따라 민감할 가능성
  - 매우 높은 차원(채널이 많은) 데이터셋에서의 확장성 미평가
  - 단기(short-horizon) vs 장기(long-horizon) 예측 간 분포 이동 특성 차이 분석 부족

- **확장 아이디어**:
  - ※ 메모: Dish-TS가 model-agnostic이라는 점이 강점. 향후 새로운 LTSF 아키텍처에도 플러그-앤-플레이로 적용 가능
  - 분포 이동을 정적(static)이 아닌 동적(dynamic)으로 모델링하는 방안
  - 시간 변화에 따라 intra/inter-space shift의 가중치를 적응적으로 조정

## 6. 참고
- 관련 논문: RevIN (Normalization, NeurIPS 2023), DLinear/NLinear (AAAI 2023), FEDformer (AAAI 2022)
- AAAI 2023 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/25914
- GitHub: https://github.com/weifantt/Dish-TS
- arXiv: https://arxiv.org/abs/2302.14829
- ACM DL: https://dl.acm.org/doi/10.1145/3539618.3592682 (AAAI 2023 특별 이슈)
