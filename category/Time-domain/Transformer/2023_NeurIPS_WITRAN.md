---
title: "WITRAN: Water-wave Information Transmission and Recurrent Acceleration Network for Long-range Time Series Forecasting"
authors: Yuxin Jia et al.
venue: NeurIPS
year: 2023
paper_url: https://openreview.net/forum?id=y08bkEtNBK
code_url: https://github.com/Water2sea/WITRAN
tags: [rnn, recurrent, long-range, forecasting, time-domain, multi-scale, information-transmission]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_NeurIPS_WITRAN.pdf
pdf_source: publisher
---

## TL;DR
WITRAN은 물결(water-wave) 정보 전달 메커니즘을 모사하여 장·단기 반복 패턴을 이중 입도(bi-granular) 정보 전달로 포착하고, 전역·지역 상관관계를 모델링하는 순환 네트워크다. NeurIPS 2023 Spotlight로 선정되었으며, 장·초장기 예측 태스크에서 기존 SOTA 대비 5.80%~14.28% 개선을 달성했다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer/MLP 계열 모델은 장기 예측에서 장·단기 패턴의 동시 포착에 한계가 있다.
- 순수 순환 모델(LSTM, GRU)은 긴 시퀀스에서 기울기 소실 문제와 낮은 연산 효율로 어려움이 있다.
- 가설: 물결 전파 방식의 이중 입도 정보 전달로 전역·지역 상관관계를 효과적으로 모델링할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 물결 전파를 모사한 bi-granular 정보 전달 메커니즘으로 장·단기 패턴의 동시 학습.
- **아키텍처**:
  1. **Bi-granular Information Transmission**: 세밀(fine)·거친(coarse) 두 입도로 시계열 정보를 병렬 처리.
  2. **Recurrent Acceleration**: 순환 구조를 통해 전역·지역 시간 상관관계 모델링.
  3. 다중 스케일 특징을 합산하여 최종 예측.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (장기) / 더 긴 길이 (초장기)
- Forecast horizons: 96, 192, 336, 720 (장기); 더 긴 horizon (초장기)
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | see Table 1 |
| 192     | see paper | see paper | see Table 1 |
| 336     | see paper | see paper | see Table 1 |
| 720     | see paper | see paper | see Table 1 |

출처: Table 1 of paper (ETTh1 포함, 장·초장기 예측 결과 모두 포함).

## 4. 주요 기여
- 물결 정보 전달 메커니즘을 모사한 bi-granular 정보 처리.
- 장기 및 초장기 예측에서 SOTA 대비 5.80%~14.28% 성능 개선.
- NeurIPS 2023 Spotlight 선정.
- 전역·지역 시간 상관관계의 통합 학습.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 매우 복잡한 비정상 시계열에서 패턴 분리 어려움.
- ※ 메모: 순환 구조의 부활 - Transformer 지배적 환경에서 RNN 계열의 경쟁력을 재확인. Mamba (SSM) 계열과의 비교 흥미.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=y08bkEtNBK
- GitHub: https://github.com/Water2sea/WITRAN
- NeurIPS 2023 Spotlight
- ※ 원본 PDF: NeurIPS proceedings 취득 (arXiv 없음)
