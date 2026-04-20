---
title: "Unified Training of Universal Time Series Forecasting Transformers"
authors: Gerald Woo et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.02592
code_url: https://github.com/SalesforceAIResearch/uni2ts
tags: [foundation-model, universal, pretrained, transformer, any-variate, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_MOIRAI.pdf
pdf_source: arxiv
---

## TL;DR
MOIRAI(Masked Encoder-based Universal TSF Transformer)는 Salesforce의 범용 시계열 파운데이션 모델. LOTSA(27B 관측) 대규모 데이터셋으로 사전학습하고 동적 패치 크기, Any-Variate Attention으로 제로샷 예측 달성. ICML 2024 Oral.

## 1. 기존 모델의 한계 / 가설
- 기존 파운데이션 모델은 단일 도메인이나 고정 변수 수에 의존하여 범용성 부족.
- 시계열의 다양한 주파수와 변수 수 처리를 위한 통합 아키텍처 부재.
- 가설: 다양한 도메인/주파수/변수 수의 대규모 데이터로 학습하면 범용 예측기 구현 가능.

## 2. 제안 방법론
- **핵심 아이디어**: LOTSA 대규모 오픈 시계열 아카이브 + 동적 패치 크기 + Any-Variate Attention.
- **아키텍처**:
  - Masked Encoder: 입력 패치 마스킹 후 복원 사전학습
  - Dynamic Patch Size: 다양한 샘플링 주파수에 적응
  - Any-Variate Attention: 임의 변수 수 처리 (변수 수 독립적)
  - LOTSA: 27B+ 관측치, 9개 도메인 사전학습 데이터
- **손실함수**: Masked reconstruction loss (사전학습), MSE (파인튜닝)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 다양한 설정
- Forecast horizons: 96, 192, 336, 720
- 제로샷 평가 (ETTh1은 학습 데이터 미포함)
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 제로샷 SOTA 수준 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- LOTSA: 9개 도메인, 27B+ 관측치 대규모 오픈 TSF 아카이브 공개
- Dynamic Patch + Any-Variate Attention으로 범용 예측
- 제로샷에서 full-shot 모델 대비 경쟁력 있는 성능
- ICML 2024 Oral 수상

## 5. 한계 및 후속 연구 아이디어
- 사전학습 데이터 선택 편향 가능성
- 특정 도메인 특화 파인튜닝 vs 범용성 트레이드오프

## 6. 참고
- [GitHub](https://github.com/SalesforceAIResearch/uni2ts)
- [arXiv 2402.02592](https://arxiv.org/abs/2402.02592)
