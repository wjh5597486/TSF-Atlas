---
title: "OLinear: A Linear Model for Time Series Forecasting in Orthogonally Transformed Domain"
authors: Wenzhen Yue et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.08550
code_url: https://github.com/jackyue1994/OLinear
tags: [time-domain, mlp-linear, orthogonal-transform, channel-dependent, linear]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_OLinear.pdf
pdf_source: arxiv
---

## TL;DR
OLinear은 직교 변환 도메인에서 동작하는 선형 기반 예측 모델로, 데이터 적응형 직교 행렬(OrthoTrans)로 시간적 의존성을 분리하고, 정규화 가중치 선형 레이어(NormLin)로 다변량 의존성을 캡처한다. 24개 벤치마크·140개 예측 태스크에서 SOTA를 달성하면서 Multi-Head Self-Attention의 절반 FLOPs만 사용한다.

## 1. 기존 모델의 한계 / 가설
- 기존 시간 도메인 선형 모델은 다변량 의존성을 충분히 포착하지 못함.
- Transformer의 Multi-Head Self-Attention은 강력하지만 계산 비용이 높음.
- 가설: 직교 변환으로 시간적 의존성을 먼저 분리하면, 선형 레이어로도 충분한 다변량 표현력을 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 시간 도메인 대신 직교 변환 도메인에서 선형 예측 수행.
- **아키텍처**:
  - **OrthoTrans**: 데이터 적응형 직교 행렬을 이용한 변환. 시간적 의존성을 분리(decorrelate)하여 이후 처리가 수월해짐.
  - **NormLin**: 정규화된 가중치를 갖는 특화 선형 레이어. 다변량 의존성 캡처. Multi-Head Self-Attention 대비 약 절반의 FLOPs.
  - OLinear = OrthoTrans + NormLin의 조합.
- **NormLin 활용**: 기존 Transformer 기반 예측기에 Plug-In 모듈로 삽입 가능.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96, 336, 720 등 다수
- Forecast horizons: 96, 192, 336, 720
- Normalization: 인스턴스 정규화(RevIN 등)
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 24개 벤치마크, 140개 예측 태스크에서 평가(ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 포함)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper Table |
| 192     |     |     | see paper Table |
| 336     |     |     | see paper Table |
| 720     |     |     | see paper Table |

출처: Table of paper. ※ 메모: 구체적 수치는 논문 PDF 참조 필요.

## 4. 주요 기여
- 직교 변환 도메인에서 동작하는 선형 예측 모델 OLinear 제안.
- OrthoTrans를 통한 데이터 적응형 직교 변환.
- NormLin이 MHSA 대비 ~50% FLOPs로 유사하거나 더 높은 성능 달성.
- 24개 벤치마크 140개 태스크에서 일관된 SOTA 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 직교 행렬 학습의 안정성(수렴)에 주의 필요.
- 확장 아이디어: 다양한 직교 변환 패밀리 탐색, 주파수 도메인 결합.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.08550
- GitHub: https://github.com/jackyue1994/OLinear
- OpenReview: https://openreview.net/forum?id=DAyKP1tvwI
