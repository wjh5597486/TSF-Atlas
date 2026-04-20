---
title: "CrossLinear: Plug-and-Play Cross-Correlation Embedding for Time Series Forecasting with Exogenous Variables"
authors: Pengfei Zhou et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2505.23116
code_url: https://github.com/mumiao2000/CrossLinear
tags: [time-domain, mlp, linear, exogenous, cross-correlation, patching, channel-independent, plugin]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Patching
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_CrossLinear.pdf
pdf_source: arxiv
---

## TL;DR
CrossLinear은 외생 변수(exogenous variable)가 있는 시계열 예측을 위한 플러그인 모듈이다. Cross-Correlation Embedding으로 외생 변수와 목표 변수 간 의존성을 경량화하여 포착하고, Patch Embedding으로 단기 패턴을, Linear Head로 장기 패턴을 학습한다. ETTh1을 포함한 12개 데이터셋에서 SOTA 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 채널 독립(CI) 모델은 외생 변수와 목표 변수 간 상관관계를 활용하지 못하여 외생 변수 정보가 낭비됨.
- 기존 채널 의존(CD) 모델은 계산 복잡도가 높고, 외생 변수와 목표 변수 간 정보 혼재로 노이즈가 증가함.
- 저자들은 1D convolution 기반 cross-correlation embedding으로 경량하게 외생 의존성을 포착할 수 있다고 주장.

## 2. 제안 방법론
- **핵심 아이디어**: Cross-Correlation Embedding(1D Conv + residual)으로 외생 변수의 기여를 경량하게 포착하고, 기존 CI 모델에 plug-and-play 방식으로 통합.
- **아키텍처 구성요소**:
  1. **Cross-Correlation Embedding**: 1D Convolution으로 외생 변수와 목표 변수 간 교차 상관 추출 + Residual Connection.
  2. **Patch Embedding**: 입력 시퀀스를 패치로 분할하여 단기 시간 패턴 포착.
  3. **Linear Forecasting Head**: 장기 시간 의존성 모델링 및 최종 예측 생성.
  4. **Instance Normalization**: RevIN 계열 정규화.
  5. **Weight-sharing**: many-to-one에서 many-to-many로 확장 시 가중치 공유.
- **손실함수**: MSE Loss.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 기본 설정
- Forecast horizons: 96, 192, 336, 720
- Normalization: Instance Normalization
- Train/Val/Test split: ETT 기본 설정
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: MSE
- 기타: ETTh1은 6개 외생 변수 포함; many-to-one 및 many-to-many 태스크 평가; 12개 데이터셋 (ETT 계열 4개, ECL, Weather, Traffic, EPF 5개)

### 3.2 Results
ETTh1 (exogenous variables 포함, many-to-one 설정):
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.055 | 0.178 | TimeXer 대비 개선 |
| 192     | 0.072 | 0.205 | |
| 336     | 0.082 | 0.226 | |
| 720     | 0.080 | 0.225 | |

출처: Table 2 of paper (arxiv:2505.23116). ※ 외생 변수 포함 설정이므로 표준 univariate/multivariate 설정과 직접 비교 불가.

## 4. 주요 기여
- 외생 변수와 목표 변수 간 cross-correlation을 포착하는 경량 plug-and-play 모듈(CCE) 제안.
- 단기 패턴(Patch Embedding) + 장기 패턴(Linear Head)의 이중 시간 모델링 구조.
- Many-to-one에서 Many-to-many로 weight-sharing 기반 확장.
- 12개 데이터셋 장단기 예측 SOTA (many-to-one 30 MSE 1위, many-to-many 31 MSE 1위).

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 외생 변수가 없는 표준 LTSF 설정에서는 이점이 감소할 수 있음.
- ※ 메모: Cross-correlation embedding이 어떤 lag에서 외생 변수의 영향을 포착하는지 해석 가능성 분석 흥미.

## 6. 참고
- [arxiv:2505.23116](https://arxiv.org/abs/2505.23116)
- [GitHub: mumiao2000/CrossLinear](https://github.com/mumiao2000/CrossLinear)
- [ACM DL: KDD 2025 V.2](https://dl.acm.org/doi/10.1145/3711896.3736899)
