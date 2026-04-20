---
title: "Battling the Non-stationarity in Time Series Forecasting via Test-time Adaptation"
authors: HyunGi Kim et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2501.04970
code_url: https://github.com/kimanki/TAFAS
tags: [time-domain, test-time-adaptation, non-stationary, normalization, distribution-shift]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Normalization
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_TAFAS.pdf
pdf_source: arxiv
---

## TL;DR
TAFAS는 시계열 예측에서 non-stationarity(분포 이동)에 대응하는 테스트 타임 적응(TTA) 프레임워크이다. 부분 관측 ground truth와 gated calibration module을 활용해 source 예측 모델을 지속적으로 변화하는 테스트 분포에 모델-무관적으로 적응시킨다.

## 1. 기존 모델의 한계 / 가설
- 실세계 시계열은 시간이 지남에 따라 분포가 변화하여(non-stationarity) 학습 시 고정 분포 가정이 무너짐
- 기존 정규화 방법(RevIN 등)은 테스트 시점의 분포 이동을 완전히 처리하지 못함
- 가설: 테스트 시 부분 관측 데이터를 활용한 적응이 분포 이동에 효과적으로 대응 가능

## 2. 제안 방법론
- **핵심 아이디어**: 테스트 시점에 부분 관측 GT와 gated calibration으로 모델 적응
- **아키텍처 구성요소**:
  1. **Source Forecaster**: 사전 학습된 임의의 예측 모델 (backbone-agnostic)
  2. **Partial Ground Truth Utilization**: 테스트 시 사용 가능한 부분 GT를 적응 신호로 활용
  3. **Gated Calibration Module**: semantic 정보 보존하면서 분포 이동 보정
  4. **Model-agnostic**: iTransformer, DLinear 등 임의 backbone에 적용 가능
- **학습 전략**: source 모델 frozen + calibration 모듈만 업데이트

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 표준
- Forecast horizons: H∈{96, 192, 336, 720}
- Normalization: backbone 모델 설정 따름
- Train/Val/Test split: chronological (ETTh1: 0.6/0.2/0.2)
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2 사용; iTransformer, DLinear backbone 실험

### 3.2 Results
| Horizon | MSE (iTransformer+TAFAS) | 개선율 |
|---------|--------------------------|--------|
| 336     | — | iTransformer 대비 4.95% MSE 감소 |
| 720     | — | iTransformer 대비 5.76% MSE 감소 |
| Chronos | — | ETT 데이터셋 MSE 최대 45% 감소 |

출처: Table 2, Figure 3 of paper.

## 4. 주요 기여
- TSF를 위한 최초의 체계적 TTA 프레임워크 (TAFAS) 제안
- Partial GT 활용 방법론: 미관측 미래를 예측하면서 부분 GT 신호 사용
- 모델 무관(model-agnostic) 설계로 임의 backbone에 적용 가능
- Gated calibration module로 semantic 정보 보존

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 부분 GT가 없는 완전 온라인 세팅에서의 적용 한계
- ※ 메모: TAFAS는 모델 자체보다 적응 프레임워크이므로 독립된 아키텍처 분류보다 Normalization/distribution-shift 범주가 더 적합하나, 시계열 예측 개선 방법론으로 Transformer 카테고리에 분류

## 6. 참고
- [arXiv 2501.04970](https://arxiv.org/abs/2501.04970)
- [GitHub](https://github.com/kimanki/TAFAS)
- 관련: RevIN (ICLR 2022), Non-stationary Transformer (NeurIPS 2022)
