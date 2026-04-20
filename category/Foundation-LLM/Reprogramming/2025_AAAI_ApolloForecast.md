---
title: "Apollo-Forecast: Overcoming Aliasing and Inference Speed Challenges in Language Models for Time Series Forecasting"
authors: Tianyi Yin et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2412.12226
code_url: ""
tags: [foundation-llm, reprogramming, anti-aliasing, inference-speed, zero-shot, tokenization]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_ApolloForecast.pdf
pdf_source: arxiv
---

## TL;DR
Apollo-Forecast는 LLM 기반 시계열 예측의 두 가지 문제인 앨리어싱 왜곡과 느린 추론 속도를 해결한다. AAQM(Anti-Aliasing Quantization Module)으로 토크나이제이션 시 고주파 노이즈를 제거하고 Race Decoding(RD)으로 1.9~2.7배 추론 가속을 달성한다.

## 1. 기존 모델의 한계 / 가설
- LLM 기반 TSF 모델의 토크나이제이션 과정에서 고주파 노이즈(앨리어싱) 발생
- 자기회귀(autoregressive) 방식의 LLM 추론이 시계열 예측에서 지나치게 느림
- 가설: (1) Butterworth 필터 기반 앨리어싱 방지, (2) 병렬 드래프트 모델로 추론 가속

## 2. 제안 방법론
- **핵심 아이디어**: 앨리어싱 방지 토크나이제이션 + 병렬 레이스 디코딩
- **아키텍처 구성요소**:
  1. **Anti-Aliasing Quantization Module (AAQM)**: Butterworth 저역통과 필터로 고주파 노이즈 제거 후 토크나이제이션; 신호 충실도 및 양자화 효율성 향상
  2. **Time Series Forecasting Model (TSFM)**: 토크나이즈된 시계열 처리하는 Transformer 기반 seq2seq 예측기
  3. **Race Decoding (RD)**: 소형 draft 모델과 대형 메인 모델의 병렬 처리 + 허용 오차 기반 결과 병합; 1.9~2.7배 추론 가속
- **저자**: Tongji University 그룹 (Tianyi Yin, Jingwei Wang 등)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 표준
- Forecast horizons: 테스트 셋 전체 길이 (prediction horizon = length of test set); default horizon 64
- Normalization: 표준
- Train/Val/Test split: 표준
- Batch size / Optimizer / LR: bfloat16 정밀도
- Loss function: WQL, MASE (확률적 평가지표)
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, ECL, Traffic 7개 데이터셋

### 3.2 Results
| 데이터셋 | WQL | MASE | 비교 SOTA 대비 |
|---------|-----|------|----------------|
| ETTh1 (Apollo-Small) | 2.135 | 4.120 | zero-shot SOTA |
| 전체 | — | — | zero-shot WQL 최대 31.44% 감소 |

출처: Table 1 of paper.

## 4. 주요 기여
- AAQM: Butterworth 필터 기반 앨리어싱 방지 토크나이제이션
- Race Decoding: 병렬 드래프트-메인 모델로 1.9~2.7배 추론 가속
- Zero-shot 시계열 예측에서 SOTA WQL/MASE 달성
- LLM 기반 TSF의 실용화 장벽 해결

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: WQL/MASE 위주 확률적 평가, 결정론적 MSE/MAE 비교 직접 어려움
- ※ 메모: zero-shot 확률적 예측 중심; 표준 LTSF 결정론적 벤치마크와 직접 비교 주의 필요

## 6. 참고
- [arXiv 2412.12226](https://arxiv.org/abs/2412.12226)
- 비교: Chronos, TimesFM, MOIRAI
