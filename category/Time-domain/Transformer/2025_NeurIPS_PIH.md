---
title: "Enhancing the Maximum Effective Window for Long-Term Time Series Forecasting"
authors: Jiahui Zhang et al.
venue: NeurIPS
year: 2025
paper_url: https://openreview.net/forum?id=Gmwsy7TlFI
code_url: https://github.com/forever-ly/PIH
tags: [time-domain, transformer, ssm-mamba, hybrid, information-bottleneck, lookback-window]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_PIH.pdf
pdf_source: openreview
---

## TL;DR
PIH(Plug-In Hybrid)는 Maximum Effective Window(MEW) 메트릭을 도입하여 모델이 lookback 윈도우를 효과적으로 활용하는 능력을 정량화하고, Information Bottleneck Filter(IBF)와 Hybrid-Transformer-Mamba(HTM) 두 모델 비종속 모듈을 통해 MEW를 향상시키는 방법을 제안한다.

## 1. 기존 모델의 한계 / 가설
- 더 긴 입력 윈도우는 노이즈와 중복 정보를 증가시켜 오히려 예측 성능을 저하시킬 수 있음.
- 기존 모델들은 긴 lookback 윈도우를 효과적으로 활용하는 최대 유효 윈도우(MEW)가 제한적.
- 가설: 정보 병목 이론으로 핵심 정보만 추출하고, Mamba의 선택적 망각으로 긴 시퀀스를 처리하면 MEW를 향상시킬 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: MEW 메트릭 도입 + 두 모델 비종속 모듈(IBF+HTM)로 MEW 향상.
- **아키텍처**:
  - **MEW(Maximum Effective Window)**: 모델이 lookback 윈도우를 얼마나 효과적으로 활용하는지 측정하는 새로운 메트릭.
  - **IBF(Information Bottleneck Filter)**: 정보 병목 이론으로 입력에서 가장 필수적인 서브시퀀스 추출.
  - **HTM(Hybrid-Transformer-Mamba)**: Mamba의 선택적 망각 메커니즘 + Transformer의 강력한 모델링 능력을 결합. 긴 시퀀스는 Mamba, 짧은 시퀀스는 Transformer로 처리.
- **특성**: Model-Agnostic 플러그인 방식으로 기존 예측기에 추가 가능.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 긴 lookback 윈도우(논문 참조)
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 인스턴스 정규화
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm, Electricity, Weather, Traffic 평가

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | see paper |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 구체적 수치는 논문 참조 필요.

## 4. 주요 기여
- Maximum Effective Window(MEW) 메트릭 최초 제안.
- Information Bottleneck Filter(IBF)로 핵심 서브시퀀스 추출.
- Hybrid-Transformer-Mamba(HTM)로 선택적 시퀀스 처리.
- Model-Agnostic 플러그인 방식.

## 5. 한계 및 후속 연구 아이디어
- IBF의 정보 병목 임계값 설정 하이퍼파라미터 민감도.
- Mamba-Transformer 분기 기준의 동적 설정 방안.

## 6. 참고
- OpenReview: https://openreview.net/forum?id=Gmwsy7TlFI
- GitHub: https://github.com/forever-ly/PIH
※ 원본 PDF 미취득: arXiv ID 없음. OpenReview에서 다운로드 필요.
