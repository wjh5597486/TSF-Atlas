---
title: "Hierarchical Classification Auxiliary Network for Time Series Forecasting"
authors: Yanru Sun et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2405.18975
code_url: https://github.com/syrGitHub/HCAN
tags: [time-domain, classification-auxiliary, hierarchical, cross-entropy, plug-in, loss]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Loss
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_HCAN.pdf
pdf_source: arxiv
---

## TL;DR
HCAN은 시계열 값의 토크나이제이션과 cross-entropy 손실을 활용하는 계층적 분류 보조 네트워크이다. Uncertainty-Aware Classifier, Hierarchical Consistency Loss, Hierarchy-Aware Attention을 결합한 플러그인 모듈로 기존 MSE 기반 모델의 과평탄화(over-smoothing) 문제를 해결한다.

## 1. 기존 모델의 한계 / 가설
- MSE 손실 기반 모델은 예측값이 평균으로 수렴하는 과평탄화(over-smooth) 현상 발생
- 분류 방식의 cross-entropy 적용 시 시계열의 연속성 무시 문제
- 가설: 계층적 분류(거칠고 세밀한 분류 동시)와 불확실성 인식으로 과평탄화 해결 가능

## 2. 제안 방법론
- **핵심 아이디어**: 시계열 값의 계층적 분류 보조로 더 현실적인 예측 학습
- **아키텍처 구성요소**:
  1. **Uncertainty-Aware Classifier (UAC)**: Dirichlet 분포 기반 증거 이론으로 softmax 대체; 과신뢰 분류 방지
  2. **Hierarchical Consistency Loss (HCL)**: 세밀한(fine) + 거친(coarse) 분류기 간 symmetric KL divergence로 경계 효과 완화
  3. **Hierarchy-Aware Attention (HAA)**: 다중 granularity 고엔트로피 특징을 주의 메커니즘으로 통합; 백본 출력과 결합
- **적용**: Informer, Autoformer, PatchTST, SCINet, DLinear 백본에 플러그인 적용
- **분류 레벨**: K₂=2 (거칠), K₃=4 (세밀)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 (백본 모델 설정 따름)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: Adam, NVIDIA RTX 3090 24GB
- Loss function: MSE + cross-entropy (auxiliary)
- 기타: ETTm1, ETTm2, ETTh1, ETTh2, Exchange-Rate, Weather, Solar Wind 7개 데이터셋; 5회 실험 평균

### 3.2 Results
| Horizon | MSE (Informer+HCAN) | MSE (Informer) | 개선 |
|---------|----------------------|----------------|------|
| 96      | 0.703 | 0.950 | +26% |
| 기타    | see paper | see paper | 다수 향상 |

출처: Table 1 of paper.

## 4. 주요 기여
- 시계열 분류 보조 학습으로 과평탄화 해결
- Uncertainty-Aware Classifier로 경계 효과 완화
- Hierarchical Consistency Loss로 다중 granularity 일관성 유지
- Hierarchy-Aware Attention으로 고엔트로피 특징 통합
- 플러그인으로 기존 모델에 쉽게 적용 가능

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 분류 granularity 선택(K₂, K₃)에 따른 민감도
- ※ 메모: 손실 함수 관점에서 CrossCutting/Loss 심볼릭 링크 적절

## 6. 참고
- [arXiv 2405.18975](https://arxiv.org/abs/2405.18975)
- [GitHub](https://github.com/syrGitHub/HCAN)
- 비교: PatchTST, DLinear, SCINet + HCAN 플러그인 조합
