---
title: "TimeMixer: Decomposable Multiscale Mixing for Time Series Forecasting"
authors: Shiyu Wang, Haixu Wu, Xiaoming Shi, Tengge Hu, Huakun Luo, Lintao Ma, James Y. Zhang, Jun Zhou
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2405.14616
code_url: https://github.com/kwuking/TimeMixer
tags: [time-domain, mlp, decomposition, multiscale]
primary_category: category/Decomposition/TrendSeasonal
related_categories:
  - category/Time-domain/MLP-Linear
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_TimeMixer.pdf
pdf_source: arxiv
---

## TL;DR
MLP 기반의 다중 스케일 혼합 구조. **PDM(Past-Decomposable-Mixing)** 으로 과거를 계절·추세로 분해·혼합하고 **FMM(Future-Multipredictor-Mixing)** 으로 여러 예측기를 앙상블.

## 1. 기존 모델의 한계 / 가설
- 단일 스케일 모델은 미세 패턴(계절)과 거시 패턴(추세)을 동시에 잡기 어려움.
- Transformer 없이도 스케일 혼합만으로 충분한 표현력을 얻을 수 있다는 가설.

## 2. 제안 방법론
- **Downsampling**으로 multi-scale 뷰 생성 → 각 스케일별 계절·추세 분해.
- **PDM**: fine→coarse 혼합(계절), coarse→fine 혼합(추세).
- **FMM**: scale별 예측기 출력을 mixer로 결합.

## 3. ETTh1 실험 결과

### 3.1 Setting
- Lookback = **96** (baseline과 동일 조건)
- Horizons = {96, 192, 336, 720}, Avg 보고.

### 3.2 Results (Table 2, ETTh1 Avg)
| Metric | TimeMixer |
|--------|-----------|
| MSE    | 0.447     |
| MAE    | 0.440     |

per-horizon 상세는 원문 Appendix Table 13 참고.

## 4. 주요 기여
- Transformer/Attention 없이 MLP 기반 multiscale mixing으로 long-term/short-term 동시 SOTA.
- PDM·FMM 두 모듈화된 혼합 전략.

## 5. 참고
- 코드: https://github.com/kwuking/TimeMixer
