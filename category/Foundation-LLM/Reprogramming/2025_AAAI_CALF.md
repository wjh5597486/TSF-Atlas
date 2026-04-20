---
title: "CALF: Aligning LLMs for Time Series Forecasting via Cross-modal Fine-Tuning"
authors: Peiyuan Liu et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2403.07300
code_url: https://github.com/Hank0626/CALF
tags: [foundation-llm, reprogramming, cross-modal, fine-tuning, long-term, short-term]
primary_category: category/Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_CALF.pdf
pdf_source: arxiv
---

## TL;DR
CALF는 시계열 데이터와 텍스트 기반 LLM 간의 분포 불일치를 cross-modal fine-tuning으로 해결한다. 시간적 브랜치와 텍스트 브랜치의 이중 구조, Cross-Modal Match Module, Feature Regularization Loss, Output Consistency Loss로 SOTA 장·단기 예측을 달성한다.

## 1. 기존 모델의 한계 / 가설
- LLM은 텍스트 데이터로 사전 학습되어 시계열 입력 토큰과 분포 불일치 발생
- 기존 LLM 기반 TSF 모델(GPT4TS 등)은 시계열-텍스트 modality 간 정렬이 불충분
- 가설: 시간적 입력과 정렬된 텍스트 입력을 함께 학습하면 LLM의 시계열 이해 능력 향상

## 2. 제안 방법론
- **핵심 아이디어**: 시간 브랜치 + 텍스트 브랜치 이중 구조 + cross-modal alignment
- **백본**: GPT-2 (6개 Transformer 레이어)
- **아키텍처 구성요소**:
  1. **Temporal Target Branch**: 시계열 입력 직접 처리
  2. **Textual Source Branch**: PCA 기반 단어 임베딩으로 정렬된 텍스트 입력 처리
  3. **Cross-Modal Match Module (CMM)**: 두 브랜치의 입력 분포를 cross-attention으로 정렬
  4. **Feature Regularization Loss**: 중간 레이어 특징 정렬
  5. **Output Consistency Loss**: 출력 표현 일관성 보장
- **효율화**: LoRA (Low-rank Adaptation)로 시간 브랜치 파인튜닝

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 512
- Forecast horizons: {96, 192, 336, 720}
- Normalization: 표준 정규화
- Train/Val/Test split: 표준 ETT 분할
- Batch size / Optimizer / LR: 논문 참조
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, ECL, Traffic (장기); M4 (단기)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| avg     | 0.432 | 0.428 | PatchTST (0.450/0.441) 대비 우수 |
| 96      | see Table 1 | see Table 1 | — |
| 192     | see Table 1 | see Table 1 | — |
| 336     | see Table 1 | see Table 1 | — |
| 720     | see Table 1 | see Table 1 | — |

출처: Table 1 of paper.

## 4. 주요 기여
- Cross-modal match module로 시계열-텍스트 분포 불일치 해결
- Feature regularization + output consistency loss로 중간/출력 레이어 정렬
- LoRA 기반 효율적 파인튜닝으로 낮은 계산 복잡도
- 장기 및 단기 예측 모두에서 SOTA 달성
- Few-shot 및 zero-shot 능력 보유

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 더 큰 LLM (GPT-3, LLaMA)으로 확장 시 추가 비용 필요
- ※ 메모: 정렬 방식이 TimeCMA와 유사하나 cross-modal fine-tuning 방향성이 다름

## 6. 참고
- [arXiv 2403.07300](https://arxiv.org/abs/2403.07300)
- [GitHub](https://github.com/Hank0626/CALF)
- 비교: GPT4TS, Time-LLM, TEMPO
