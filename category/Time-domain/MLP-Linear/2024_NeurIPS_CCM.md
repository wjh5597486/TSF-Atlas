---
title: "From Similarity to Superiority: Channel Clustering for Time Series Forecasting"
authors: Jiawei Chen et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2404.01340
code_url: ""
tags: [channel-clustering, channel-dependent, channel-independent, multivariate, plug-and-play]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_CCM.pdf
pdf_source: arxiv
---

## TL;DR
CCM(Channel Clustering Module)은 채널 독립(CI)과 채널 의존(CD) 접근의 장점을 결합. 유사한 채널을 동적으로 그룹화하여 클러스터 정보를 활용함으로써 기존 모델에 plug-and-play로 적용 가능.

## 1. 기존 모델의 한계 / 가설
- CI(채널 독립) 전략: 일반화 향상되지만 채널 간 의존성 무시.
- CD(채널 의존) 전략: 불필요한 채널 혼합으로 과평탄화(oversmoothing) 발생.
- 가설: 유사한 채널끼리 클러스터링하면 CI+CD의 장점을 동시에 활용 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Channel Clustering Module(CCM) — 동적 채널 클러스터링 후 클러스터 아이덴티티 기반 예측.
- **아키텍처**:
  - 입력 채널을 유사도 기반으로 동적 그룹화
  - 채널별 아이덴티티 대신 클러스터 소속 정보 활용
  - 기존 CI/CD 모델에 plug-and-play로 적용
  - 제로샷 일반화 지원 (새 채널/도메인)
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | CI/CD 모델 대비 평균 2.4% MSE 향상 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table 1 of paper.

## 4. 주요 기여
- CI와 CD 전략의 장점을 결합하는 채널 클러스터링 프레임워크
- 기존 모델에 plug-and-play로 적용 가능 (평균 2.4% / 7.2% MSE 향상)
- 제로샷 예측 지원 및 해석 가능한 채널 패턴 발견
- ETTh1 등 실험에서 채널 0, 2, 4가 유사 클러스터를 형성함 발견

## 5. 한계 및 후속 연구 아이디어
- 클러스터 수 K 설정이 성능에 영향
- 시간 변화하는 채널 유사도에 대한 처리 미비

## 6. 참고
- [arXiv 2404.01340](https://arxiv.org/abs/2404.01340)
