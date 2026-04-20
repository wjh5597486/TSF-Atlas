---
title: "OneNet: Enhancing Time Series Forecasting Models under Concept Drift by Online Ensembling"
authors: Yifan Zhang et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2309.12659
code_url: https://github.com/yfzhang114/OneNet
tags: [online-learning, concept-drift, ensemble, time-domain, forecasting, channel-independent, channel-dependent]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_OneNet.pdf
pdf_source: arxiv
---

## TL;DR
OneNet은 시계열 개념 표류(concept drift) 문제를 해결하기 위해, 시간 의존성 모델과 변수 간 의존성 모델을 온라인으로 동적 결합하는 앙상블 프레임워크다. 강화학습 기반의 온라인 볼록 프로그래밍을 사용해 두 모델의 가중치를 스트리밍 데이터에 적응적으로 조정한다.

## 1. 기존 모델의 한계 / 가설
- 실세계 시계열은 지속적으로 분포가 변화(concept drift)하지만, 기존 모델들은 오프라인 훈련만 수행하여 이에 적응하지 못한다.
- 시간 의존성과 변수 간 의존성 중 어느 것이 더 중요한지는 데이터와 시점에 따라 달라진다.
- 가설: 두 가지 의존성 모델을 동적으로 결합하면 개념 표류에 강건한 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 시간 차원 의존성 모델(CI)과 변수 간 의존성 모델(CD)을 온라인 앙상블로 결합, 강화학습 기반 가중치 동적 조정.
- **아키텍처**:
  1. **시간 의존성 모델**: Channel-Independent(CI) 방식으로 각 변수 독립 처리.
  2. **변수 간 의존성 모델**: Channel-Dependent(CD) 방식으로 변수 간 상호작용 학습.
  3. **온라인 볼록 프로그래밍**: 스트리밍 데이터에서 두 모델 가중치를 강화학습으로 동적 업데이트.
- **손실함수**: MSE + 온라인 학습 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할 (스트리밍 방식 평가)
- Batch size / Optimizer / LR: Adam; 온라인 학습 설정
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | see Table 1 |
| 192     | see paper | see paper | see Table 1 |
| 336     | see paper | see paper | see Table 1 |
| 720     | see paper | see paper | see Table 1 |

출처: Table 1 of paper (ETTh1, ETTh2, ETTm1, ETTm2, ECL, Traffic, WTH 포함).

## 4. 주요 기여
- 온라인 앙상블 방식으로 CI/CD 두 모델을 동적 결합.
- 강화학습 기반 온라인 볼록 프로그래밍으로 개념 표류 적응.
- 7개 벤치마크에서 오프라인 SOTA 모델 대비 성능 유지 및 개선.
- 실시간 스트리밍 예측 시나리오에 최초 적용.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 온라인 업데이트 계산 비용이 오프라인 방법보다 높음.
- ※ 메모: Test-time adaptation(TTA) 시계열 연구의 초기 주요 작업 중 하나. 이후 TEST(2024) 등과 연결.

## 6. 참고
- arXiv: https://arxiv.org/abs/2309.12659
- GitHub: https://github.com/yfzhang114/OneNet
- 관련: TTT, Test-time adaptation 계열 논문들
