---
title: "VCformer: Variable Correlation Transformer with Inherent Lagged Correlation for Multivariate Time Series Forecasting"
authors: Yingnan Yang et al.
venue: IJCAI 2024
year: 2024
paper_url: https://arxiv.org/abs/2405.11470
code_url: https://github.com/CSyyn/VCformer
tags: [transformer, channel-dependent, cross-correlation, lag, koopman, non-stationary, multivariate]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_IJCAI_VCformer.pdf
pdf_source: arxiv
---

## TL;DR
VCformer는 다변수 시계열에 내재한 변수 간 lagged correlation(지연 상관관계)을 Koopman 동역학 기반의 비정상 처리와 결합하는 Transformer 기반 MTSF 모델이다. Variable Correlation Attention(VCA)와 Koopman Temporal Detector(KTD)로 구성되며, 8개 벤치마크(ETTh1 포함)에서 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 변수 간의 lagged correlation(다른 lag에서의 교차상관)을 충분히 활용하지 못함.
- 비정상성(non-stationarity)이 forecasting 성능을 저하시킴.
- Cross-correlation을 다양한 lag에 대해 계산하면 변수 간 관계를 더 효과적으로 포착할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: VCA로 다양한 lag에서의 cross-correlation을 attention score에 통합하고, KTD로 Koopman 이론 기반 비정상 처리.
- **아키텍처 구성요소**:
  - **Variable Correlation Attention (VCA)**: query-key 쌍에 대해 다양한 lag에서의 cross-correlation 점수를 계산하고 통합하여 attention weight 생성
  - **Koopman Temporal Detector (KTD)**: Koopman 연산자 이론에서 영감받아 시계열의 비정상 성분을 분리/처리
  - 표준 Transformer encoder 구조와 결합
- **손실함수**: MSE
- **핵심 수식**: $\text{VCA}(Q,K,V) = \text{softmax}\left(\sum_\tau \text{CrossCorr}(Q,K,\tau)\right) \cdot V$

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, Exchange 8개 데이터셋
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 8개 데이터셋에서 top-tier 성능 보고. ETTh1 구체 수치는 원문 참고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- Lagged cross-correlation을 활용한 Variable Correlation Attention(VCA) 설계
- Koopman 이론 기반 비정상성 처리 (Koopman Temporal Detector)
- 두 모듈의 결합으로 8개 표준 TSF 벤치마크에서 우수한 성능
- 파라미터 효율적 구조

## 5. 한계 및 후속 연구 아이디어
- Lag 탐색 범위가 넓어질수록 계산 비용 증가
- ※ 메모: Koopa(ICLR 2024)와 Koopman 이론 공유; 결합 가능성 있음

## 6. 참고
- 관련 논문: Autoformer (NeurIPS 2021, autocorrelation 기반), iTransformer (ICLR 2024), Koopa (ICLR 2024)
- IJCAI 2024 proceedings: https://www.ijcai.org/proceedings/2024/590
- arxiv: https://arxiv.org/abs/2405.11470
- GitHub: https://github.com/CSyyn/VCformer
