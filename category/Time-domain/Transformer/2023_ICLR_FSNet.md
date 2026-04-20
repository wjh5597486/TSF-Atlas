---
title: "Learning Fast and Slow for Online Time Series Forecasting"
authors: Quang Pham et al.
venue: ICLR
year: 2023
paper_url: https://arxiv.org/abs/2202.11672
code_url: https://github.com/salesforce/fsnet
tags: [time-domain, transformer, online-learning, continual-learning, adapter, memory]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2023_ICLR_FSNet.pdf
pdf_source: arxiv
---

## TL;DR
FSNet(Fast and Slow Network)은 온라인(streaming) 시계열 예측에서 갑작스러운 개념 이동(concept drift)과 반복 패턴에 동시에 적응하는 프레임워크다. 보완적 학습 시스템(CLS) 이론에서 영감을 받아, 빠른 적응을 위한 per-layer adapter와 반복 패턴 저장·재활용을 위한 연상 메모리(associative memory)를 결합한다.

## 1. 기존 모델의 한계 / 가설
- 기존 오프라인 TSF 모델은 온라인 환경에서 distribution shift(개념 이동)에 취약함.
- 오프라인 지속 학습(continual learning) 방법은 새로운 패턴 적응과 과거 반복 패턴 기억 사이의 균형이 어려움.
- 가설: fast learner(adapter)로 빠른 적응, slow learner(associative memory)로 장기 반복 패턴 보존하면 online TSF 성능 향상 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Complementary Learning Systems(CLS) 이론 기반 — Fast(adapter) + Slow(associative memory) 이중 구조.
- **Per-layer Adapter**: 각 Transformer 레이어에 경량 adapter 모듈 추가. 최근 데이터로 빠르게 업데이트하여 새 패턴에 적응.
- **Associative Memory**: 과거에 관찰된 중요 반복 패턴을 key-value 쌍으로 저장. 유사 패턴 발생 시 recall하여 adapter 가이드.
- **Online Training**: 스트리밍 데이터를 받을 때마다 adapter + memory를 업데이트.
- **Backbone**: Transformer (기존 pretrained 모델 활용 가능).
- **손실함수**: 표준 forecasting loss (MSE 계열).

## 3. ETTh1 실험 결과
> ETTh2와 ETTm1을 주요 벤치마크로 사용; ETTh1은 직접 보고되지 않음. `reports_etth1: false`

※ 메모: 논문은 ETTh2, ETTm1, Traffic, 합성 데이터셋(S-Gradual, S-Abrupt)을 사용. ETTh1은 실험에 포함되지 않음.

ETTh2 주요 결과 (참고용):
- FSNet MSE=0.846, MAE=0.515 (horizon 48) — DER++ MSE=1.157 대비 27% 개선.

## 4. 주요 기여
- CLS 이론을 온라인 TSF에 최초 적용.
- Per-layer adapter + associative memory로 fast/slow 학습 이분법 구현.
- 갑작스러운 개념 이동과 반복 패턴 모두에서 기존 continual learning 방법 대비 우수.
- 온라인 설정에서 Transformer backbone의 지속적 적응 가능성 실증.

## 5. 한계 및 후속 연구 아이디어
- 오프라인 배치 학습 설정에서는 비교 기준(baseline)이 달라 직접 비교 어려움.
- Associative memory의 크기가 커지면 메모리/계산 비용 증가.
- ※ 메모: 온라인 TSF 특화 논문으로 ETTh1 표준 오프라인 벤치마크와 직접 비교 불가. ETTh2 사용.

## 6. 참고
- arXiv: https://arxiv.org/abs/2202.11672
- OpenReview: https://openreview.net/forum?id=q-PbpHD3EOk
- 공식 구현: https://github.com/salesforce/fsnet
- ※ 원본 PDF 미취득: FSNet은 ETTh1을 직접 보고하지 않음; ETTh2, ETTm1 사용.
