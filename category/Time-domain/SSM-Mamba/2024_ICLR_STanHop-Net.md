---
title: "STanHop: Sparse Tandem Hopfield Model for Memory-Enhanced Time Series Prediction"
authors: Dennis Wu, Jerry Yao-Chieh Hu, Weijian Li, Bo-Yu Chen, Han Liu
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2312.17346
code_url: https://github.com/MAGICS-LAB/STanHop
tags: [hopfield, memory-network, sparse]
primary_category: category/Time-domain/SSM-Mamba
related_categories:
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_STanHop-Net.pdf
pdf_source: arxiv
---

## TL;DR
Modern Hopfield Network의 sparse 변형을 이중(tandem) 구조로 쌓은 memory-augmented TSF 모델. **TimeGSH**(시간축)와 **SeriesGSH**(변수축) 두 Hopfield 레이어로 패턴을 저장·회상.

## 1. 기존 모델의 한계 / 가설
- Self-attention/RNN은 과거 패턴을 명시적으로 "저장"하기 어려움.
- Sparse Hopfield는 대용량 패턴 메모리를 지원하므로 TSF에 유리하다는 가설.

## 2. 제안 방법론
- **TimeGSH**: 시간축 sparse Hopfield 저장.
- **SeriesGSH**: 변수축 sparse Hopfield 저장.
- Encoder-decoder 구조에 tandem으로 결합.

## 3. ETTh1 실험 결과
### 3.1 Setting
- ETTh1, ETTm1, Weather, ILI 등 standard benchmarks.
- Noise injection 실험 포함 (ETTh1 + horizon 168/336 등).

### 3.2 Results
표준 long-term forecasting에서 경쟁적, noisy 조건에서 강건성 우위. 상세는 원문 Table 참고.

## 4. 주요 기여
- Sparse Hopfield의 TSF 확장.
- Noisy 시계열에서의 memory-based 강건성.

## 5. 참고
- 코드: https://github.com/MAGICS-LAB/STanHop
