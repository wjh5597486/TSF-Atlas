---
title: "Unlocking the Power of LSTM for Long Term Time Series Forecasting"
authors: Yaxuan Kong et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2408.10006
code_url: https://github.com/Eleanorkong/P-sLSTM
tags: [time-domain, ssm, lstm, slstm, patching, channel-independent, long-term]
primary_category: category/Time-domain/SSM-Mamba
related_categories:
  - category/CrossCutting/Patching
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_PsLSTM.pdf
pdf_source: arxiv
---

## TL;DR
P-sLSTM는 NLP용으로 개발된 sLSTM에 패칭(patching)과 채널 독립성을 적용하여 LTSF에 최적화한 모델이다. sLSTM의 잠재적 단기 기억 문제를 해결하고 SOTA 성능을 달성한다. AAAI 2025 Oral (Top 5%) 논문.

## 1. 기존 모델의 한계 / 가설
- 전통 LSTM은 장기 시계열 예측에서 gradient vanishing 및 장기 의존성 포착 한계
- sLSTM (xLSTM의 구성요소)은 지수 게이팅과 메모리 믹싱으로 NLP에서 우수하나 TSF에 직접 적용 시 잠재적 단기 기억 문제
- 가설: 패칭과 채널 독립성을 sLSTM에 결합하면 장기 시계열 예측 성능이 크게 향상

## 2. 제안 방법론
- **핵심 아이디어**: sLSTM + 패칭 + 채널 독립성 = P-sLSTM
- **아키텍처 구성요소**:
  1. **sLSTM (Scalar LSTM)**: 지수 게이팅(exponential gating)과 메모리 믹싱으로 장기 의존성 포착
  2. **Patching**: 시계열을 패치로 분할하여 입력 시퀀스 길이 압축 (단기 기억 문제 완화)
  3. **Channel Independence (CI)**: 각 채널 독립 처리로 과적합 방지 및 계산 효율성 향상
  4. **Linear Projection**: 입출력 차원 조정
- **처리 흐름**: 입력 → 선형 투영 → sLSTM 블록 → 패치 생성 → 최종 예측

## 3. ETTh1 실험 결과
> 본 논문은 ETTh1 결과를 보고하지 않음. Weather, Electricity, Solar, ETTm1, PEMS03 데이터셋 평가.
> `reports_etth1: false`

## 4. 주요 기여
- sLSTM의 TSF 적용 가능성 실증 (패칭 + CI 결합)
- 지수 게이팅으로 기존 LSTM 대비 장기 의존성 포착 향상
- 패칭으로 sLSTM의 단기 기억 문제 해결
- Weather, Electricity, Solar, ETTm1 등에서 SOTA 달성
- AAAI 2025 Oral (Top 5%) 선정

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: ETTh1 미평가 (ETTm1 중심), PEMS 교통 데이터 포함
- ※ 메모: sLSTM이 SSM과 유사한 선택적 상태 공간 메커니즘을 활용하므로 SSM-Mamba 카테고리 적합

## 6. 참고
- [arXiv 2408.10006](https://arxiv.org/abs/2408.10006)
- [GitHub](https://github.com/Eleanorkong/P-sLSTM)
- 원본: xLSTM (Beck et al., 2024), sLSTM
- 비교: iTransformer, TimeMixer, Mamba 기반 모델
