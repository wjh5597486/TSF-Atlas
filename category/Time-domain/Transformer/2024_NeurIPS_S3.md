---
title: "Segment, Shuffle, and Stitch: A Simple Layer for Improving Time-Series Representations"
authors: Rupert Mitchell et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2405.20082
code_url: ""
tags: [transformer, representation-learning, plug-and-play, patching, shuffling, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_S3.pdf
pdf_source: arxiv
---

## TL;DR
S3(Segment, Shuffle, and Stitch)는 기존 시계열 모델에 plug-and-play로 추가 가능한 간단한 레이어. 비중첩 세그먼트를 태스크 최적화 방식으로 학습 가능하게 셔플링하여 표현 학습을 개선.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 모델은 순차적 입력 구조에 의존하여 표현 학습이 국소적.
- 중요한 타임포인트/패턴을 Transformer가 스스로 집중하도록 유도하는 메커니즘 부재.
- 가설: 세그먼트를 학습된 순서로 셔플링하면 모델이 시계열의 중요한 위치를 더 잘 학습.

## 2. 제안 방법론
- **핵심 아이디어**: Segment-Shuffle-Stitch — 비중첩 세그먼트로 분할, 학습 가능한 셔플, 재조합.
- **아키텍처**:
  - Segment: 시계열을 비중첩 세그먼트로 분할
  - Shuffle: 학습 가능한 순열 행렬로 세그먼트 재배열 (태스크 최적)
  - Stitch: 재배열된 세그먼트를 모델에 입력, 출력 후 원래 순서로 복원
  - 기존 Informer, Autoformer, PatchTST 등에 plug-and-play 적용
- **손실함수**: MSE (기본 모델 손실)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정 (기본 모델 기준)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | Informer 백본 대비 1.64~16.69% MSE 향상 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- Segment-Shuffle-Stitch 간단한 plug-and-play 레이어 제안
- 다양한 기존 모델(Informer, Autoformer 등)에 적용하여 1.64~16.69% 향상
- ETTh1 포함 여러 벤치마크에서 일관된 개선 확인
- 학습 가능한 셔플로 태스크 최적화 세그먼트 순서 발견

## 5. 한계 및 후속 연구 아이디어
- 셔플 순열 학습의 최적화 안정성 의문
- 매우 긴 시퀀스에서 계산 비용 증가

## 6. 참고
- [arXiv 2405.20082](https://arxiv.org/abs/2405.20082)
