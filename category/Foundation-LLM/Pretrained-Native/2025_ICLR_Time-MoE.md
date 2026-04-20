---
title: "Time-MoE: Billion-Scale Time Series Foundation Models with Mixture of Experts"
authors: Xiaoming Shi et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2409.16040
code_url: https://github.com/Time-MoE/Time-MoE
tags: [foundation-model, pretrained, transformer, decoder-only, mixture-of-experts, zero-shot, autoregressive]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_Time-MoE.pdf
pdf_source: arxiv
---

## TL;DR
Time-MoE는 Sparse Mixture-of-Experts(MoE) 구조를 적용한 decoder-only 시계열 파운데이션 모델로, 2.4B 파라미터까지 스케일업하였다. Time-300B(300B 포인트 이상)로 사전학습되어 임의 예측 horizon·context length에 대응하며, ICLR 2025 Spotlight로 발표되었다.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 파운데이션 모델(Timer, MOIRAI 등)은 모델 크기가 제한적이어서 LLM 수준의 스케일링 이점을 누리지 못함
- Dense 모델은 파라미터가 늘어날수록 계산 비용이 비례하여 증가
- 시계열 스케일링 법칙(scaling law)이 존재하는지 불분명

## 2. 제안 방법론
- 핵심 아이디어: Sparse MoE를 시계열 transformer에 적용하여 활성화된 파라미터 수만큼만 계산 → 효율적 대규모 스케일링
- **아키텍처**: Decoder-only transformer + token-level routing sparse MoE
- **Time-300B 데이터셋**: 9개 도메인, 300B 이상 타임포인트의 사전학습 데이터
- **Context length**: 최대 4096 스텝 지원, 임의 예측 horizon
- 손실함수: Autoregressive next-token prediction (MSE)
- 스케일: Time-MoE-Base(50M 활성), Time-MoE-Large(200M 활성), Time-MoE-Ultra(2.4B 전체)

## 3. ETTh1 실험 결과
> 본 논문은 ETTh1 표준 supervised 벤치마크보다 zero-shot 예측 평가(ETTm1/ETTm2 등)에 초점. ETTh1 supervised LTSF 결과를 보고하지 않으므로 `reports_etth1: false`.

※ 메모: zero-shot 세팅에서 ETTm1/ETTm2, M4, Monash 등 다양한 벤치마크에서 평가. ETTh1에 대한 직접 supervised 결과는 논문에 없음.

## 4. 주요 기여
- 시계열 파운데이션 모델 최초 2.4B 파라미터 스케일업
- Sparse MoE를 통한 Dense 모델 대비 동일 계산 예산에서 일관된 성능 우위
- 시계열에서도 학습 토큰·모델 크기에 대한 스케일링 법칙 실증
- zero-shot 일반화 성능 SOTA

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: pre-training 데이터 도메인 편향 가능성
- 확장 아이디어: 특정 도메인(ETT, 전력 등)에 대한 파인튜닝으로 supervised LTSF SOTA 경쟁 가능성

## 6. 참고
- arXiv: https://arxiv.org/abs/2409.16040
- OpenReview: https://openreview.net/forum?id=e1wDDFmlVu
- ICLR 2025 Spotlight: https://iclr.cc/virtual/2025/poster/28954
- GitHub: https://github.com/Time-MoE/Time-MoE
