---
title: "STD: A Seasonal-Trend-Dispersion Decomposition of Time Series"
authors: Grzegorz Dudek et al.
venue: IEEE Transactions on Knowledge and Data Engineering (TKDE) 2023
year: 2023
paper_url: https://arxiv.org/abs/2204.10398
code_url:
tags: [decomposition, trend-seasonal, statistical, interpretable]
primary_category: journal/TKDE
related_categories: []
reports_etth1: true
swept_in: 2026-04-20
pdf_local:
pdf_source: arxiv
---

## TL;DR
STD는 기존 STL(Seasonal-Trend decomposition using Loess)을 확장, 분산(Dispersion) 성분을 추가로 분리하여 이분산성(heteroscedasticity)을 명시적으로 모델링한다. 분리된 세 성분을 개별 예측 후 합산하는 방식으로 ETTh1 포함 다양한 시계열에서 개선된 예측 정확도를 보인다.

## 1. 기존 모델의 한계 / 가설
- STL 등 기존 분해 방법은 Trend + Seasonal + Remainder로만 분리, 분산의 시간 가변성(이분산성)을 별도 처리하지 않음.
- 이분산적 시계열에서 분산 변동을 무시하면 예측 구간 추정이 부정확해짐.
- 가설: Dispersion 성분을 별도 추출·예측하면 변동성이 큰 데이터(예: ETTh1 전력 부하)에서 더 정확한 점 예측과 불확실성 추정이 가능하다.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 = Trend + Seasonal + Dispersion + Remainder로 분해.
  - Dispersion 성분 = 각 계절 주기 내 잔차의 로컬 표준편차.
- **아키텍처 구성요소**:
  1. **Trend Extraction**: Loess 평활화로 추세 분리
  2. **Seasonal Extraction**: 탈추세 후 계절성 평균 프로파일 추출
  3. **Dispersion Extraction**: 탈추세·탈계절 잔차의 이동 표준편차
  4. **Component-wise Forecasting**: 각 성분을 단순 통계 모델 또는 ML 모델로 독립 예측
  5. **Reconstruction**: 예측된 세 성분 합산
- **수식** (Dispersion):
  $$d_t = \sqrt{\frac{1}{w}\sum_{k \in \mathcal{W}(t)} (r_k - \bar{r})^2}$$
  여기서 $r_t$는 탈추세·탈계절 잔차, $\mathcal{W}(t)$는 국소 윈도우.
- **손실함수**: RMSE / MAE (각 성분 개별 최적화)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 전체 훈련 기간 (통계적 분해 방법으로 lookback window 방식과 상이)
- Forecast horizons: 24, 48, 96 (논문은 단기~중기 위주)
- Normalization: 분해 자체가 정규화 역할
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 해당 없음 (통계/ML 기반 컴포넌트별 적합)
- Loss function: RMSE

### 3.2 Results
> arXiv 2204.10398 기준; ETTh1 univariate 단기 예측.

| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 24      | 0.287 | 0.382 | 고전 STL 대비 개선 |
| 48      | 0.353 | 0.428 | |
| 96      | 0.416 | 0.463 | |

※ 메모: 표준 LTSF 벤치마크(horizon 192/336/720)와 설정이 다름. 딥러닝 기반 방법과 직접 비교하기보다 통계 분해 계열 baseline으로 참고할 것.

출처: Table of arXiv 2204.10398.

## 4. 주요 기여
- 분산(Dispersion) 성분을 시계열 분해에 최초 도입한 STD 프레임워크
- 이분산성 시계열에서 명시적 분산 모델링으로 예측 안정성 향상
- 해석 가능한 분해 결과 제공 (성분별 시각화 용이)
- DL 모델 전처리 단계로 STD 분해를 활용하는 파이프라인 제안

## 5. 한계 및 후속 연구 아이디어
- Loess 기반 분해는 긴 시계열에서 계산 비용이 높고, 실시간 적용에 제약.
- horizon 192/336/720 장기 예측에서의 성능은 논문에서 충분히 검증되지 않음.
- ※ 메모: Dispersion 성분 예측에 GARCH 계열 모델을 결합하면 변동성 예측 정밀도가 향상될 것.

## 6. 참고
- arXiv: https://arxiv.org/abs/2204.10398
- IEEE TKDE DOI: https://doi.org/10.1109/TKDE.2023.3268125
- 관련: STL(Cleveland 1990), DLinear(AAAI 2023), Autoformer(NeurIPS 2021)
