---
title: "Time Evidence Fusion Network: Multi-Source View in Long-Term Time Series Forecasting"
authors: Tianxiang Zhan et al.
venue: IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI) 2025
year: 2025
paper_url: https://arxiv.org/abs/2405.06419
code_url: https://github.com/ztxtech/Time-Evidence-Fusion-Network
tags: [time-domain, transformer, channel-dependent, evidence-theory, fusion]
primary_category: journal/TPAMI
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: arxiv
---

## TL;DR
TEFN은 Dempster-Shafer 증거 이론에 기반한 Basic Probability Assignment(BPA) 모듈을 도입해 다변량 시계열의 채널 축과 시간 축 양방향에서 불확실성을 정량화하고 정보를 융합한다. 다중 소스 뷰(multi-source view)를 통해 기존 Transformer 계열 대비 안정적인 ETTh1 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 방법들은 채널 간 의존성과 시간적 의존성을 분리 처리하거나 결합하는 방식으로 학습하지만, 예측 불확실성을 명시적으로 다루지 않음.
- 가설: 증거 이론(evidence theory)을 적용해 각 채널·시점의 예측 기여도를 확률 질량(BPA)으로 표현하면 불확실성을 줄이고 예측 안정성을 높일 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: BPA 모듈로 채널 차원과 시간 차원 각각의 예측을 "증거"로 간주, Dempster 결합 규칙으로 융합.
- **아키텍처 구성요소**:
  1. **Temporal BPA Module**: 시간 패치 단위로 BPA 함수를 학습 → 각 시점의 예측 신뢰도 점수 산출
  2. **Channel BPA Module**: 채널 간 상관관계를 BPA로 인코딩 → 채널 가중치 동적 조정
  3. **Evidence Fusion**: Dempster-Shafer 결합으로 두 소스의 증거를 합산, 최종 예측 생성
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 / 336 (논문에서 복수 설정 실험)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 ETT 분할 (12/4/4 months)
- Batch size / Optimizer / LR: Adam, lr=1e-4 (기본값; 세부 내용은 논문 Appendix 참조)
- Loss function: MSE

### 3.2 Results
> arXiv 논문(2405.06419) 기준; 정확한 수치는 논문 Table 참조 요망.

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | ~0.370 | ~0.397 | iTransformer 대비 소폭 개선 |
| 192     | ~0.413 | ~0.421 | |
| 336     | ~0.422 | ~0.436 | |
| 720     | ~0.447 | ~0.468 | |

※ 메모: 수치는 arXiv 초고 기준 근사값. TPAMI 최종판에서 달라질 수 있음.

## 4. 주요 기여
- 증거 이론(Dempster-Shafer)을 TSF에 최초 적용, 예측 불확실성을 명시적으로 모델링
- 채널 축·시간 축 이중 BPA 모듈로 다중 소스 정보 융합
- 기존 Transformer 기반 방법 대비 안정적인 예측 성능 향상
- 경량 구조로 대용량 데이터셋에서도 효율적 동작

## 5. 한계 및 후속 연구 아이디어
- Dempster 결합 규칙은 상충되는 증거(highly conflicting evidence)에서 직관에 반하는 결과를 낼 수 있음.
- BPA 학습이 잘 안 될 경우 기존 단순 attention보다 오히려 불안정할 수 있음.
- ※ 메모: Yager 규칙이나 PCR5 등 대안 결합 규칙과의 비교 실험이 후속 연구에서 흥미로울 것.

## 6. 참고
- arXiv: https://arxiv.org/abs/2405.06419
- GitHub: https://github.com/ztxtech/Time-Evidence-Fusion-Network
- 관련: iTransformer(ICLR 2024), PatchTST(ICLR 2023)
