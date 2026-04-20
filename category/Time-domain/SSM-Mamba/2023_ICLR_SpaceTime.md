---
title: "Effectively Modeling Time Series with Simple Discrete State Spaces"
authors: Michael Zhang et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2303.09489
code_url: https://github.com/HazyResearch/spacetime
tags: [time-domain, ssm, state-space, companion-matrix, autoregressive]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_SpaceTime.pdf
pdf_source: arxiv
---

## TL;DR
SpaceTime은 companion matrix 기반 SSM 파라미터화를 통해 이산 자기회귀(AR) 과정을 정확히 모델링할 수 있는 새로운 상태 공간 시계열 아키텍처다. 기존 S4가 표현하지 못하는 AR 과정을 지원하며, 장기 예측을 위한 "closed-loop" 변형으로 자체 생성한 입력으로 먼 미래를 예측한다. ETTh1 등 Informer 벤치마크에서 14/16 태스크 SOTA를 달성했다.

## 1. 기존 모델의 한계 / 가설
- S4 등 기존 SSM은 AR(자기회귀) 과정을 표현할 수 없어 시계열 모델링 능력이 제한됨.
- Transformer는 O(L²) 복잡도로 긴 시퀀스 학습이 느리고, LSTM은 병렬화가 어려움.
- 가설: companion matrix로 SSM을 파라미터화하면 임의의 ARMA 과정을 정확히 표현할 수 있으며, closed-loop 구조로 장기 예측을 효율화할 수 있음.

## 2. 제안 방법론
- **핵심 아이디어**: Companion matrix를 SSM state transition matrix A로 사용하여 이산 AR 과정 표현 가능성 확보.
- **Companion SSM**: A = companion matrix (다항식의 companion form). SSM 레이어가 데이터에서 AR 계수를 학습.
- **Closed-loop 예측**: 장기 예측 시 모델 자신의 출력을 다음 레이어 입력으로 재투입하여 autoregressive multi-step 예측.
- **SpaceTime 아키텍처**: Companion SSM 레이어 스택 + fully-connected projection. 표준 딥러닝 구성 요소와 호환.
- **학습 효율**: ETTh1 기준 Transformer 대비 73%, LSTM 대비 80% 학습 시간 절감.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (또는 데이터셋별 표준 설정)
- Forecast horizons: {96, 192, 336, 720} (Informer 벤치마크 설정)
- Normalization: Z-score
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: Adam optimizer (세부 세팅은 논문 Appendix 참조)
- Loss function: MSE
- 기타: SpaceTime 레이어 수, hidden dimension 등은 Appendix Table 참조

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | see paper | see paper | 16개 Informer 태스크 중 14개 SOTA |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 2-3 of paper (arXiv:2303.09489). ※ 메모: 논문은 Informer-style long-short 예측 benchmarks에서 SOTA 주장; 구체적 ETTh1 수치는 원문 확인 필요.

## 4. 주요 기여
- Companion matrix SSM으로 임의 ARMA 과정 표현 가능성 이론적 증명.
- 기존 S4가 해결 못한 AR 과정 모델링 문제 해결.
- Closed-loop 예측 구조로 장기 예측의 정확도와 효율성 동시 개선.
- Informer 벤치마크 14/16 태스크에서 SOTA, Transformer 대비 73~80% 학습 가속.

## 5. 한계 및 후속 연구 아이디어
- PatchTST 등 이후 모델 대비 ETTh1 절대 성능이 낮을 수 있음 (비교 baseline이 다름).
- Companion matrix 파라미터화의 수치 안정성 문제 가능성.
- ※ 메모: Mamba(선택적 상태 공간, 2023 말)와 비교하면 데이터 의존적 선택 메커니즘이 없다는 점이 차이; SpaceTime은 이론적 AR 표현에 강점.

## 6. 참고
- arXiv: https://arxiv.org/abs/2303.09489
- OpenReview: https://openreview.net/forum?id=2EpjkjzdCAa
- 공식 구현: https://github.com/HazyResearch/spacetime
