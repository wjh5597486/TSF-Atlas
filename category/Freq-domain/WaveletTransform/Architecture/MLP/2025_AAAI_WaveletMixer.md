---
title: "WaveletMixer: A Multi-Resolution Wavelets Based MLP-Mixer for Multivariate Long-Term Time Series Forecasting"
authors: Zichi Zhang et al.
venue: AAAI
year: 2025
paper_url: ""
code_url: ""
tags: [freq-domain, wavelet, mlp, mixer, multi-resolution, long-term, multivariate]
primary_category: category/Freq-domain/WaveletTransform/Architecture/MLP
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_WaveletMixer.pdf
pdf_source: arxiv
---

## TL;DR
WaveletMixer는 웨이블릿 다중 해상도 특성을 활용하여 다중 주파수 도메인에 대한 복수의 예측 모델을 생성하는 반복적(iterative) 프레임워크이다. TS-Learner라는 MLP 기반 서브모델을 도입하며, MLP·LSTM·Transformer 등 임의 아키텍처에 범용으로 적용 가능하다.

## 1. 기존 모델의 한계 / 가설
- 단일 해상도로 시계열을 처리하면 다양한 주파수 특성을 놓칠 수 있음
- 기존 웨이블릿 방법들은 시계열의 로컬-글로벌 특성을 동시에 충분히 활용하지 못함
- 가설: 다중 레벨 웨이블릿 분해 + 반복적 모델 조정으로 예측 편향과 오차를 순차적으로 감소

## 2. 제안 방법론
- **핵심 아이디어**: 다중 해상도 웨이블릿 분해로 각 주파수 대역별 예측 모델 생성 후 반복적 조정
- **아키텍처 구성요소**:
  1. **Multi-level Wavelet Decomposition**: 시계열을 다중 해상도의 approximation/detail 계수로 분해
  2. **TS-Learner**: 각 해상도에 최적화된 MLP 기반 서브모델 (로컬+글로벌 관점 통합)
  3. **Iterative Adjustment**: 전체 레벨 예측 모델을 동시에 반복적으로 조정하여 편향 감소
  4. **General Framework**: MLP, LSTM, Transformer 등 임의 딥러닝 아키텍처에 적용 가능
- **저자**: Queen's University Belfast (Zichi Zhang, Tuan Dung Pham 등)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 chronological
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: 9개 실세계 데이터셋 평가 (ETTh1/h2/m1/m2 포함으로 추정)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 반복적 다중 레벨 웨이블릿 기반 예측 프레임워크
- TS-Learner: 장기 예측 특화 MLP 서브모델
- 로컬·글로벌 양방향 관점 동시 활용
- 임의 딥러닝 아키텍처에 plug-in 가능한 범용 설계

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 웨이블릿 타입 및 분해 레벨 선택 의존성
- ※ 메모: arXiv 프리프린트 링크 미확인 (AAAI DOI: 10.1609/aaai.v39i21.34434)
- ※ 메모: WPMixer(동일 venue)와 유사한 방향이나 반복적 조정 설계가 차별점

## 6. 참고
- AAAI 2025 proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/34434
- 비교: WPMixer (AAAI 2025), TimeMixer, PatchTST

※ 원본 PDF 미취득: arXiv 링크 미확인
