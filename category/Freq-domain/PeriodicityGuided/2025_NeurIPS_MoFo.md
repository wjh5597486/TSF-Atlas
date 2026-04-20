---
title: "MoFo: Empowering Long-term Time Series Forecasting with Periodic Pattern Modeling"
authors: Jiaming Ma et al.
venue: NeurIPS
year: 2025
paper_url: https://openreview.net/forum?id=sbvLts2HqR
code_url: https://github.com/PoorOtterBob/MoFo
tags: [freq-domain, periodicity, patching, transformer, mlp, periodic-modeling]
primary_category: category/Freq-domain/PeriodicityGuided
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_MoFo.pdf
pdf_source: openreview
---

## TL;DR
MoFo는 주기성을 기간 정렬된 타임스텝 간의 상관관계와 기간 오프셋 타임스텝 간의 추세로 동시에 해석하여, Period-Structured Patch(2D 텐서)와 Period-Aware Modulator로 장기 예측에서 주기 패턴을 직접 모델링한다. 17개 SOTA 대비 경쟁적 성능을 유지하면서 메모리 효율 14배, 학습 속도 10배 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 모델들은 시계열 주기성을 간접적으로만 처리하거나, 주기 패턴의 두 가지 측면(정렬된 타임스텝 간 상관, 오프셋 타임스텝 간 추세)을 분리하지 못함.
- TimesNet 등은 FFT로 주기를 식별하고 2D 변환을 수행하지만, 주기 내 정렬 구조를 직접 활용하지 않음.
- 가설: 주기 정렬 타임스텝을 하나의 행으로 묶는 Period-Structured Patch를 사용하면 주기 상관관계를 직접 모델링할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: Period-Structured Patch(2D 텐서) 설계 - 각 행에 주기 정렬 타임스텝만 포함.
- **아키텍처**:
  - **Period-Structured Patching**: 이산 샘플링으로 생성한 2D 텐서. 각 행은 동일 주기 내 정렬 타임스텝.
  - **Period-Aware Modulator**: 기간 오프셋 타임스텝 간의 추세를 캡처하는 학습 가능한 완화(relaxation) 함수 포함 adaptive inductive bias 모듈.
  - 전체 모델은 경량 아키텍처로 구성.
- **주요 특성**: 메모리 효율성 (최대 14배), 학습 속도 (최대 10배) 향상.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 논문 참조
- Forecast horizons: 96, 192, 336, 720
- Normalization: 인스턴스 정규화(표준)
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 17개 baselines와 비교, 다수 시계열 벤치마크 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체적 수치는 논문/OpenReview 참조 필요.

## 4. 주요 기여
- 주기성을 (1) 기간 정렬 타임스텝 간의 상관관계, (2) 기간 오프셋 타임스텝 간의 추세로 이중 해석.
- Period-Structured Patch (2D 텐서) 설계로 주기 패턴 직접 모델링.
- Period-Aware Modulator를 통한 적응형 inductive bias.
- 메모리·속도 효율 대폭 향상(14× 메모리, 10× 속도).

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 주기 길이 추정의 정확도에 의존.
- 확장 아이디어: 다중 주기 동시 모델링, 비규칙 샘플링 시계열에 적용.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=sbvLts2HqR
- GitHub: https://github.com/PoorOtterBob/MoFo
※ 원본 PDF 미취득: arXiv ID 없음. OpenReview에서 다운로드 필요.
