---
title: "Temporal Query Network for Efficient Multivariate Time Series Forecasting"
authors: Shengsheng Lin et al.
venue: ICML
year: 2025
paper_url: https://arxiv.org/abs/2505.12917
code_url: https://github.com/ACAT-SCUT/TQNet
tags: [time-domain, mlp, attention, channel-dependent, multivariate-forecasting, periodicity]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_ICML_TQNet.pdf
pdf_source: arxiv
---

## TL;DR
TQNet proposes a Temporal Query (TQ) technique using periodically shifted learnable vectors as queries to capture global inter-variable patterns in multivariate time series forecasting. Using only a single attention layer and a lightweight MLP, it achieves state-of-the-art accuracy across 12 real-world datasets with minimal computation.

## 1. 기존 모델의 한계 / 가설
- 기존 채널-독립 방식(PatchTST 등)은 변수 간 의존성을 무시하여 성능 한계 존재.
- 채널-의존 방법들(iTransformer 등)은 복잡한 구조로 연산 비용이 높음.
- 단일 attention layer + MLP만으로도 강력한 inter-variable 상관관계 포착이 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Temporal Query(TQ) — 주기적으로 이동된 learnable vector를 query로 사용하여 global inter-variable pattern 캡처.
- **아키텍처**:
  - Single-layer attention: TQ를 query로, 원시 입력에서 key/value 도출.
  - Lightweight MLP: 예측 헤드 역할.
  - 전체 파라미터 수가 매우 적음 (경량 설계).
- **손실함수**: MSE (표준).
- **핵심 특징**: 주기적으로 이동된 query가 시간적 주기성(periodicity)을 내재적으로 활용.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (논문 기본값, 512도 실험)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 12개 실제 데이터셋 평가 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, Solar, PEMS, Exchange, ILI 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | SOTA 수준 |
| 192     | see paper | see paper | SOTA 수준 |
| 336     | see paper | see paper | SOTA 수준 |
| 720     | see paper | see paper | SOTA 수준 |

출처: Table 1 of paper (12 datasets, 86% avg win ratio vs baselines).

## 4. 주요 기여
- Temporal Query(TQ) 기법으로 주기성을 이용한 전역 inter-variable 패턴 포착.
- 단일 attention layer + MLP만으로 SOTA 달성 — 구조적 단순성과 높은 성능의 양립.
- 12개 실제 데이터셋에서 86% 평균 win ratio로 기존 SOTA 능가.
- 연산 효율성: 파라미터 수와 계산량 모두 매우 적음.

## 5. 한계 및 후속 연구 아이디어
- Temporal Query가 주기성이 약한 데이터셋(Exchange 등)에서의 효과는 제한적일 수 있음.
- 매우 긴 look-back window(1024 이상)에서의 스케일링 효과 탐구 필요.
- ※ 메모: CycleNet의 periodicity 아이디어를 inter-variable attention으로 확장한 형태.

## 6. 참고
- 관련 논문: CycleNet (NeurIPS 2024), SparseTSF (ICML 2024), iTransformer (ICLR 2024)
- GitHub: https://github.com/ACAT-SCUT/TQNet
- PMLR: https://proceedings.mlr.press/v267/
