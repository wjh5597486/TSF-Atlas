---
title: "Efficiently Enhancing Long-term Series Forecasting via Ultra-long Lookback Windows"
authors: Suxin Tong et al.
venue: AAAI
year: 2025
paper_url: ""
code_url: ""
tags: [time-domain, mlp, long-term, lookback, decomposition, patching, ultra-long]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Patching
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_IRPA.pdf
pdf_source: arxiv
---

## TL;DR
IRPA(Input Refinement and Prediction Auxiliary)는 4개의 선형 레이어로 구성된 경량 프레임워크로, 초장기 lookback 윈도우에서 핵심 정보를 효율적으로 추출한다. IRM(입력 정제)과 PAM(예측 보조) 두 모듈로 평균 16.1% MSE 감소를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 표준 LTSF 모델은 일반적으로 lookback 길이 96~336을 사용하여 장기 역사 패턴을 충분히 활용하지 못함
- 초장기 lookback 사용 시 과적합과 파라미터 폭발 문제 발생
- 가설: 경량 입력 정제 모듈로 초장기 lookback을 효율적으로 처리하면 예측 정확도 향상

## 2. 제안 방법론
- **핵심 아이디어**: 4개 선형 레이어로 구성된 경량 IRM+PAM이 초장기 lookback 처리
- **아키텍처 구성요소**:
  1. **Input Refinement Module (IRM)**: 초장기 시계열의 효과적 분해 및 패칭 수행; 계절·추세 특징의 정보 밀도 향상; 2개의 선형 레이어 서브모듈
  2. **Prediction Auxiliary Module (PAM)**: 초장기 lookback에서 역사 유사성 및 계절 패턴 추출; 예측 정확도 향상; 2개의 선형 레이어 서브모듈
  3. **임의 백본 통합**: 기존 LTSF 모델에 plug-in 형태로 적용 가능

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): {480, 1920, 3600} (데이터셋 샘플링 주파수에 따라 결정)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준
- Train/Val/Test split: 표준 chronological
- Batch size: 32
- Loss function: MSE
- 기타: 10 epochs, series refinement length 및 patch length = 96; ETTh1 포함 8개 데이터셋

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg     | see paper | see paper | 평균 16.1% MSE 감소 |
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 2 of paper.

## 4. 주요 기여
- 초장기 lookback 윈도우 처리를 위한 경량(4-linear-layer) 프레임워크
- IRM: 정보 밀도 향상을 위한 분해+패칭
- PAM: 역사 유사성 및 계절성 패턴 추출
- 8개 데이터셋에서 평균 16.1% MSE 감소
- 임의 백본 모델에 plug-in 적용 가능

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 초장기 lookback에 의한 메모리 요구량 증가
- ※ 메모: arXiv 프리프린트 링크 미확인 (AAAI DOI: 10.1609/aaai.v39i20.35386)

## 6. 참고
- AAAI 2025 proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/35386
- 비교: PatchTST, TimeMixer, DLinear 등 표준 LTSF 모델

※ 원본 PDF 미취득: arXiv 링크 미확인, AAAI proceedings PDF 다운로드 실패
