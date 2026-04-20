---
title: "Pyraformer: Low-Complexity Pyramidal Attention for Long-Range Time Series Modeling and Forecasting"
authors: Shizhan Liu, Hang Yu, Cong Liao, Jianguo Li, Weiyao Lin, Alex X. Liu, Schahram Dustdar
venue: ICLR 2022
year: 2022
paper_url: https://arxiv.org/abs/2202.07125
code_url: https://github.com/ant-research/Pyraformer
tags: [transformer, attention, pyramidal-attention, multi-scale, multi-resolution, long-range, time-domain, forecasting]
primary_category: category/Time-domain/Transformer
related_categories: [category/Decomposition/MultiScale]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_ICLR_Pyraformer.pdf
pdf_source: arxiv
---

## TL;DR
Pyraformer는 다중해상도 표현을 활용하는 저복잡도 피라미드 주의(pyramidal attention)를 제시한다. 신호 경로의 최대 거리를 상수로 유지(O(1))하면서 시간·공간 복잡도는 선형(O(L))으로 확장하며, ICLR 2022 oral 발표를 받았다.

## 1. 기존 모델의 한계 / 가설
- 표준 Transformer의 자주의(self-attention)는 시퀀스 길이 L에 대해 O(L²) 복잡도를 가져 긴 시계열에 부적합하다.
- 시계열의 장거리 의존성을 모델링하려면 깊은 네트워크가 필요한데, 이는 그래디언트 흐름 문제를 야기한다.
- 가설: 시계열의 계층적 구조와 다중해상도 표현을 이용한 피라미드 구조라면 O(L) 복잡도로 긴 시퀀스를 효율적으로 모델링할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 피라미드 그래프로 시계열의 시간 의존성을 다중해상도로 표현. 계층 간(inter-scale)과 계층 내(intra-scale) 연결로 다양한 범위의 의존성 모델링.
  
- **아키텍처**:
  1. **Pyramidal Graph 구성**
     - 최상단 부모(parent) 노드에서 인접한 자식(child) 노드들을 연결
     - 각 계층은 이전 계층의 정보를 집계하여 다중해상도 표현 생성
  
  2. **Pyramidal Attention Module (PAM)**
     - 계층 간(inter-scale) 주의: 부모 노드가 자식 노드들 간 의존성을 요약
     - 계층 내(intra-scale) 주의: 같은 해상도의 인접 노드들 간 시간적 의존성 모델링
     - 신호 경로 최대 거리: O(1) (log L이 아님)
  
  3. **Encoder-Decoder 구조**
     - Encoder: 다층 PAM으로 인코딩
     - Decoder: 예측 시점에서의 예측값 생성
  
- **복잡도 분석**:
  - 시간 복잡도: O(L log L) (트리 구조와 주의 계산)
  - 공간 복잡도: O(L log L)
  - 메모리 효율: 표준 Transformer 대비 대폭 감소

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 ETT 분할 (60%, 20%, 20%)
- Batch size / Optimizer / LR: 32, Adam, 기본 학습률
- Loss function: MSE
- 기타: 해상도 축소 비율(reduction ratio) 4 사용

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 단일·다중 스텝 예측 모두 SOTA 대비 우수 |
| 192     | see paper | see paper | see Table 3 |
| 336     | see paper | see paper | see Table 3 |
| 720     | see paper | see paper | see Table 3 |

출처: Table 3 of paper (ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather, Traffic 등).

## 4. 주요 기여
- 피라미드 그래프로 다중해상도 시계열 표현을 효율적으로 학습.
- 신호 경로 최대 거리를 상수로 유지하면서 O(L) 시간·공간 복잡도 달성.
- 단일·다중 스텝 긴 범위 예측 모두에서 SOTA 방법 대비 우수한 정확도.
- 실제 장시간 데이터에서 시간과 메모리 효율 우수함을 실증.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 고정 피라미드 구조로 인해 최적의 해상도 선택이 데이터에 따라 달라질 수 있음.
- ※ 메모: Transformer 기반 시계열 예측의 효율성 문제를 명확히 해결한 핵심 논문. PatchTST와 달리 고정 아키텍처 기반. 이후 Crossformer(ICLR 2023) 등에서 다차원 의존성까지 확장된 기반이 됨.

## 6. 참고
- arXiv: https://arxiv.org/abs/2202.07125
- GitHub: https://github.com/ant-research/Pyraformer
- ICLR 2022 Oral 발표
- 관련: Informer (ICLR 2021), Autoformer (ICLR 2021), FEDformer (ICML 2022), Crossformer (ICLR 2023)
