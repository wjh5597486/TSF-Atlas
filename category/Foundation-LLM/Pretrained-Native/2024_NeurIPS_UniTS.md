---
title: "UniTS: A Unified Multi-Task Time Series Model"
authors: Shanghua Gao et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2403.00131
code_url: https://github.com/mims-harvard/UniTS
tags: [foundation-model, multi-task, unified, transformer, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_UniTS.pdf
pdf_source: arxiv
---

## TL;DR
UniTS는 예측(predictive)과 생성(generative) 태스크를 단일 프레임워크로 통합하는 다중태스크 시계열 파운데이션 모델. Task tokenization으로 다양한 태스크를 표준화하고, 수정된 Transformer 블록으로 범용 시계열 표현 학습.

## 1. 기존 모델의 한계 / 가설
- 기존 모델은 특정 태스크(예측, 분류, 이상 탐지)에 특화되어 다중 태스크 처리 불가.
- LLM 기반 접근은 시계열 특성(주기성, 추세)을 텍스트 언어 모델에 억지로 맞추는 문제.
- 가설: 태스크 토큰화로 다양한 시계열 태스크를 통합 표현 공간에서 처리 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Task Tokenization + 수정된 Transformer 블록으로 예측/생성 태스크 통합.
- **아키텍처**:
  - Task Token: 태스크 유형을 특수 토큰으로 인코딩
  - 수정된 Transformer: 시계열 특화 어텐션 + 태스크 조건부 처리
  - Predictive 태스크(예측, 분류, 이상탐지)와 Generative 태스크(보간, 변환) 동시 지원
  - Few-shot 및 zero-shot 일반화
- **손실함수**: 태스크별 손실(MSE for forecasting)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 단일 모델 다중 태스크에서 경쟁력 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 예측/생성 태스크를 하나의 모델로 처리하는 최초의 통합 프레임워크
- Task Tokenization으로 다양한 시계열 태스크 표준화
- Few-shot/zero-shot 일반화 지원
- 38개 데이터셋, 10개 태스크에서 경쟁력 있는 성능

## 5. 한계 및 후속 연구 아이디어
- 태스크 수 증가 시 충돌하는 그래디언트 관리 어려움
- 도메인 특화 파인튜닝의 효과 제한 가능

## 6. 참고
- [GitHub](https://github.com/mims-harvard/UniTS)
- [arXiv 2403.00131](https://arxiv.org/abs/2403.00131)
