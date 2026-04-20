---
title: "DUET: Dual Clustering Enhanced Multivariate Time Series Forecasting"
authors: Xiangfei Qiu et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2412.10859
code_url: https://github.com/decisionintelligence/DUET
tags: [time-domain, mlp, clustering, channel-dependent, normalization]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_DUET.pdf
pdf_source: arxiv
---

## TL;DR
DUET introduces dual clustering on temporal and channel dimensions to enhance multivariate time series forecasting. A Temporal Clustering Module (TCM) groups sub-series into fine-grained distributions for heterogeneous pattern modeling, while a Channel Clustering Module (CCM) captures inter-channel relationships via frequency-domain metric learning. It was ranked 1st in the most influential KDD 2025 papers and evaluated on 25 datasets from 10 domains.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer/MLP 모델은 채널 간 이질적인 시간 패턴을 단일 방식으로 처리하여 heterogeneous temporal patterns를 모델링하지 못함.
- Channel-independent 방식은 채널 간 관계를 무시하고, channel-dependent 방식은 노이즈 채널까지 포함하여 성능이 저하됨.
- 저자들은 temporal 클러스터링과 channel soft-clustering을 결합하면 두 문제를 동시에 해결할 수 있다고 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 시간 차원과 채널 차원을 동시에 클러스터링하여 이질적 패턴을 처리하고, 유사 채널 간 정보만 선택적으로 집약.
- **아키텍처 구성요소**:
  1. **Temporal Clustering Module (TCM)**: Distribution Router가 각 sub-series를 K개의 Pattern Extractor(Linear 기반)로 라우팅 → fine-grained 분포 클러스터링.
  2. **Channel Clustering Module (CCM)**: 주파수 도메인에서 채널 간 metric learning + sparsification → 채널 soft-clustering mask 생성.
  3. **Fusion Module (FM)**: Masked Attention으로 temporal feature와 channel mask를 결합.
- **정규화**: Instance Normalization (RevIN 계열).
- **손실함수**: MSE Loss.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96, 336, 512 중 최적값 선택
- Forecast horizons: 96, 192, 336, 720
- Normalization: Instance Normalization
- Train/Val/Test split: 논문 기본 설정 (6:2:2 추정)
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: MSE
- 기타: 25개 데이터셋 통합 실험; ETTh1은 7-variate 시계열

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.352 | 0.385 | iTransformer 대비 개선 |
| 192     | 0.398 | 0.409 | |
| 336     | 0.414 | 0.426 | |
| 720     | 0.430 | 0.457 | |

출처: Table 3 of paper (arxiv:2412.10859).

## 4. 주요 기여
- Temporal Clustering Module (TCM): 서브시리즈를 세밀한 분포로 클러스터링하여 이질적 시간 패턴 처리.
- Channel Clustering Module (CCM): 주파수 도메인 metric learning + sparsification으로 채널 간 관계 포착.
- Masked Attention Fusion: temporal feature와 channel mask를 결합하는 경량 통합 모듈.
- KDD 2025 Most Influential Paper 선정 (25개 데이터셋 SOTA).

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: look-back window 선택이 데이터셋별 하이퍼파라미터 탐색 필요.
- ※ 메모: TCM의 클러스터 수 K가 성능에 미치는 영향 분석 및 자동화 가능성 탐색 여지.

## 6. 참고
- [arxiv:2412.10859](https://arxiv.org/abs/2412.10859)
- [GitHub: decisionintelligence/DUET](https://github.com/decisionintelligence/DUET)
- [ACM DL: KDD 2025](https://dl.acm.org/doi/10.1145/3690624.3709325)
