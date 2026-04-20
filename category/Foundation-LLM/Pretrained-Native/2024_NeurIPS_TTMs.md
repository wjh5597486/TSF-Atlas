---
title: "Tiny Time Mixers (TTMs): Fast Pre-trained Models for Enhanced Zero/Few-Shot Forecasting of Multivariate Time Series"
authors: Vijay Ekambaram et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2401.03955
code_url: https://github.com/ibm-granite/granite-tsfm
tags: [foundation-model, pretrained, zero-shot, few-shot, mlp-mixer, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_TTMs.pdf
pdf_source: arxiv
---

## TL;DR
TTMs(Tiny Time Mixers)는 경량 TSMixer 아키텍처 기반의 사전학습 시계열 파운데이션 모델. CPU에서도 실행 가능하며 제로/퓨샷 예측에서 기존 대형 모델보다 우수하거나 동등한 성능. IBM 연구소 개발.

## 1. 기존 모델의 한계 / 가설
- 기존 사전학습 모델(Chronos, TimesFM 등)은 거대 모델로 추론 비용이 높고 CPU 실행 불가.
- 제로샷 성능이 데이터셋에 따라 불안정.
- 가설: 경량 모델도 충분히 다양한 데이터로 사전학습하면 제로/퓨샷에서 대형 모델 수준 달성 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 경량 TSMixer 아키텍처로 사전학습 → 제로/퓨샷 예측.
- **아키텍처**:
  - TSMixer 기반 (MLP-Mixer 변형): 시간 축 + 채널 축 MLP 블록 교대 적용
  - 입력: 멀티도메인 시계열 데이터로 사전학습 (ETTh2 포함 다양한 소스)
  - 경량 설계: 수백만 파라미터 (vs GPT 수십억)
  - 채널 믹싱 + 시간 믹싱 분리
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 512
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- ETTh1은 타겟 데이터, ETTh2는 소스(사전학습) 데이터
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 제로샷에서 대형 모델 대비 경쟁력 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 경량 사전학습 TSF 모델 (CPU 실행 가능)
- 제로/퓨샷 예측에서 대형 모델 대비 경쟁력 있는 성능
- IBM Granite 시리즈 기반 오픈소스 TSF 파운데이션 모델
- 도메인 적응(fine-tuning) 효율성 검증

## 5. 한계 및 후속 연구 아이디어
- 제한된 사전학습 데이터셋으로 일반화 한계 존재
- 복잡한 다변량 관계 포착에 MLP 믹서의 한계

## 6. 참고
- [GitHub (IBM Granite)](https://github.com/ibm-granite/granite-tsfm)
- [arXiv 2401.03955](https://arxiv.org/abs/2401.03955)
