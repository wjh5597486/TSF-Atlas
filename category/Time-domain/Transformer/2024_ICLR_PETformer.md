---
title: "PETformer: Long-term Time Series Forecasting with Placeholder-enhanced Transformer"
authors: Shengsheng Lin, Weiwei Lin, Xinyi Hu, Wentai Wu, Ruichao Mo, Haocheng Zhong
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2308.04791
code_url: ""
tags: [time-domain, transformer, patching, placeholder]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/CrossCutting/Patching
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_PETformer.pdf
pdf_source: arxiv
---

## TL;DR
과거 토큰 뒤에 **학습 가능한 placeholder 토큰**을 concatenate해 미래 구간의 표현을 직접 학습. 별도 decoder 없이 encoder-only Transformer로 long-horizon 예측.

## 1. 기존 모델의 한계 / 가설
- Encoder-decoder 구조는 학습·추론 복잡도가 크고 정보 누락 위험.
- 미래를 "placeholder"로 표현하고 self-attention에 맡기면 단순·효과적.

## 2. 제안 방법론
- Input patch tokens ‖ Learnable placeholder tokens → Transformer encoder.
- **Long Sub-sequence Division (LSD)** + **Multi-channel Separation & Interaction (MSI)** 전략.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Lookback/horizon: 표준 {96/96~720}.

### 3.2 Results
ETTh1/ETTm1/Weather에서 PatchTST와 경쟁 수준. 상세는 원문 Table 참고.

## 4. 주요 기여
- Decoder-free, placeholder-based long-term TSF.
- 패치 분할 + 다채널 상호작용 조합.

## 5. 참고
- arXiv: https://arxiv.org/abs/2308.04791
