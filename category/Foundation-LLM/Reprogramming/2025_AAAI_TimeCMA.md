---
title: "TimeCMA: Towards LLM-Empowered Multivariate Time Series Forecasting via Cross-Modality Alignment"
authors: Chenxi Liu et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2406.01638
code_url: https://github.com/ChenxiLiu-HNU/TimeCMA
tags: [foundation-llm, reprogramming, cross-modality, alignment, multivariate, long-term]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_TimeCMA.pdf
pdf_source: arxiv
---

## TL;DR
TimeCMA는 LLM 강화 텍스트 인코딩과 시계열 인코딩의 이중 브랜치 구조에 cross-modality alignment를 적용하여 "두 세계의 최선"을 결합하는 다변량 시계열 예측 모델이다. AAAI 2025 Oral 논문으로 선정되었다.

## 1. 기존 모델의 한계 / 가설
- 시계열 인코딩만 사용하면 분리된(disentangled) 임베딩을 얻을 수 있으나 노이즈에 취약
- LLM 기반 텍스트 인코딩은 강건하나 임베딩이 얽혀(entangled) 해석 어려움
- 가설: 두 인코딩 방식을 cross-modality alignment로 결합하면 disentangled + robust한 표현 가능

## 2. 제안 방법론
- **핵심 아이디어**: 이중 모달리티 인코딩 + cross-modality alignment로 최적 표현 획득
- **아키텍처 구성요소**:
  1. **Time Series Encoding Branch**: 시계열 직접 인코딩 (약하지만 분리된 임베딩)
  2. **LLM-empowered Encoding Branch**: 텍스트 프롬프트 기반 LLM 인코딩 (강건하지만 얽힌 임베딩)
  3. **Cross-Modality Alignment**: 두 임베딩 공간을 정렬하여 보완적 정보 결합
  4. **Last-token Optimization**: 긴 텍스트 프롬프트에서 마지막 토큰만 최적화하여 효율성 향상
- **효율화**: 긴 프롬프트의 마지막 토큰 집중으로 계산 절감

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정 (논문 참조)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 다변량 시계열 다중 데이터셋 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 이중 모달리티(시계열+텍스트) 인코딩 프레임워크 제안
- Cross-modality alignment로 분리성(disentanglement)과 강건성(robustness) 동시 확보
- Last-token optimization으로 LLM 인코딩 효율화
- AAAI 2025 Oral 선정

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 텍스트 프롬프트 품질에 성능 의존
- ※ 메모: CALF와 달리 cross-modal fine-tuning보다 alignment 관점이 강조됨

## 6. 참고
- [arXiv 2406.01638](https://arxiv.org/abs/2406.01638)
- [GitHub](https://github.com/ChenxiLiu-HNU/TimeCMA)
- 비교: CALF, Time-LLM, GPT4TS
