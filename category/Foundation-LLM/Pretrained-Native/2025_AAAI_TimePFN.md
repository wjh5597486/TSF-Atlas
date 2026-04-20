---
title: "TimePFN: Effective Multivariate Time Series Forecasting with Synthetic Data"
authors: Ege Onur Taga et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2502.16294
code_url: https://github.com/egetaga/TimePFN
tags: [foundation-llm, pretrained-native, synthetic-data, zero-shot, few-shot, gaussian-process, transformer]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_TimePFN.pdf
pdf_source: arxiv
---

## TL;DR
TimePFN은 Prior-data Fitted Networks(PFN) 개념을 시계열에 적용하여 합성 데이터만으로 사전 학습하는 다변량 시계열 예측 모델이다. 가우시안 프로세스 커널과 선형 공동 지역화로 합성 데이터를 생성하고, zero-shot 및 few-shot 세팅에서 SOTA를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 파운데이션 모델은 대량의 실제 데이터 사전 학습이 필요
- 합성 데이터 기반 사전 학습이 시계열에 효과적일 수 있음 (NLP의 PFN 성공 경험)
- 가설: 다양한 가우시안 프로세스 커널 조합으로 생성한 합성 MTS가 실제 데이터의 다양한 패턴을 충분히 커버 가능

## 2. 제안 방법론
- **핵심 아이디어**: Gaussian Process 기반 합성 데이터 생성 + PFN 아키텍처로 Bayesian inference 근사
- **아키텍처 구성요소**:
  1. **Synthetic MTS Generation**: 다양한 GP 커널(RBF, Periodic, Linear, Matern 등) + 선형 공동 지역화(LCM)로 교차채널 의존성 포함 합성 데이터 생성
  2. **PFN Transformer Architecture**: 시간 및 교차채널 의존성을 모든 입력 패치에 걸쳐 처리
  3. **In-Context Learning**: few-shot 데이터를 컨텍스트로 활용
  4. **Fine-tuning**: 소량(50~500개) 실제 데이터로 파인튜닝 지원

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 표준 설정
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준
- Train/Val/Test split: zero-shot (사전 학습 후 평가) / few-shot (500 포인트 이하)
- Batch size / Optimizer / LR: 파운데이션 모델 학습 설정
- Loss function: 논문 참조
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Solar Energy, ECL, Exchange, Traffic 9개 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg     | see paper | see paper | zero-shot/few-shot SOTA |
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1, Table 2 of paper.

## 4. 주요 기여
- 합성 데이터만으로 사전 학습하는 MTS 파운데이션 모델
- 다양한 GP 커널 + LCM으로 현실적 다변량 시계열 합성
- Zero-shot 및 few-shot SOTA 달성
- 50개 데이터 포인트로도 경쟁력 있는 성능
- Bayesian inference 근사로 불확실성 추정 가능

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 합성 데이터가 실제 비정상성(non-stationarity) 완전 포착 한계
- ※ 메모: zero-shot 중심 평가이므로 표준 supervised LTSF 대비 직접 비교 어려울 수 있음

## 6. 참고
- [arXiv 2502.16294](https://arxiv.org/abs/2502.16294)
- [GitHub](https://github.com/egetaga/TimePFN)
- 원본 개념: PFN (Prior-data Fitted Networks, Müller et al. 2022)
- 비교: Chronos, TimesFM, MOIRAI
