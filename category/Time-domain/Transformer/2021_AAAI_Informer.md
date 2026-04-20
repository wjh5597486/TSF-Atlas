---
title: Informer: Beyond Efficient Transformer for Long Sequence Time-Series Forecasting
authors: Haoyi Zhou, Shanghang Zhang, Jieqi Peng, Shuai Zhang, Jianxin Li, Hui Xiong, Wancai Zhang
venue: AAAI 2021
year: 2021
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/17325
code_url: https://github.com/zhouhaoyi/Informer2020
tags: [time-domain, transformer, attention, long-term-forecasting, efficient]
primary_category: Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local: ./2021_AAAI_Informer.pdf
pdf_source: aaai
---

## TL;DR
Informer introduces ProbSparse self-attention mechanism with O(L log L) complexity and self-attention distilling to efficiently handle long-sequence time-series forecasting, winning AAAI 2021 Best Paper Award. The generative-style decoder predicts long sequences in a single forward pass rather than step-by-step.

## 1. 기존 모델의 한계 / 가설
- 표준 Transformer self-attention은 시계열 길이 L에 대해 O(L²) 복잡도를 가지므로, LTSF(long-term time-series forecasting)에 부적합
- 기존 encoder-decoder 구조에서 step-by-step 예측으로 인한 오차 누적 및 느린 추론
- 장시간 의존성 모델링에 전체 attention이 불필요 → sparse attention 필요성

## 2. 제안 방법론

### 핵심 아이디어
ProbSparse self-attention mechanism: 모든 query가 주요 key들과의 의존성만 선택적으로 계산 (Query-Key 점수 분포의 KL divergence 기반 선택)

### 아키텍처 구성요소

**ProbSparse Self-Attention**
- 시간복잡도: O(L log L), 메모리 O(L log L)
- Top-u query만 모든 key와 상호작용, 나머지는 sparse pattern
- Canonical Sampling: 점수 분포의 상위 확률을 가진 key만 선택

**Self-Attention Distilling**
- 인접한 두 attention layer의 결과를 비교하여 dominant attention만 하위 layer로 전달
- 계층을 거칠수록 입력 길이 절반으로 단계적 축소 (hierarchical memory saving)

**Generative-style Decoder**
- Encoder의 최종 representation에서 전체 forecast horizon을 한 번에 생성 (step-by-step 대신)
- 추론 속도 대폭 개선

### 주요 수식
- Query i의 sparse selectivity:
  $$s_i = \max_j(q_i K^T) - \frac{1}{L}\sum_{j=1}^L q_i K^T$$
- Top-u queries만 모든 keys와 상호작용, 나머지는 sampling

### 손실함수 / 학습 전략
- 표준 MSE loss for point forecasting
- Adam optimizer, learning rate scheduling

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 24, 48, 168, 336, 720
- Normalization: RevIN (instance-wise normalization)
- Train/Val/Test split: 60% / 20% / 20%
- Batch size: 32
- Optimizer: Adam with learning rate 0.0001
- Loss function: MSE
- 기타: Encoder 4 layers, Decoder 1 layer, 64-dim embedding

### 3.2 Results (Univariate ETTh1)
| Horizon | MSE   | MAE   | 비교 SOTA 대비      |
|---------|-------|-------|---------------------|
| 24      | 0.383 | 0.459 | 기준               |
| 48      | 0.502 | 0.554 | 상위권              |
| 168     | 0.754 | 0.663 | 상위권              |
| 336     | 1.161 | 0.802 | SOTA 달성          |
| 720     | 1.532 | 0.860 | SOTA 달성          |

출처: Table 1, Informer paper (AAAI 2021)

### 3.3 Results (Multivariate ETTh1)
| Horizon | MSE   | MAE   | 비교 SOTA 대비      |
|---------|-------|-------|---------------------|
| 24      | 0.346 | 0.418 | 기준               |
| 48      | 0.418 | 0.501 | 상위권              |
| 168     | 0.657 | 0.613 | 상위권              |
| 336     | 0.958 | 0.731 | SOTA 달성          |
| 720     | 1.381 | 0.806 | SOTA 달성          |

※ 메모: Informer은 ETTh1, ETTh2, ETTm1을 실험하여 SOTA 성능 달성. 추론 시간도 기존 대비 6배 빠름 확인.

## 4. 주요 기여
- ProbSparse self-attention으로 transformer complexity 혁신 (O(L²) → O(L log L))
- Self-attention distilling 방식의 hierarchical attention 도입
- Generative-style decoder로 추론 속도 및 성능 대폭 개선
- AAAI 2021 Best Paper Award 수상 (최초 transformer LTSF 혁신 사례)
- 단순하면서도 효과적인 구조로 많은 후속 연구의 기초 마련

## 5. 한계 및 후속 연구 아이디어
- ProbSparse mechanism이 특정 시계열(주기성 약한 경우)에서 성능 저하 가능
- 인코더 깊이 증가시 distilling 효과의 한계
- 확률적(probabilistic) 예측이 아닌 결정론적 점 예측만 지원
- 다채널 간 상관관계 모델링은 implicit (channel-independent assumption 경향)

※ 메모: Informer 이후 Autoformer, DLinear, PatchTST 등이 새로운 개선 방향 제시. Transformer의 효율성 문제를 근본적으로 재고하는 계기 마련.

## 6. 참고
- GitHub: https://github.com/zhouhaoyi/Informer2020 (PyTorch 구현, 실험 재현 코드 포함)
- 관련 논문: Autoformer (Wu et al., NeurIPS 2021), N-BEATS (Oreshkin et al., ICLR 2020)
- 후속 작업: ETDataset 공개 (GitHub zhouhaoyi/ETDataset) - 광범위한 LTSF 벤치마킹 활성화
