---
title: "Enhancing Time Series Forecasting through Selective Representation Spaces: A Patch Perspective"
authors: Xingjian Wu et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2510.14510
code_url: https://github.com/decisionintelligence/SRSNet
tags: [time-domain, transformer, patching, mlp, channel-independent]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Patching
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_SRSNet.pdf
pdf_source: arxiv
---

## TL;DR
SRSNet은 Selective Representation Space(SRS) 모듈을 통해 시계열 패치를 적응적으로 선택·재배열하여 표현 공간의 다양성을 높이는 Patch 기반 예측 모델이다. MLP 헤드와 결합하여 NeurIPS 2025 Spotlight 논문으로 채택되었으며, 기존 고정 패치 분할 방식의 한계를 극복한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Patch 기반 모델(PatchTST 등)은 인접한 타임스텝을 고정 크기로 분할하여 표현 공간이 고정됨.
- 고정 패치 분할은 시계열의 다양한 패턴을 충분히 표현하지 못하고 정보 손실을 야기함.
- 가설: 패치를 학습 가능하게 선택·재조합하면 더 표현력 있는 컨텍스트 표현이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 학습 가능한 Selective Patching과 Dynamic Reassembly를 통해 컨텍스트 시계열에서 패치를 적응적으로 선택·셔플.
- **아키텍처**:
  - **SRS(Selective Representation Space) Module**: Selective Patching으로 다양한 위치의 패치 선택, Dynamic Reassembly로 재배열.
  - **SRSNet**: SRS + MLP Head로 구성된 경량 모델.
  - Plug-and-Play 모듈로서 기존 Patch 기반 모델(PatchTST, TimesNet 등)에 삽입 가능.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96 or 336 (실험별 상이)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN(인스턴스 정규화)
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 8개 실제 데이터셋 평가 (ETTh1, ETTh2, ETTm1, ETTm2, Electricity, Weather, Traffic 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper Table 1 |
| 192     |     |     | see paper Table 1 |
| 336     |     |     | see paper Table 1 |
| 720     |     |     | see paper Table 1 |

출처: Table 1 of paper. ※ 메모: 구체적 수치는 논문 PDF 참조 필요.

## 4. 주요 기여
- 적응형 패치 선택 및 재배열을 통한 Selective Representation Space(SRS) 모듈 제안.
- 기존 Patch 기반 모델에 Plug-and-Play 방식으로 성능 향상 가능.
- SRSNet(SRS + MLP)이 8개 데이터셋에서 SOTA 달성.
- NeurIPS 2025 Spotlight 논문으로 선정.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 동적 패치 선택의 계산 오버헤드(소규모이지만 고려 필요).
- 확장 아이디어: 멀티스케일 SRS 적용, 비정형(irregular) 시계열로 확장 검토.

## 6. 참고
- arxiv: https://arxiv.org/abs/2510.14510
- GitHub: https://github.com/decisionintelligence/SRSNet
- OpenReview: https://openreview.net/forum?id=BirE0jYKt0
