---
title: "IN-Flow: Instance Normalization Flow for Non-stationary Time Series Forecasting"
authors: Wei Fan et al.
venue: KDD
year: 2025
paper_url: https://arxiv.org/abs/2401.16777
code_url: ""
tags: [normalization, freq-domain, flow, non-stationary, plugin]
primary_category: category/CrossCutting/Normalization
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_KDD_IN-Flow.pdf
pdf_source: arxiv
---

## TL;DR
IN-Flow는 분포 이동(distribution shift) 제거 과정을 예측(forecasting)과 분리하여 bi-level 최적화 문제로 공식화한다. Instance Normalization과 coupling layer를 결합한 invertible network(IN-Flow)를 통해 원시 분포를 목표 분포로 변환한 후 기존 예측 모델에 입력한다. 다양한 백본 모델에 플러그인으로 적용 가능하다.

## 1. 기존 모델의 한계 / 가설
- RevIN, SAN 등 기존 정규화 기법은 분포 이동과 예측을 결합하여 처리하여 최적 분리가 되지 않음.
- 기존 normalizing flow 모델은 사전 분포(prior distribution)로 정규화하는 방향이나, 시계열에서는 목표 분포가 알려지지 않음.
- 저자들은 분포 변환과 예측을 완전히 분리한 bi-level 최적화로 각 단계의 최적화를 독립적으로 수행할 수 있다고 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 분포 이동 제거를 invertible network(IN-Flow)가 담당하고, 예측은 기존 백본이 담당하는 decoupled bi-level 최적화 프레임워크.
- **아키텍처 구성요소**:
  1. **Outer Loop (Transformation)**: IN-Flow가 원시 시계열 분포를 목표 분포로 변환.
  2. **Inner Loop (Forecasting)**: 변환된 시계열로 기존 예측 모델(Autoformer, N-BEATS 등) 학습.
  3. **IN-Flow Architecture**: Instance Normalization Layer + RealNVP-style Coupling Layers 스택으로 구성된 invertible network.
  4. **Inverse Transform**: 예측 후 역변환으로 원래 스케일로 복원.
- **손실함수**: MSE Loss (예측 손실) + transformation 목적함수.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 48, 96, 168, 336 (horizon과 동일하게 설정)
- Forecast horizons: 48, 96, 168, 336 (기존 Autoformer 설정 따름; 표준 96/192/336/720과 다름)
- Normalization: IN-Flow 자체가 normalization 모듈
- Train/Val/Test split: ETT 기본 설정
- Batch size / Optimizer / LR: 논문 기본 설정
- Loss function: MSE
- 기타: Autoformer + IN-Flow, N-BEATS + IN-Flow로 백본별 비교; 비표준 horizon 사용

### 3.2 Results
| Horizon | MSE (Autoformer+IN-Flow) | MAE (Autoformer+IN-Flow) | 비교 SOTA 대비 |
|---------|--------------------------|--------------------------|----------------|
| 48      | - | - | (horizon 48 결과는 논문 참조) |
| 96      | 1.428 | 2.141 | Autoformer baseline(1.710) 대비 개선 |
| 168     | 1.303 | 2.095 | |
| 336     | 1.502 | 2.305 | |

출처: Table I of paper (arxiv:2401.16777v1). ※ 비표준 horizon (96/192/336/720이 아닌 48/96/168/336 사용).

## 4. 주요 기여
- 분포 이동 제거와 예측을 완전히 분리하는 decoupled bi-level 최적화 프레임워크 제안.
- Instance Normalization과 coupling layer를 결합한 invertible network(IN-Flow) 설계.
- 백본 모델에 무관한 플러그인 방식으로 다양한 예측 모델에 적용 가능.
- ETTh1/ETTh2/ETTm2/Weather/CAISO/NordPool 데이터셋 실험.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: bi-level 최적화의 수렴 보장 및 계산 비용이 단순 RevIN 대비 높음.
- ※ 메모: 표준 LTSF 벤치마크(96/192/336/720 horizon)로 재실험하면 최신 모델들과 직접 비교 가능. 비교 설정이 구버전(Autoformer 시대) 기준.

## 6. 참고
- [arxiv:2401.16777](https://arxiv.org/abs/2401.16777)
- [ACM DL: KDD 2025](https://dl.acm.org/doi/10.1145/3690624.3709260)
