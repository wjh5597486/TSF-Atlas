---
title: "AutoTimes: Autoregressive Time Series Forecasters via Large Language Models"
authors: Yong Liu et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.02370
code_url: https://github.com/thuml/AutoTimes
tags: [llm, autoregressive, reprogramming, time-domain, in-context-forecasting]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_AutoTimes.pdf
pdf_source: arxiv
---

## TL;DR
AutoTimes는 decoder-only LLM을 자기회귀(autoregressive) TSF로 재활용. 시계열을 언어 토큰 임베딩 공간으로 투영하고, LLM 임베딩 텍스트 타임스탬프로 시간 정보를 정렬. 0.1% 파라미터만 학습.

## 1. 기존 모델의 한계 / 가설
- 기존 LLM 기반 TSF 모델은 encoder-only 구조나 다중 레이어 파인튜닝으로 비효율.
- 자기회귀 방식의 임의 길이 예측 지원이 제한됨.
- 가설: decoder-only LLM의 자기회귀 능력을 그대로 활용하면 효율적이고 유연한 TSF 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 LLM 언어 토큰 임베딩 공간으로 투영 → LLM이 자기회귀적으로 미래 예측.
- **아키텍처**:
  - 시계열 패치 → 언어 토큰 임베딩 공간 선형 투영
  - LLM 임베딩 텍스트 타임스탬프: 다변량 시계열 정렬 용도
  - In-context forecasting: 이전 윈도우를 프롬프트로 활용하여 lookback 확장
  - LLaMA, GPT-2 등 임의 decoder-only LLM 호환
  - 0.1% 파라미터만 학습 (선형 투영층만)
- **손실함수**: Autoregressive MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/512 (+ in-context 확장 가능)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- Train/Val/Test split: 표준 ETT 분할
- Optimizer: Adam (선형 투영층만); LR: 표준
- Loss function: MSE
- Hardware: RTX 3090-24G (LLaMA-7B 기준 ~15분 훈련)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | LLM 기반 모델 SOTA |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- Decoder-only LLM 자기회귀 능력 TSF에 활용 (0.1% 파라미터)
- LLM 임베딩 텍스트 타임스탬프로 다변량 시계열 정렬
- In-context forecasting으로 lookback 윈도우 확장
- 5× 이상 훈련/추론 속도 개선 vs 기존 LLM 기반 모델

## 5. 한계 및 후속 연구 아이디어
- LLM 크기가 클수록 성능이 개선되는지 확인 필요
- 텍스트 타임스탬프의 효용성에 대한 추가 ablation 필요

## 6. 참고
- [GitHub](https://github.com/thuml/AutoTimes)
- [arXiv 2402.02370](https://arxiv.org/abs/2402.02370)
