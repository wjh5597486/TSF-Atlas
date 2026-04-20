---
title: "TimeSiam: A Pre-Training Framework for Siamese Time-Series Modeling"
authors: Jiaxiang Dong et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.02475
code_url: https://github.com/thuml/TimeSiam
tags: [self-supervised, siamese, pretrained, time-domain, contrastive]
primary_category: category/SelfSupervised
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICML_TimeSiam.pdf
pdf_source: arxiv
---

## TL;DR
TimeSiam은 과거-현재 서브시계열 간의 내재적 시간 상관관계를 포착하기 위한 샴 네트워크 기반 사전학습 프레임워크. 시간적 거리를 구별하는 lineage embedding으로 13개 벤치마크에서 일관된 SOTA.

## 1. 기존 모델의 한계 / 가설
- 랜덤 마스킹 기반 사전학습(MAE)은 시계열 고유의 시간 상관관계를 왜곡.
- 시리즈 수준 유사도 계산은 내재적 시간 의존성을 무시.
- 가설: 과거-현재 서브시계열 쌍으로 샴 인코더를 학습하면 시간 의존성 표현 학습 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 시간 이동된 과거/현재 서브시계열 쌍으로 샴 인코더 학습 + lineage embedding으로 시간 거리 구별.
- **아키텍처**:
  - 무작위 마스킹으로 다양한 서브시계열 생성 (데이터 증강)
  - 샴 인코더 쌍: 과거와 현재 서브시계열 각각 인코딩
  - Past-to-current 재구성으로 시간 의존성 학습
  - Lineage Embedding: 샘플된 서브시계열 간 시간 거리 구별
- **손실함수**: 마스킹 재구성 + 시간 의존성 MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Fine-tuning 후 평가
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 사전학습 기반 모델 중 SOTA |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 시간 의존성 보존하는 샴 사전학습 프레임워크
- Lineage Embedding으로 시간 거리 명시적 표현
- 13개 벤치마크에서 도메인 내/도메인 간 전이 SOTA
- TSLD-1G → ETTh1/ETTm1 도메인 간 전이 성공

## 5. 한계 및 후속 연구 아이디어
- 샴 인코더 2개로 메모리 비용 증가
- 사전학습 데이터 도메인 선택이 전이 성능에 민감

## 6. 참고
- [GitHub](https://github.com/thuml/TimeSiam)
- [arXiv 2402.02475](https://arxiv.org/abs/2402.02475)
