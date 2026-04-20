---
title: "MICN: Multi-scale Local and Global Context Modeling for Long-term Series Forecasting"
authors: Huiqiang Wang et al.
venue: ICLR
year: 2023
paper_url: https://openreview.net/forum?id=zt53IDUR1Ucode_url: https://github.com/wanghq21/MICN
tags: [time-domain, cnn, multi-scale, decomposition, isometric-convolution]
primary_category: category/Time-domain/CNN-TCN
related_categories:
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ""
pdf_source: none
---

## TL;DR
MICN(Multi-scale Isometric Convolution Network)은 다운샘플링 컨볼루션으로 로컬 특징을 추출하고, 등척 컨볼루션(isometric convolution)으로 글로벌 상관관계를 포착하는 이중 경로 구조를 제안한다. Multi-scale Hybrid Decomposition(MHD)으로 추세·계절 분해를 수행하며, 선형 복잡도로 6개 벤치마크 SOTA를 달성했다.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델은 O(L²) attention 복잡도로 긴 시퀀스 처리 효율이 낮음.
- 기존 CNN 모델은 local pattern에만 집중하거나, dilated convolution으로 글로벌 context 포착이 불완전함.
- 가설: downsampling convolution(로컬)과 isometric convolution(글로벌)을 결합하면 효율적으로 long-range dependency를 포착할 수 있음.

## 2. 제안 방법론
- **핵심 아이디어**: Multi-scale Hybrid Decomposition(MHD) + 이중 경로 CNN(downsampling + isometric convolution).
- **Multi-scale Hybrid Decomposition (MHD)**:
  - 이동 평균 필터(kernel k₁ < k₂ < ...)로 계층적 분해하여 추세(trend)와 계절(seasonal) 성분 분리.
- **Downsampling Convolution**: 로컬 특징 추출 (stride > 1).
- **Isometric Convolution**: 시퀀스 길이를 보존하면서 글로벌 상관관계 포착 (padding으로 등척 유지).
- **Multi-scale 처리**: 여러 scale에서 두 컨볼루션을 적용, 결과를 합산.
- **복잡도**: O(L) — 시퀀스 길이에 선형 비례.
- **손실함수**: MSE.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: {96, 192, 336, 720}
- Normalization: Z-score
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: batch size 32, Adam, lr 0.0001
- Loss function: MSE
- 기타: multi-scale 단계 수, 이동 평균 커널 크기 데이터셋마다 조정

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | 0.383 | 0.406 | FEDformer 0.376/0.419 대비 유사 수준 |
| 192     | 0.440 | 0.436 | — |
| 336     | 0.490 | 0.469 | — |
| 720     | 0.492 | 0.487 | — |

출처: Table 1 of paper (OpenReview: zt53IDUR1U). ※ 메모: multivariate 설정 결과; 논문에서 ETTm1에서 두드러진 개선 보고.

## 4. 주요 기여
- Downsampling + Isometric convolution 이중 경로로 local-global 특징 동시 포착.
- Multi-scale Hybrid Decomposition(MHD)으로 비선형 추세·계절 분해.
- 선형 복잡도 O(L)로 긴 시퀀스에 효율적.
- Multivariate 17.2%, Univariate 21.6% 상대 MSE 개선 (당시 기준, 6개 벤치마크 평균).

## 5. 한계 및 후속 연구 아이디어
- 일부 데이터셋에서 PatchTST(Transformer 기반)보다 성능이 낮음.
- Isometric convolution의 padding 방식이 경계 아티팩트를 유발할 수 있음.
- ※ 메모: ModernTCN(ICLR 2024)이 MICN의 아이디어를 확장한 후속 연구로 볼 수 있음.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=zt53IDUR1U
- 공식 구현: https://github.com/wanghq21/MICN
- ※ 원본 PDF 미취득: OpenReview PDF 직접 다운로드 403 응답; 원본 PDF는 https://openreview.net/pdf?id=zt53IDUR1U 에서 수동 취득 필요.
