---
title: "DeformableTST: Transformer for Time Series Forecasting without Over-reliance on Patching"
authors: Donghao Luo and Xue Wang
venue: NeurIPS 2024
year: 2024
paper_url: https://proceedings.neurips.cc/paper_files/paper/2024/hash/a0b1082fc7823c4c68abcab4fa850e9c-Abstract-Conference.html
code_url: https://github.com/luodhhh/DeformableTST
tags: [transformer, deformable-attention, time-domain, patching-free]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_DeformableTST.pdf
pdf_source: arxiv
---

## TL;DR
DeformableTST는 Transformer가 패칭(patching)에 과도하게 의존하는 문제를 해결하기 위해 변형 가능한 어텐션(Deformable Attention)을 도입. 학습 가능한 샘플링 포인트로 글로벌 + 로컬 시간 정보를 동시에 포착.

## 1. 기존 모델의 한계 / 가설
- PatchTST 이후 대부분 Transformer TSF 모델이 패칭에 의존하여 패칭에 적합하지 않은 태스크(단기 예측, 이상 탐지)에 취약.
- Full attention은 중요한 타임포인트에 집중하기 위해 패칭 가이던스에 의존.
- 가설: 학습 가능한 샘플링 좌표로 중요한 시간 포인트에 직접 attend할 수 있으면 패칭 의존성 제거 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Deformable Attention — 균일한 sparse reference points + 학습 가능한 offsets로 중요 타임포인트 샘플링.
- **아키텍처**:
  - Reference points: 균일하게 분산된 기준 위치
  - Sampling offsets: 학습 가능한 변위로 중요 위치 적응적 선택
  - 글로벌 정보: Deformable attention
  - 로컬 정보: Depth-wise 컨볼루션 주입 FFN (ConvFFN)
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96/336/720
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | 다양한 태스크에서 일관된 SOTA |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 패칭 의존성 없는 Transformer TSF 최초 제안
- Deformable Attention으로 적응적 타임포인트 샘플링
- ConvFFN으로 글로벌+로컬 정보 동시 포착
- 패칭 불적합 태스크(단기 예측, 이상탐지 등)에서도 우수한 성능

## 5. 한계 및 후속 연구 아이디어
- Deformable attention의 학습 안정성
- 매우 긴 시퀀스에서 sampling offset 수렴 문제 가능

## 6. 참고
- [GitHub](https://github.com/luodhhh/DeformableTST)
