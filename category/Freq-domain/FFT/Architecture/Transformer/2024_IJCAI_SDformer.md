---
title: "SDformer: Transformer with Spectral Filter and Dynamic Attention for Multivariate Time Series Long-term Forecasting"
authors: Ziyu Zhou et al.
venue: IJCAI 2024
year: 2024
paper_url: https://www.ijcai.org/proceedings/2024/629
code_url: https://github.com/zhouziyu02/SDformer
tags: [transformer, freq-domain, fft, spectral-filter, dynamic-attention, multivariate, long-term-forecasting]
primary_category: category/Freq-domain/FFT/Architecture/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_IJCAI_SDformer.pdf
pdf_source: publisher
---

## TL;DR
SDformer는 Transformer의 self-attention이 많은 변수(variate)를 처리할 때 attention weight이 균등하게 분산되어 row-homogenization 문제가 발생함을 지적하고, Spectral Filter Transform(SFT)와 Dynamic Directional Attention(DDA) 모듈로 이를 해결하는 MTSF 모델이다. IJCAI 2024 Time Series Session 유일한 Long Oral로 선정됨.

## 1. 기존 모델의 한계 / 가설
- Vanilla self-attention은 다수 변수 처리 시 attention weight이 고르게 퍼져(row-homogenization) 핵심 변수 간 상관관계를 포착하지 못함.
- FFT로 노이즈를 제거하면 attention 분포가 더욱 집중되어 유의미한 상관관계 포착이 가능하다는 가설.

## 2. 제안 방법론
- **핵심 아이디어**: FFT 기반 스펙트럼 필터로 시계열을 정제한 후, 정제된 데이터에서 동적 directional attention을 계산하여 중요 변수에 집중.
- **아키텍처 구성요소**:
  - **SFT (Spectral Filter Transform)**: FFT로 주요 주파수 선택 + Hamming Window로 스무딩 및 노이즈 제거
  - **DDA (Dynamic Directional Attention)**: 정제된 query/key에 특수 kernel function 적용 → 예리한 attention 분포 생성
  - 두 모듈의 결합으로 multivariate correlation 포착 강화
- **손실함수**: MSE
- **학습 전략**: 표준 encoder-decoder Transformer 구조

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 상세는 원문 참고
- Forecast horizons: 96, 192, 336, 720
- Normalization: 상세는 원문 참고
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 상세는 원문 Table 참고
- Loss function: MSE

### 3.2 Results
> 상세는 원문 Table 참고. 논문에서는 다수의 공개 데이터셋(ETTh1 포함)에서 SOTA 대비 우수한 성능 보고. IJCAI 2024 Time Series Session Long Oral 수상.

출처: 원문 비교 Table 참고.

## 4. 주요 기여
- Row-homogenization 문제를 Transformer-based MTSF에서 실험적으로 규명
- SFT: FFT 기반 스펙트럼 필터링으로 노이즈 제거
- DDA: Kernel function 기반 동적 directional attention으로 예리한 attention 분포 달성
- IJCAI 2024 Time Series Session 유일한 Long Oral 발표

## 5. 한계 및 후속 연구 아이디어
- FFT 기반 필터링은 비정상 시계열에서 성능 저하 가능성
- ※ 메모: FEDformer, Fredformer와 유사한 주파수 도메인 attention 패밀리; 비교 실험 의미 있음

## 6. 참고
- 관련 논문: FEDformer (ICML 2022), Fredformer (KDD 2024), iTransformer (ICLR 2024)
- IJCAI 2024 proceedings: https://www.ijcai.org/proceedings/2024/629
- GitHub: https://github.com/zhouziyu02/SDformer

※ 원본 PDF 미취득: arXiv 프리프린트 없음; IJCAI 공개 PDF https://www.ijcai.org/proceedings/2024/0629.pdf 이용 가능.
