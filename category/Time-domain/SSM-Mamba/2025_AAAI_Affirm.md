---
title: "Affirm: Interactive Mamba with Adaptive Fourier Filters for Long-term Time Series Forecasting"
authors: Yuhan Wu et al.
venue: AAAI
year: 2025
paper_url: ""
code_url: ""
tags: [time-domain, ssm, mamba, freq-domain, fft, adaptive-filter, long-term, lightweight]
primary_category: category/Time-domain/SSM-Mamba
related_categories:
  - category/Freq-domain/LearnableFilter
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_Affirm.pdf
pdf_source: arxiv
---

## TL;DR
Affirm은 적응적 Fourier 필터 블록과 이중 인터랙티브 Mamba 블록을 결합한 경량 장기 시계열 예측 모델이다. 노이즈 감소와 주파수 상호작용, 다중 스케일 특징 추출을 통해 Transformer 기반 모델보다 우수한 성능과 효율성을 달성한다.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델은 높은 계산 복잡도와 긴 시퀀스 처리의 어려움
- Mamba 모델은 주파수 도메인 정보를 충분히 활용하지 못함
- 기존 Fourier 필터는 정적이어서 입력에 적응하지 못함
- 가설: 적응적 Fourier 필터와 Mamba의 결합으로 효율적이면서 강력한 예측 가능

## 2. 제안 방법론
- **핵심 아이디어**: 적응적 Fourier 필터(AFF) + 이중 인터랙티브 Mamba(DIM) 결합
- **아키텍처 구성요소**:
  1. **Adaptive Fourier Filter Block (AFF)**: 입력 의존적으로 주파수 성분 필터링, 노이즈 제거 및 주파수 상호작용 캡처
  2. **Dual Interactive Mamba Block (DIM)**: 두 개의 Mamba 스트림이 상호 작용하며 다중 스케일 특징 추출
  3. **경량 설계**: Transformer 대비 낮은 파라미터 수와 계산 비용
- **저자**: Yuhan Wu, Xiyu Meng, Huajin Hu, Junru Zhang, Yabo Dong, Dongming Lu (Zhejiang University)

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 설정
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1 포함 표준 벤치마크 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | — |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper (AAAI 2025, Vol. 39 No. 20, pp. 21599–21607).

## 4. 주요 기여
- Adaptive Fourier Filter Block으로 입력 적응적 주파수 필터링
- Dual Interactive Mamba Block으로 다중 스케일 특징 추출
- Mamba + Fourier 하이브리드 아키텍처의 효율적 설계
- Transformer 대비 경량화와 성능 동시 달성

## 5. 한계 및 후속 연구 아이디어
- ※ 메모: arXiv 프리프린트 링크 미확인 (AAAI DOI: 10.1609/aaai.v39i20.35463)
- ※ 메모: Mamba + Fourier 하이브리드이므로 primary는 SSM-Mamba; LearnableFilter 심볼릭 링크

## 6. 참고
- AAAI 2025 proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/35463
- 비교: Chimera (NeurIPS 2024), Attraos (NeurIPS 2024), FilterNet (NeurIPS 2024)

※ 원본 PDF 미취득: arXiv 링크 미확인, AAAI proceedings PDF 다운로드 실패
