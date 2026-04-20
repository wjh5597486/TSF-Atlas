---
title: "MOMENT: A Family of Open Time-series Foundation Models"
authors: Mononito Goswami et al.
venue: ICML 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.03885
code_url: https://github.com/moment-timeseries-foundation-model/moment
tags: [foundation-model, pretrained, masked-reconstruction, transformer, time-domain]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2024_ICML_MOMENT.pdf
pdf_source: arxiv
---

## TL;DR
MOMENT는 CMU가 개발한 오픈소스 시계열 파운데이션 모델 패밀리. Time-series Pile이라는 대규모 공개 데이터셋으로 마스킹 재구성 방식 사전학습. 예측, 분류, 이상 탐지, 보간 등 다중 태스크 지원.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 파운데이션 모델은 독점 데이터나 제한된 태스크에 의존.
- 오픈소스 대규모 시계열 파운데이션 모델 부재로 연구 재현성 문제.
- 가설: 대규모 공개 시계열 데이터셋(Time-series Pile)으로 마스킹 사전학습 시 범용 표현 학습 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Time-series Pile 구축 + 마스킹 재구성 사전학습으로 범용 TSF 표현 학습.
- **아키텍처**:
  - T5-기반 Transformer 인코더 (MOMENT-Small/Base/Large)
  - 마스킹 패치 재구성 사전학습
  - 다양한 길이/주파수 시계열 처리
  - 다중 태스크: 예측, 분류, 이상탐지, 보간
- **손실함수**: Masked reconstruction MSE (사전학습)

## 4. 주요 기여
- Time-series Pile: 대규모 오픈 시계열 사전학습 데이터셋 공개
- 오픈소스 TSF 파운데이션 모델 패밀리 (Small/Base/Large)
- 다중 다운스트림 태스크 지원
- 재현 가능한 연구를 위한 완전 오픈소스 생태계

## 5. 한계 및 후속 연구 아이디어
- 사전학습 데이터의 품질 편차 문제
- 예측에서 zero-shot 성능이 완전 파인튜닝 대비 제한적

## 6. 참고
- [GitHub](https://github.com/moment-timeseries-foundation-model/moment)
- [arXiv 2402.03885](https://arxiv.org/abs/2402.03885)

※ 원문에서 ETTh1 예측 결과 명시 여부 불확실 - 주로 다중 태스크 평가 결과 보고. `reports_etth1: false`로 설정.
