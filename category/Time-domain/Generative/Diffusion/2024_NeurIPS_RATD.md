---
title: "Retrieval-Augmented Diffusion Models for Time Series Forecasting"
authors: Jingwei Liu et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2410.18712
code_url: https://github.com/stanliu96/RATD
tags: [diffusion, retrieval-augmented, generative, time-domain]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_RATD.pdf
pdf_source: arxiv
---

## TL;DR
RATD(Retrieval-Augmented Time series Diffusion)는 확산 모델의 불안정한 성능과 부족한 가이던스 문제를 해결하기 위해, 유사한 과거 시계열을 데이터베이스에서 검색하고 이를 denoising 프로세스의 가이드로 활용.

## 1. 기존 모델의 한계 / 가설
- 시계열 확산 모델은 성능이 불안정하며, 학습 데이터 부족과 가이던스 부재가 주요 원인.
- 기존 모델은 과거 유사 패턴을 활용하지 않고 오직 현재 입력에만 의존.
- 가설: 유사 시계열 검색으로 가이던스를 제공하면 확산 모델의 품질과 안정성 향상 가능.

## 2. 제안 방법론
- **핵심 아이디어**: 임베딩 기반 검색 + 참조 기반 가이드 확산 모델의 2단계 구조.
- **아키텍처**:
  - Stage 1: 임베딩 기반 검색 — 과거 시계열 임베딩으로 데이터베이스에서 유사 참조 검색
  - Stage 2: Reference-guided Diffusion — 검색된 참조 시계열로 denoising 프로세스 가이드
  - 참조 정보는 cross-attention 또는 조건부 입력으로 주입
- **손실함수**: Diffusion noise prediction loss

## 4. 주요 기여
- 검색 증강(Retrieval-Augmented) 확산 프레임워크 최초 적용 (TSF)
- 임베딩 기반 유사 시계열 검색 모듈
- 복잡한 예측 태스크에서 기존 확산 모델 대비 향상된 성능
- 다양한 시계열 벤치마크에서의 효과성 검증

## 5. 한계 및 후속 연구 아이디어
- 데이터베이스 구축 및 검색 비용으로 추론 지연 증가
- 검색된 참조가 항상 관련성 있다는 보장 없음

## 6. 참고
- [GitHub](https://github.com/stanliu96/RATD)
- [arXiv 2410.18712](https://arxiv.org/abs/2410.18712)

※ ETTh1 결과 보고 여부가 논문 abstract에서 불확실 — `reports_etth1: false`로 설정. 상세 확인 필요.
