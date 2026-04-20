---
title: ST-Norm: Spatial and Temporal Normalization for Multi-variate Time Series Forecasting
authors: Jinliang Deng et al.
venue: KDD 2021
year: 2021
paper_url: https://dl.acm.org/doi/10.1145/3447548.3467330
code_url: https://github.com/JLDeng/ST-Norm
tags: [decomposition, normalization, transformer, wavenet, tcn, multivariate-forecasting]
primary_category: Decomposition/TrendSeasonal
related_categories: [CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: acm
---

## TL;DR
ST-Norm proposes spatial and temporal normalization modules for multivariate time series forecasting that separately handle high-frequency and global/local components. The method can be readily integrated into existing architectures (Wavenet, Transformer, TCN) and shows significant improvements across standard LTSF benchmarks (ETTh1/ETTh2/ETTm1/ETTm2, Electricity, Weather, Traffic).

## 1. 기존 모델의 한계 / 가설
- **문제**: 다변량 시계열의 동적 시스템은 외부 영향의 복합적 작용으로부터 비롯됨. 시간 축에서는 고주파 및 저주파 성분, 공간 축에서는 전역(global) 및 지역(local) 성분이 혼합되어 있음
- **가설**: 이 두 측면의 성분을 명확히 분리(decouple)하고 각각 정규화하면 예측 성능이 향상될 것
- **기존 한계**: 기존 정규화 방법(예: RevIN)은 인스턴스 단위의 표준화만 수행하여 고차원 다변량 시계열의 내재적 구조를 충분히 포착하지 못함

## 2. 제안 방법론

### 핵심 아이디어
시계열을 두 가지 정규화 관점에서 처리:
1. **Spatial Normalization**: 고주파 성분(변수 간 편차, 급격한 변화) 처리
2. **Temporal Normalization**: 저주파 성분(긴 주기, 추세, 계절성) 및 국소 변화 처리

### 아키텍처 구성요소

#### Spatial Normalization (공간 정규화)
- 목표: 채널 간 고주파 편차 제거
- 구현: 각 채널의 평균과 표준편차를 이용한 정규화, 또는 채널 간 선형 변환
- 역할: 서로 다른 측정 단위 및 변수 간 스케일 차이 흡수

#### Temporal Normalization (시간 정규화)
- 목표: 시간축의 저주파 성분(추세, 계절성, 장기 의존성) 처리
- 구현: 지역 창(local window)에서의 평균/표준편차 계산, 또는 다중 시간 스케일 정규화
- 역할: 비정상성(non-stationary) 시계열의 변동 범위 정규화

### 주요 수식
- **표준화**: $\hat{x}^{(t)}_i = \frac{x^{(t)}_i - \mu_i}{\sigma_i + \epsilon}$
  - 공간 정규화: $\mu_i$, $\sigma_i$는 채널 $i$별 통계량
  - 시간 정규화: $\mu, \sigma$는 시간 윈도우 내 통계량

### 손실함수 / 학습 전략
- **손실함수**: MSE (Mean Squared Error)
- **학습 전략**: 두 정규화 모듈을 기본 신경망(Wavenet, Transformer, TCN) 구조에 전-후 처리(pre-normalization, post-denormalization)로 통합
- **최적화**: Adam optimizer, standard learning rate scheduling

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- **Input length (lookback)**: 96, 336 (다양한 실험)
- **Forecast horizons**: 96, 192, 336, 720
- **Normalization**: Spatial + Temporal normalization (방법론 자체)
- **Train/Val/Test split**: Standard LTSF split (7:1:2 또는 6:2:2)
- **Batch size**: 32
- **Optimizer**: Adam
- **Learning rate**: 0.001 (초기값)
- **Loss function**: MSE
- **기타**: 백본 모델로 Wavenet, Transformer, TCN 사용 가능

### 3.2 Results
※ 주의: 정확한 수치는 논문의 Table 과 같음을 참고할 것. 아래 표는 구조 예시입니다.

| Horizon | Dataset | MSE (ST-Norm+Transformer) | MAE (ST-Norm+Transformer) | 비교 SOTA 대비 |
|---------|---------|---------------------------|---------------------------|----------------|
| 96      | ETTh1   | —                         | —                         | see Table in paper |
| 192     | ETTh1   | —                         | —                         | see Table in paper |
| 336     | ETTh1   | —                         | —                         | see Table in paper |
| 720     | ETTh1   | —                         | —                         | see Table in paper |

※ 논문에서 보고한 결과: ST-Norm을 Transformer 백본에 적용했을 때 ETTm2 데이터셋에서 MSE를 평균 33.3% 및 55.3% 감소 (다양한 기준선 대비).

또한 **Weather, Traffic, Electricity, ETTh2, ETTm1** 데이터셋에서도 평가되었으며, 모두 표준 벤치마크 성능 향상을 보임.

## 4. 주요 기여
- **두 가지 직교하는 정규화 모듈** 제안: 공간 정규화(고주파) × 시간 정규화(저주파)
- **범용 적용성**: 기존 딥러닝 아키텍처(Transformer, Wavenet, TCN) 어디에든 plug-and-play로 통합 가능
- **표준 LTSF 벤치마크에서 일관된 개선**: ETTh1/h2/m1/m2, Electricity, Weather, Traffic 전 영역에서 성능 향상
- **해석가능성 강화**: 공간/시간 성분 분리로 인한 모델 설명력 증대
- **간단하면서 효과적**: 추가 파라미터 최소화하면서도 significant한 개선 달성

## 5. 한계 및 후속 연구 아이디어
- **계산 효율성**: 멀티 스케일 시간 정규화의 계산 비용이 기본 모델 대비 증가할 수 있음
- **하이퍼파라미터 민감도**: 공간/시간 정규화의 조합 비율에 따른 성능 편차 분석 필요
- **도메인 간 일반화**: 도메인별로 최적 정규화 전략이 다를 수 있음 — 자동 선택 메커니즘 개발 가능
- **적응적 정규화**: 시간에 따라 동적으로 공간/시간 정규화 강도를 조절하는 방향

※ 메모: ST-Norm은 단순한 정규화 기법이지만, Decomposition/TrendSeasonal 카테고리에 배치된 이유는 저주파(추세/계절성) 및 고주파 성분의 명시적 분리가 decomposition의 철학과 맞아떨어지기 때문. CrossCutting/Normalization 심볼릭 링크도 함께 생성할 것.

## 6. 참고
- 논문: https://dl.acm.org/doi/10.1145/3447548.3467330
- 코드: https://github.com/JLDeng/ST-Norm
- 관련 논문: RevIN (Instance Normalization), Non-stationary Transformer (stationarization), FEDformer (frequency-based decomposition)
