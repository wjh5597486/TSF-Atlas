---
title: "A Time Series is Worth 64 Words: Long-term Forecasting with Transformers"
authors: Yuqi Nie et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2211.14730
code_url: https://github.com/yuqinie98/PatchTST
tags: [time-domain, transformer, patching, channel-independent]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Patching
  - category/CrossCutting/ChannelStrategy/ChannelIndependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_PatchTST.pdf
pdf_source: arxiv
---

## TL;DR
PatchTST는 시계열을 패치(subseries 단위 세그먼트) 토큰으로 분할하여 Transformer에 입력하고, 채널 독립(CI) 전략으로 각 변수를 독립적으로 처리함으로써 긴 컨텍스트를 효율적으로 활용한다. Transformer 기반 모델 중 최초로 DLinear 등 단순 선형 모델을 능가하는 성능을 보였으며, 자기지도 사전학습에서도 강력한 표현 학습 능력을 입증했다.

## 1. 기존 모델의 한계 / 가설
- 기존 Transformer 기반 TSF 모델(Informer, Autoformer, FEDformer 등)은 point-wise attention으로 인해 시간적 지역 의미(local semantic)를 잃어버리고, 채널 간 혼합으로 노이즈가 증폭됨.
- 입력 길이 증가 시 토큰 수가 선형 비례하여 메모리/계산 비용이 폭증함.
- DLinear 등 단순 선형 모델에도 성능이 뒤지는 문제가 지적됨.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 overlapping patch 토큰으로 분할하고, 채널 독립(CI)으로 각 변수를 별도 시퀀스로 처리.
- **Patching**: 시계열을 patch length P, stride S로 분할. 토큰 수 = ⌈(L - P)/S⌉ + 2. 기본값 P=16, S=8.
- **Channel Independence (CI)**: 모든 채널이 동일한 Transformer 가중치를 공유하여 과적합 방지 및 일반화 향상.
- **Transformer backbone**: 표준 Transformer encoder (Multi-head self-attention + FFN).
- **자기지도 사전학습(PatchTST/SSL)**: 마스크된 패치 복원을 pretext task로 사용, 다운스트림 forecasting에 fine-tuning.
- **손실함수**: MSE (supervised); 마스크 패치 복원 MSE (SSL).

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (lookback window L=336)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: RevIN (Reversible Instance Normalization) 적용
- Train/Val/Test split: 표준 ETT split (12/4/4 months)
- Batch size / Optimizer / LR: batch size 128, Adam optimizer, lr 0.0001
- Loss function: MSE
- 기타: patch length P=16, stride S=8, d_model=128, n_heads=16, e_layers=3, dropout=0.2

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 |
|---------|-------|-------|----------------|
| 96      | 0.370 | 0.400 | FEDformer 0.376/0.419 대비 개선 |
| 192     | 0.413 | 0.429 | FEDformer 0.420/0.448 대비 개선 |
| 336     | 0.422 | 0.440 | FEDformer 0.459/0.465 대비 개선 |
| 720     | 0.447 | 0.468 | FEDformer 0.454/0.492 대비 개선 |

출처: Table 1 of paper (arXiv:2211.14730).

## 4. 주요 기여
- 패치 기반 토큰화로 토큰 수를 최대 수십 배 절감하면서 더 긴 컨텍스트 활용 가능.
- 채널 독립(CI) 전략이 채널 혼합(CM)보다 ETT 등 다수 벤치마크에서 우수함을 실증.
- Transformer 기반 모델 최초로 DLinear를 넘는 장기 예측 성능 달성.
- 자기지도 사전학습(PatchTST/SSL)으로 레이블 없는 시계열에서도 강력한 표현 학습.
- 이후 iTransformer, PETformer, TimeXer 등 다수 후속 연구에 큰 영향.

## 5. 한계 및 후속 연구 아이디어
- 채널 독립 전략이 채널 간 상관관계가 강한 데이터셋(Traffic 등)에서는 채널 혼합 방식에 뒤질 수 있음 (저자 명시).
- 패치 크기 및 stride가 하이퍼파라미터로, 도메인마다 튜닝이 필요함.
- ※ 메모: iTransformer는 CI의 반대 방향 (채널 혼합, 역전된 attention)으로 보완적 접근; 두 모델의 조합 연구 가능.

## 6. 참고
- arXiv: https://arxiv.org/abs/2211.14730
- OpenReview: https://openreview.net/forum?id=Jbdc0vTOcol
- 공식 구현: https://github.com/yuqinie98/PatchTST
- IBM PatchTST (self-supervised): https://github.com/ibm-research/patchtst-etth1-pretrain
