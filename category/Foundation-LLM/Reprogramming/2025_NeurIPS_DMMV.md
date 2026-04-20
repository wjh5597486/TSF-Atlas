---
title: "Multi-Modal View Enhanced Large Vision Models for Long-Term Time Series Forecasting"
authors: ChengAo Shen et al.
venue: NeurIPS
year: 2025
paper_url: https://arxiv.org/abs/2505.24003
code_url: https://github.com/D2I-Group/dmmv
tags: [foundation-llm, reprogramming, multi-modal, vision-model, decomposition, trend-seasonal]
primary_category: category/Foundation-LLM/Reprogramming
related_categories:
  - category/Decomposition/TrendSeasonal
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_NeurIPS_DMMV.pdf
pdf_source: arxiv
---

## TL;DR
DMMV는 시계열 데이터를 수치 시퀀스·이미지·텍스트의 세 가지 멀티모달 형태로 변환하여, 사전학습된 대형 비전 모델(LVM)의 강력한 표현력을 LTSF에 활용한다. 추세-계절 분해와 새로운 backcast-residual 기반 적응형 분해를 통해 8개 벤치마크 중 6개에서 최고 MSE를 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 TSF 모델은 사전학습된 대형 비전/언어 모델(LVM/LLM)의 다중 뷰 표현 능력을 충분히 활용하지 못함.
- 단일 뷰(수치 시퀀스만)로는 시계열의 다양한 시각적·텍스트적 패턴을 포착하기 어려움.
- 가설: 시계열을 멀티모달 뷰(수치+이미지+텍스트)로 변환하면 LVM의 강력한 사전학습 지식을 효과적으로 활용할 수 있다.

## 2. 제안 방법론
- **핵심 아이디어**: 시계열을 수치 시퀀스·이미지·텍스트로 변환하여 LVM으로 처리.
- **아키텍처(DMMV)**:
  - **Decomposition-based Multi-Modal View (DMMV)**: 추세-계절 분해 + backcast-residual 기반 적응형 분해로 멀티모달 뷰 생성.
  - **수치 뷰**: 표준 시계열 임베딩.
  - **이미지 뷰**: 시계열을 선 그래프 이미지로 변환. LVM의 비전 인코더 활용.
  - **텍스트 뷰**: 시계열 패턴을 텍스트로 변환. LVM의 언어 이해 활용.
  - 세 뷰를 통합하여 최종 예측.
- **손실함수**: MSE 손실.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 LTSF 설정
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 인스턴스 정규화
- Train/Val/Test split: 표준 ETT split
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, Electricity, Weather, Traffic 등 8개 데이터셋, 14개 SOTA 비교

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     | 8개 중 6개 데이터셋에서 최고 MSE |
| 192     |     |     | see paper |
| 336     |     |     | see paper |
| 720     |     |     | see paper |

출처: Table of paper. ※ 메모: 8개 벤치마크 중 6개에서 최고 MSE 달성(14개 SOTA 비교).

## 4. 주요 기여
- 시계열을 수치·이미지·텍스트 멀티모달 뷰로 변환하는 DMMV 프레임워크 제안.
- Backcast-residual 기반 적응형 분해 모듈 신설.
- 대형 비전 모델(LVM) 활용으로 시계열 예측 성능 향상.
- 8개 중 6개 벤치마크에서 최고 MSE(14개 SOTA 대비).

## 5. 한계 및 후속 연구 아이디어
- 멀티모달 변환 비용(이미지 렌더링 등) 추가 발생.
- 변환 품질이 LVM의 사전학습 도메인에 의존.

## 6. 참고
- arxiv: https://arxiv.org/abs/2505.24003
- GitHub: https://github.com/D2I-Group/dmmv
- OpenReview: https://openreview.net/forum?id=PMdHrorFMF
