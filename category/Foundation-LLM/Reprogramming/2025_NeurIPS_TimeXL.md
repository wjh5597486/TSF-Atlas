---
title: "TimeXL: Explainable Multi-modal Time Series Prediction with LLM-in-the-Loop"
authors: Yushan Jiang et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2503.01013
code_url: ""
tags: [foundation-llm, reprogramming, multi-modal, explainability, prototype, llm]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_TimeXL.pdf
pdf_source: arxiv
---

## TL;DR
TimeXL은 프로토타입 기반 시계열 인코더와 세 LLM을 협력 루프로 연결하여 정확한 예측과 해석 가능한 설명을 동시에 제공하는 멀티모달 프레임워크다. LLM 피드백 루프를 통한 반복 수정으로 AUC를 최대 8.9% 향상시킨다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 높은 예측 정확도를 제공하지만 인간이 이해 가능한 설명을 제공하지 못함.
- 단일 모달리티(수치 시계열만)로는 복잡한 시간적 패턴의 해석이 어려움.
- 가설: 시계열과 텍스트 입력을 멀티모달로 처리하고, LLM 추론을 활용하면 정확도와 설명 가능성을 동시에 달성할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 프로토타입 기반 인코더 + 3개의 협력 LLM으로 구성된 LLM-in-the-Loop 프레임워크.
- **아키텍처**:
  - **멀티모달 프로토타입 인코더**: 시계열과 텍스트 입력을 동시에 처리하여 예비 예측과 사례 기반 근거(rationale) 생성.
  - **예측 LLM**: 인코더 출력(예측+설명)을 기반으로 텍스트 불일치를 추론하여 예측 수정.
  - **피드백 루프**: 예측-근거-수정의 반복 루프로 지속적 개선.
- **손실함수**: 태스크별 손실 + LLM 정렬 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: 태스크별 손실
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 등 4개 실제 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | AUC 최대 8.9% 향상 |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 주로 AUC 지표 기준; MSE/MAE 구체 수치는 논문 참조 필요.

## 4. 주요 기여
- LLM-in-the-Loop 멀티모달 예측 프레임워크 TimeXL 제안.
- 프로토타입 기반 인코더로 시계열+텍스트 멀티모달 처리.
- 3개의 협력 LLM을 통한 예측-근거-수정 피드백 루프.
- AUC 최대 8.9% 향상 + 인간 중심적 설명 생성.

## 5. 한계 및 후속 연구 아이디어
- 3개의 LLM 협력으로 인한 높은 계산 비용.
- LLM 추론 품질에 의존하는 설명 신뢰성.

## 6. 참고
- arxiv: https://arxiv.org/abs/2503.01013
- OpenReview: https://openreview.net/forum?id=WRwr2YZ4zt
