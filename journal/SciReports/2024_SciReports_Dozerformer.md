---
title: "Sparse Transformer with Local and Seasonal Adaptation for Multivariate Time Series Forecasting"
authors: Yifan Zhang et al.
venue: Scientific Reports (Springer Nature) 2024
year: 2024
paper_url: https://arxiv.org/abs/2312.06874
code_url: https://github.com/yfzhang114/Dozerformer
tags: [time-domain, transformer, sparse-attention, seasonal, local]
primary_category: journal/SciReports
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: arxiv
---

## TL;DR
Dozerformer는 Local, Stride, Vary 세 가지 희소 어텐션 컴포넌트를 조합한 "Dozer Attention"을 제안, 지역성·주기성·장거리 의존성을 효율적으로 포착한다. ETTh1 포함 다수 벤치마크에서 dense self-attention 대비 계산 효율과 예측 성능을 동시에 개선한다.

## 1. 기존 모델의 한계 / 가설
- Vanilla Transformer의 O(L²) 어텐션은 긴 시퀀스에서 계산 비용이 급증하면서도 지역 패턴과 주기성을 효과적으로 포착하지 못함.
- 기존 희소 어텐션(Informer LogSparse, Autoformer Auto-Correlation 등)은 특정 패턴만 선택해 다양한 의존성을 누락.
- 가설: 지역성·스트라이드·가변 범위를 조합한 멀티-컴포넌트 희소 어텐션이 세 유형의 의존성을 균형 있게 포착할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: Dozer Attention = Local Attention + Stride Attention + Vary Attention.
- **아키텍처 구성요소**:
  1. **Local Attention**: 인접 윈도우 내 토큰만 어텐션 → 지역 의존성 포착
  2. **Stride Attention**: 일정 간격(stride)으로 샘플링된 토큰 간 어텐션 → 주기성 포착
  3. **Vary Attention**: 쿼리마다 다른 범위를 동적으로 선택 → 장거리 가변 의존성
  4. 세 컴포넌트 출력을 concat 후 linear projection
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam, lr=1e-4 (논문 기본 설정)
- Loss function: MSE

### 3.2 Results
> arXiv 2312.06874 기준; ETTh1 multivariate.

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.386 | 0.402 | PatchTST 대비 소폭 개선 |
| 192     | 0.427 | 0.430 | |
| 336     | 0.444 | 0.450 | |
| 720     | 0.467 | 0.480 | |

※ 메모: 수치는 arXiv 초고 근사값. Scientific Reports 최종판 확인 필요.

출처: Table of arXiv 2312.06874.

## 4. 주요 기여
- 세 가지 희소 어텐션 컴포넌트를 통합한 Dozer Attention 설계
- 복잡도 O(L²) → 대폭 감소, 긴 시퀀스에서 실용적
- 지역성·주기성·장거리 의존성을 단일 모듈에서 동시 처리
- ETTh1/ETTh2/ETTm1/ETTm2/Weather/Electricity 다수 벤치마크에서 일관된 성능

## 5. 한계 및 후속 연구 아이디어
- 세 컴포넌트의 비율(local window size, stride 크기 등)을 데이터 특성에 맞게 자동 탐색하는 메커니즘 부재.
- ※ 메모: NAS(Neural Architecture Search)나 AutoML로 컴포넌트 하이퍼파라미터를 자동화하면 범용성이 높아질 것.

## 6. 참고
- arXiv: https://arxiv.org/abs/2312.06874
- Scientific Reports DOI: https://doi.org/10.1038/s41598-024-66886-1
- 관련: Informer(AAAI 2021), Autoformer(NeurIPS 2021), PatchTST(ICLR 2023)
