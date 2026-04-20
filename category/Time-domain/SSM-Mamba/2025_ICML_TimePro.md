---
title: "TimePro: Efficient Multivariate Long-term Time Series Forecasting with Variable- and Time-Aware Hyper-state"
authors: Xiaowen Ma et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2505.20774
code_url: https://github.com/xwmaxwma/TimePro
tags: [time-domain, ssm, mamba, multivariate-forecasting, channel-dependent, hyper-state, multi-delay]
primary_category: category/Time-domain/SSM-Mamba
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TimePro.pdf
pdf_source: arxiv
---

## TL;DR
TimePro introduces a Mamba-based model with variable- and time-aware hyper-states to address the "multi-delay" issue in multivariate time series forecasting, where different variables influence the target over distinct time intervals. It achieves competitive LTSF performance with linear complexity across 8 real-world benchmarks.

## 1. 기존 모델의 한계 / 가설
- **Multi-delay 문제**: 서로 다른 변수들이 타겟 변수에 영향을 미치는 시간 지연(lag)이 각기 달라서, 단일 표현으로는 이를 충분히 포착하기 어려움.
- 기존 Mamba 기반 방법들은 각 변수 토큰의 세밀한 시간적 특징을 보존하지 못함.
- Transformer 기반 방법의 이차 복잡도(O(n²)) 문제를 선형 복잡도(O(n))로 해결 필요.

## 2. 제안 방법론
- **핵심 아이디어**: Variable- and Time-Aware Hyper-state를 구성하여 각 변수의 세밀한 시간 특징과 변수 간 관계를 동시에 포착.
- **아키텍처**:
  - Mamba 기반 백본으로 선형 복잡도 달성.
  - Variable-aware component: 변수 간 상관관계를 반영한 hyper-state.
  - Time-aware component: 각 변수의 중요한 시간 포인트를 adaptive하게 선택.
  - Hyper-state가 변수 관계와 시간적 정보를 동시에 인코딩.
- **손실함수**: MSE.
- **학습 전략**: Standard supervised LTSF.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조 (표준 336/512)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 8개 LTSF benchmark datasets 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | competitive |
| 192     | see paper | see paper | competitive |
| 336     | see paper | see paper | competitive |
| 720     | see paper | see paper | competitive |

출처: Table of paper (8 real-world benchmarks).

## 4. 주요 기여
- Multi-delay 문제를 명시적으로 다루는 Variable- and Time-Aware Hyper-state 설계.
- Mamba 기반 선형 복잡도로 효율적인 장기 예측.
- 8개 실제 LTSF benchmark에서 경쟁력 있는 성능.
- 각 변수 토큰의 세밀한 시간 특징을 보존하는 새로운 접근.

## 5. 한계 및 후속 연구 아이디어
- Mamba의 선택적 SSM이 모든 시간적 패턴을 동등하게 처리하지 못할 수 있음.
- ※ 메모: iTransformer처럼 채널-의존 방식을 Mamba 아키텍처에 적용한 것으로 볼 수 있음.

## 6. 참고
- 관련 논문: Chimera (NeurIPS 2024), TimeMachine (ICML 2024), iTransformer (ICLR 2024)
- GitHub: https://github.com/xwmaxwma/TimePro
- arXiv: https://arxiv.org/abs/2505.20774
