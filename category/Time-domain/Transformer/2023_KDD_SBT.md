---
title: "Sparse Binary Transformers for Multivariate Time Series Modeling"
authors: Matt Gorbett et al.
venue: KDD 2023
year: 2023
paper_url: https://arxiv.org/abs/2308.04637
code_url: 
tags: [transformer, sparse, binary, efficiency, compression, multivariate]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_KDD_SBT.pdf
pdf_source: arxiv
---

## TL;DR
SBT는 Lottery Ticket Hypothesis를 시계열 Transformer에 적용하여, dense float Transformer를 희소(sparse) 및 이진(binary) 가중치로 압축하면서도 유사한 예측 정확도를 유지하는 효율적 모델이다. 최대 53배 저장소 절감, 10.5배 FLOPs 감소를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 TSF 모델은 높은 파라미터 수와 연산 비용을 가짐.
- Lottery Ticket Hypothesis가 시계열 Transformer에도 적용 가능하다는 가설.
- 희소화(pruning) + 이진화(binarization)의 결합으로 정확도 손실 없이 극단적 압축 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Sparse + Binary Transformer를 시계열 모델링에 적용.
- **아키텍처 구성요소**:
  - 분류 태스크: fixed mask를 query/key/value activation에 적용
  - 예측/이상 탐지 태스크: 현재 시간 스텝에서만 연산하는 attention mask
  - 가중치 이진화(binarization): 부동소수점 가중치를 ±1로 양자화
  - 점진적 희소화 스케줄링
- **손실함수**: MSE (예측), cross-entropy (분류)
- **학습 전략**: 사전학습 후 pruning + binarization 적용

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: ETTh1, ETTm1 등 표준 TSF 벤치마크
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 dense Transformer와 유사한 정확도를 유지하면서 53배 저장소 절감, 10.5배 FLOPs 감소 보고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- Lottery Ticket Hypothesis를 시계열 Transformer에 적용
- Sparse + Binary 결합으로 극단적 모델 압축 달성
- 분류/예측/이상 탐지 등 다양한 TSF 태스크에서 검증
- 엣지 디바이스 배포 가능성 제시

## 5. 한계 및 후속 연구 아이디어
- 이진화로 인한 표현력 제한 가능성
- Pruning 비율에 따른 성능-효율 트레이드오프 분석 필요
- ※ 메모: 효율적 TSF 모델 방향에서 SparseTSF(ICML 2024)와 비교 가능

## 6. 참고
- 관련 논문: SparseTSF (ICML 2024), FITS (ICLR 2024), PatchTST (ICLR 2023)
- ACM DL: https://dl.acm.org/doi/10.1145/3580305.3599508
- arxiv: https://arxiv.org/abs/2308.04637
