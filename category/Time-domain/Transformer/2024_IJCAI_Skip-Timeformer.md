---
title: "Skip-Timeformer: Skip-Time Interaction Transformer for Long Sequence Time-Series Forecasting"
authors: (저자 정보 원문 확인 필요)
venue: IJCAI 2024
year: 2024
paper_url: https://www.ijcai.org/proceedings/2024/608
code_url: 
tags: [transformer, multi-scale, skip-time, long-term-forecasting, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_IJCAI_Skip-Timeformer.pdf
pdf_source: publisher
---

## TL;DR
Skip-Timeformer는 시계열을 서로 다른 time interval을 기반으로 여러 subsequence로 분해하고, skip-time interaction 메커니즘으로 건너뛰는 시간 차원에서의 의존성을 포착하는 장기 시계열 예측 Transformer이다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델은 연속 시간 단계 간의 의존성에 집중하여, 일정 간격으로 건너뛰는(skip-time) 시간 단계 간의 장거리 패턴을 놓침.
- 다양한 time interval로 subsequence를 생성하면 skip-time 차원의 상관관계를 명시적으로 학습할 수 있다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 여러 time interval 기반 subsequence로 분해 후, skip-time interaction으로 각 subsequence 간 의존성을 포착.
- **아키텍처 구성요소**:
  - 다수의 time interval로 subsequence 생성 및 토큰 임베딩
  - Multi-head self-attention을 multi-skip sequence token embedding에 적용
  - **STICLN (Skip-Time Interaction Conditional Layer Normalization)**: subsequence 간의 차이를 줄이는 conditional normalization
- **손실함수**: MSE
- **하이퍼파라미터**: µ=2 (ETTh, Electricity, Weather, Traffic 데이터셋 기준)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): t = 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: STICLN 적용
- Train/Val/Test split: 표준 ETT 분할 (ETTh1, ETTh2, ETTm1, ETTm2 + Electricity, Weather, Traffic)
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 long-sequence 예측에서 Transformer 변형 모델 대비 우수한 성능과 임의 lookback window에 대한 적응성 보고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- Skip-time interaction 개념 도입: 건너뛰는 시간 차원의 의존성 명시적 포착
- STICLN: subsequence 간의 분포 차이를 줄이는 조건부 정규화
- 다양한 lookback window에 대한 적응성
- ETTh, Electricity, Weather, Traffic 데이터셋에서 우수한 성능

## 5. 한계 및 후속 연구 아이디어
- subsequence 수와 interval 설정이 성능에 미치는 민감도 분석 필요
- ※ 메모: Patching(PatchTST)과 Skip-time의 결합 아이디어와 비교 가능

## 6. 참고
- 관련 논문: PatchTST (ICLR 2023), iTransformer (ICLR 2024), Pathformer (ICLR 2024)
- IJCAI 2024 proceedings: https://www.ijcai.org/proceedings/2024/608
- PDF: https://www.ijcai.org/proceedings/2024/0608.pdf

※ 원본 PDF 취득: IJCAI 공개 proceedings PDF.
