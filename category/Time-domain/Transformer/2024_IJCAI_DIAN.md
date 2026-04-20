---
title: "Decoupled Invariant Attention Network for Multivariate Time-series Forecasting"
authors: Haihua Xu et al.
venue: IJCAI 2024
year: 2024
paper_url: https://www.ijcai.org/proceedings/2024/275
code_url: 
tags: [transformer, invariant-learning, distribution-shift, channel-dependent, decoupled, multivariate]
primary_category: category/Time-domain/Transformer
related_categories: [category/CrossCutting/ChannelStrategy/ChannelDependent]
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_IJCAI_DIAN.pdf
pdf_source: publisher
---

## TL;DR
DIAN은 시공간적 관계에서 분포 변화(distribution shift)에 강건한 불변(invariant) 패턴과 변동(variant) 패턴을 분리 학습하는 다변수 시계열 예측 모델이다. Spatial/Temporal 두 차원에서 invariant-variant 분리를 수행하며, 파라미터 효율적 구조로 Traffic/Wiki 데이터셋에서 70% 이상 파라미터 절감을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 공간적(변수 간) 및 시간적 분포 변화에 취약함.
- 불변 패턴(invariant)과 변동 패턴(variant)을 분리 학습하면 distribution shift에 더욱 강건해진다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Spatial Decoupled Invariant-Variant Learning(SDIVL)과 Temporal Augmented Invariant-Variant Learning(TAIVL)로 공간/시간적 불변-변동 패턴을 각각 분리 학습.
- **아키텍처 구성요소**:
  - **SDIVL (Spatial Decoupled Invariant-Variant Learning)**: 공간 차원에서 invariant/variant attention score 분리
  - **TAIVL (Temporal Augmented Invariant-Variant Learning)**: 시간 차원에서 temporal intervention mechanism으로 개입 샘플 생성
  - **Temporal Intervention Mechanism**: 잠재적 분포 변화에 적응하기 위한 개입(intervened) 샘플 생성
  - **Joint Optimization**: 예측 오류 최소화를 위한 결합 최적화 전략
- **손실함수**: MSE + Joint Optimization 목적함수

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할 및 Traffic, Wiki 등
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. Traffic 및 Wiki 데이터셋에서 파라미터 볼륨 70.7%/50.2% 절감, 학습 속도 56.3%/46.9% 향상 보고. ETTh1 수치는 원문 참고.

출처: 원문 Table 참고.

## 4. 주요 기여
- 시공간적 Invariant-Variant 분리 학습 프레임워크 제안
- Spatial/Temporal 두 차원에서의 분포 변화 처리
- Temporal Intervention Mechanism으로 잠재적 변화 대응
- 파라미터 효율적 구조: Traffic/Wiki에서 70%+ 파라미터 절감

## 5. 한계 및 후속 연구 아이디어
- Invariant/variant 분리 기준이 데이터에 따라 달라질 수 있음
- ※ 메모: 인과 추론(causal inference) 관점과 연결 가능성; 인과 불변성(invariant causal prediction)과 유사한 동기

## 6. 참고
- 관련 논문: iTransformer (ICLR 2024), Crossformer (ICLR 2023)
- IJCAI 2024 proceedings: https://www.ijcai.org/proceedings/2024/275
- PDF: https://www.ijcai.org/proceedings/2024/0275.pdf
