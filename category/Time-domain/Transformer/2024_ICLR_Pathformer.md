---
title: "Pathformer: Multi-scale Transformers with Adaptive Pathways for Time Series Forecasting"
authors: Peng Chen, Yingying Zhang, Yunyao Cheng, Yang Shu, Yihang Wang, Qingsong Wen, Bin Yang, Chenjuan Guo
venue: ICLR
year: 2024
paper_url: https://arxiv.org/abs/2402.05956
code_url: https://github.com/decisionintelligence/pathformer
tags: [time-domain, transformer, multiscale, adaptive-routing]
primary_category: category/Time-domain/Transformer
related_categories:
  - category/Decomposition/MultiScale
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_ICLR_Pathformer.pdf
pdf_source: arxiv
---

## TL;DR
다중 해상도 분할과 **입력에 따라 동적으로 경로를 선택하는 multi-scale Transformer**. Temporal dynamics에 맞춰 adaptive pathway가 켜지고 꺼진다.

## 1. 기존 모델의 한계 / 가설
- 고정된 해상도/패치 크기로는 시계열마다 다른 temporal dynamics에 적응하기 어렵다.

## 2. 제안 방법론
- **Multi-scale division**: 여러 patch size로 동일 입력을 분할.
- **Adaptive pathways**: 입력 특성 기반 router가 어떤 scale/path를 활성화할지 결정.
- 각 path는 독립 Transformer 블록.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Lookback 96 기준. Horizons = {96, 192, 336, 720}.

### 3.2 Results
상세 수치는 원문 Table 1 참고 (PatchTST·iTransformer와 비교 우위 보고).

## 4. 주요 기여
- 고정 scale의 한계를 동적 routing으로 해결.
- multi-scale + adaptive = 다양한 데이터셋에 일반화.

## 5. 참고
- 코드: https://github.com/decisionintelligence/pathformer
