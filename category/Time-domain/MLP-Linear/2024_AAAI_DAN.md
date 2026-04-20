---
title: "Learning from Polar Representation: An Extreme-Adaptive Model for Long-Term Time Series Forecasting"
authors: David Anastasiu et al.
venue: AAAI
year: 2024
paper_url: https://arxiv.org/abs/2312.08763
code_url: https://github.com/davidanastasiu/dan
tags: [time-domain, mlp, extreme-value, polar-representation, multivariate]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_DAN.pdf
pdf_source: arxiv
---

## TL;DR
DAN(Distance-weighted Auto-regularized Neural network)은 극좌표 표현(polar representation)을 활용하여 극단값(extreme values)에 적응적인 장기 시계열 예측 모델이다. 거리 가중 다중 손실 함수와 쌓을 수 있는 블록을 통해 외생 데이터로부터 지시 시퀀스를 동적으로 정제하며, 가우시안 혼합 확률 모델링을 통해 단변량 시계열도 처리한다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 홍수, 폭풍 등 극단적 이벤트(extreme events)를 잘 예측하지 못함 — 학습 데이터에서 희귀하기 때문.
- 일반적인 MSE 기반 학습은 평균에 편향되어 극단값을 평탄화(smoothing)하는 경향.
- 가설: 극좌표 표현을 통해 극단값의 크기와 방향을 분리하면 보다 적응적인 예측이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 극좌표(magnitude, direction)로 변환하여 극단값을 명시적으로 처리.
- **DAN 구성요소**:
  1. **Polar Representation**: 입력 시계열을 크기(magnitude)와 방향(direction) 성분으로 변환.
  2. **Stackable Blocks**: 외생 데이터로부터 지시(indicator) 시퀀스를 반복적으로 정제.
  3. **Distance-weighted Multi-loss Mechanism**: 극단값에 더 높은 가중치를 부여하는 손실 함수로 편향 완화.
  4. **Gaussian Mixture Modeling**: 단변량 시계열에서의 확률적 예측 지원.
- 손실함수: 거리 가중 MSE (distance-weighted multi-loss).
- 학습 전략: 표준 지도학습.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정 (96~512)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETTh1 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: Distance-weighted MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity 등 표준 벤치마크 사용

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | 극단값 시나리오에서 우수 |
| 192     | see paper | see paper | 극단값 시나리오에서 우수 |
| 336     | see paper | see paper | 극단값 시나리오에서 우수 |
| 720     | see paper | see paper | 극단값 시나리오에서 우수 |

출처: Table 1 of paper (arXiv 2312.08763).

## 4. 주요 기여
- 극좌표 표현을 장기 시계열 예측에 최초로 도입.
- 극단값에 적응적인 거리 가중 다중 손실 함수 제안.
- 쌓을 수 있는 블록 구조로 외생 데이터 기반 정제 지원.
- 가우시안 혼합 모델로 단변량/다변량 시계열 모두 처리.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 극좌표 변환의 해석 가능성에 대한 이론적 보장 부족.
- ※ 메모: 극단값 적응 손실을 확산 모델(diffusion)과 결합하는 방향 흥미로움.
- ※ 메모: 환경/기후 예측에서의 응용 가치가 높음.

## 6. 참고
- arXiv: https://arxiv.org/abs/2312.08763
- GitHub: https://github.com/davidanastasiu/dan
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/27768
