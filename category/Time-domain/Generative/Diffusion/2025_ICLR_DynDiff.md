---
title: "Dynamical Diffusion: Learning Temporal Dynamics with Diffusion Models"
authors: (authors, thuml group)
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2503.00951
code_url: https://github.com/thuml/dynamical-diffusion
tags: [diffusion, generative, temporal-dynamics, time-domain, probabilistic, spatiotemporal]
primary_category: category/Time-domain/Generative/Diffusion
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_DynDiff.pdf
pdf_source: arxiv
---

## TL;DR
Dynamical Diffusion(DyDiff)은 시간 인식 forward/reverse 프로세스를 통해 각 diffusion 스텝에서 시간적 전이(temporal transition)를 명시적으로 모델링하는 확산 모델 프레임워크이다. 이전 상태 의존성을 reparameterization trick으로 효율적 훈련·추론에 통합하여 시계열·비디오 예측에서 일관된 성능 향상을 달성한다.

## 1. 기존 모델의 한계 / 가설
- 기존 확산 모델 기반 예측은 예측 학습을 조건부 생성 문제로 다루지만 데이터 내 시간적 역학(temporal dynamics)을 충분히 활용 못함
- 시간적으로 일관된(coherent) 시퀀스 생성 어려움
- 각 diffusion 스텝이 시간적 전이를 모델링해야 더 나은 예측 가능하다는 가설

## 2. 제안 방법론
- 핵심 아이디어: Temporally-aware forward/reverse process로 diffusion 각 스텝에서 이전 상태 의존성 명시적 모델링
- **Temporally-aware forward process**: 이전 상태에 의존하는 노이즈 추가
- **Temporally-aware reverse process**: 시간적 전이를 역전파에 통합
- **Reparameterization trick**: 표준 diffusion 훈련·추론과 동등한 효율성 유지
- 과학적 시공간 예측, 비디오 예측, 시계열 예측에 적용
- 손실함수: Diffusion 표준 denoising 목표

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 표준 LTSF 설정
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Loss function: Diffusion denoising loss
- 기타: 시계열 예측 외 Turbulence, SEVIR(비디오) 등 다양한 시공간 태스크도 평가

### 3.2 Results
Turbulence 데이터셋에서 CRPS 12% 이상 감소. 시계열 예측 벤치마크(ETTh1 포함)에서 기존 확산 모델 대비 일관된 향상. 상세 수치는 논문 참고.

출처: 논문 Table (ICLR 2025, thuml/dynamical-diffusion).

## 4. 주요 기여
- Temporally-aware forward/reverse diffusion process 제안
- 각 diffusion 스텝에서 시간적 의존성 명시적 모델링
- reparameterization으로 표준 diffusion과 동등한 훈련·추론 효율
- 시계열·비디오·시공간 예측 등 다양한 시간적 예측 태스크에서 일관된 향상

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 확산 모델 특유의 느린 샘플링 속도
- 확장 아이디어: diffusion 스텝 수 감소(consistency model 등)와 결합

## 6. 참고
- arXiv: https://arxiv.org/abs/2503.00951
- OpenReview: https://openreview.net/forum?id=c5JZEPyFUE
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/29071
- GitHub: https://github.com/thuml/dynamical-diffusion

※ 원본 PDF 미취득: arXiv 버전(2503.00951)이 2025년 3월 제출로 ICLR 2025 최종본과 동일 여부 확인 필요.
