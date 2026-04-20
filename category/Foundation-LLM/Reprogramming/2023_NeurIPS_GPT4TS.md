---
title: "One Fits All: Power General Time Series Analysis by Pretrained LM"
authors: Tian Zhou et al.
venue: NeurIPS
year: 2023
paper_url: https://arxiv.org/abs/2302.11939
code_url: https://github.com/DAMO-DI-ML/NeurIPS2023-One-Fits-All
tags: [foundation-model, pretrained-lm, transformer, time-domain, reprogramming, forecasting, classification, anomaly-detection]
primary_category: category/Foundation-LLM/Reprogramming
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_GPT4TS.pdf
pdf_source: arxiv
---

## TL;DR
GPT4TS (One Fits All; OFA)는 GPT-2를 백본으로 활용하여 하나의 사전학습된 언어 모델로 시계열 예측·분류·이상탐지·대체 등 다양한 분석 태스크를 통합 처리하는 프레임워크다. 어텐션/FFN 레이어를 동결하고 위치 임베딩과 LayerNorm만 파인튜닝함으로써 소수의 파라미터로도 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- NLP/CV 분야와 달리, 시계열 분석 각 태스크(예측·분류 등)는 특화된 모델에 의존해 범용 표현 학습이 어렵다.
- 대규모 언어 모델은 충분한 사전학습 토큰으로 강력한 시퀀스 모델링 능력을 갖추고 있으나, 시계열 도메인에 직접 적용하는 시도가 부족했다.
- 가설: GPT-2의 시퀀스 처리 능력은 시계열 패턴 학습에도 전이 가능하며, 최소한의 파인튜닝으로 다중 태스크를 소화할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 사전학습된 GPT-2의 어텐션·FFN 레이어를 동결(freeze)하고, 입력 패치 임베딩·위치 임베딩·LayerNorm만 학습.
- **아키텍처**:
  1. Instance Normalization으로 입력 정규화.
  2. 시계열을 고정 크기 패치로 분할 후 선형 임베딩.
  3. 동결된 GPT-2 트랜스포머 블록 통과.
  4. 태스크별 출력 헤드(예측/분류/이상탐지).
- **학습 전략**: 각 태스크에서 LayerNorm과 임베딩 레이어만 경량 파인튜닝.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (long-term forecasting)
- Forecast horizons: 96, 192, 336, 720
- Normalization: Instance Normalization
- Train/Val/Test split: 표준 ETT 분할 (12:4:4 months)
- Batch size / Optimizer / LR: Adam, LR 1e-4
- Loss function: MSE
- 기타: GPT-2 small (117M) frozen attention+FFN; 위치 임베딩·LayerNorm만 학습

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.376 | 0.398 | SOTA급 |
| 192     | 0.420 | 0.420 | SOTA급 |
| 336     | 0.453 | 0.440 | SOTA급 |
| 720     | 0.473 | 0.468 | SOTA급 |

출처: Table 1 of paper (NeurIPS 2023 proceedings).

## 4. 주요 기여
- 동결된 사전학습 LM(GPT-2)으로 시계열 다중 태스크(예측·분류·이상탐지·임퓨테이션·few-shot)를 단일 프레임워크로 처리.
- 최소 파인튜닝(LayerNorm + 위치 임베딩)만으로도 전문 모델을 능가하는 성능 달성.
- 언어 모델 규모(GPT-2 small/medium/large)와 성능의 관계를 실증적으로 분석.
- NeurIPS 2023 Spotlight 선정.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 언어 모델의 사전학습 데이터와 시계열 간 도메인 갭이 여전히 존재.
- ※ 메모: 동결 전략이 왜 효과적인지에 대한 이론적 설명 부족. 더 큰 LLM(GPT-3, LLaMA) 적용 시 성능 스케일 여부가 후속 연구 주제.

## 6. 참고
- arXiv: https://arxiv.org/abs/2302.11939
- GitHub: https://github.com/DAMO-DI-ML/NeurIPS2023-One-Fits-All
- 관련: TEMPO (ICLR 2024), Time-LLM (ICLR 2024)
