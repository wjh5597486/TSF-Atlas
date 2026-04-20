---
title: "SMARTformer: Semi-Autoregressive Transformer with Efficient Integrated Window Attention for Long Time Series Forecasting"
authors: Yiduo Li et al.
venue: IJCAI 2023
year: 2023
paper_url: https://www.ijcai.org/proceedings/2023/241
code_url: 
tags: [transformer, semi-autoregressive, window-attention, long-term-forecasting, time-domain]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2023_IJCAI_SMARTformer.pdf
pdf_source: publisher
---

## TL;DR
SMARTformer는 Non-Autoregressive(NAR) 방식의 global horizon 포착 능력과 Autoregressive(AR) 방식의 local detail 학습을 결합한 Semi-Autoregressive Transformer이다. Integrated Window Attention(IWA)으로 효율적인 attention을 달성하며, 5개 벤치마크에서 multivariate 10.2%, univariate 18.4% 성능 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- NAR 디코더: global horizon 포착에 유리하지만 local detail을 놓침.
- AR 디코더: local detail 포착에 유리하지만 global horizon이 제한됨.
- 두 방식의 장점을 결합하는 Semi-Autoregressive 접근이 필요하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: Semi-AutoRegressive (SAR) Decoder로 subsequence를 AR 방식으로 반복 생성하되, 전체 시퀀스를 NAR 방식으로 정제하는 하이브리드 디코딩.
- **아키텍처 구성요소**:
  - **IWA (Integrated Window Attention)**: 윈도우 기반 효율적 어텐션으로 지역/전역 의존성을 동시에 포착
  - **SAR Decoder**: 부분 시퀀스를 AR로 생성 후 전체 시퀀스를 NAR로 정제
  - **TIE (Time-Independent Embedding)**: 위치 임베딩과 값 임베딩 분리로 다양한 주기 혼합 방지
- **손실함수**: MSE
- **학습 전략**: Encoder-Decoder 구조; IWA로 O(n log n) 복잡도

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할 (ETTh1/h2 포함) + 기타 5개 벤치마크
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 5개 벤치마크에서 multivariate 10.2%, univariate 18.4% 성능 향상 보고.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- Semi-Autoregressive Decoder: AR과 NAR의 장점 결합
- Integrated Window Attention: 효율적 지역/전역 의존성 포착
- Time-Independent Embedding: 다양한 주기 혼합 문제 해결
- 5개 벤치마크에서 SOTA 대비 일관적 성능 향상

## 5. 한계 및 후속 연구 아이디어
- SAR의 반복 생성으로 인한 inference 비용 증가 가능성
- ※ 메모: AR-NAR 하이브리드 접근은 iTransformer(채널 역전)과 직교적 아이디어; 결합 가능

## 6. 참고
- 관련 논문: Autoformer (NeurIPS 2021), PatchTST (ICLR 2023)
- IJCAI 2023 proceedings: https://www.ijcai.org/proceedings/2023/241
- PDF: https://www.ijcai.org/proceedings/2023/0241.pdf

※ 원본 PDF 취득: IJCAI 공개 proceedings PDF.
