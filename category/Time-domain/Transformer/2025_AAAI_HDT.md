---
title: "HDT: Hierarchical Discrete Transformer for Multivariate Time Series Forecasting"
authors: Shibo Feng et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2502.08302
code_url: https://github.com/hdtkk/HDT
tags: [time-domain, transformer, generative, discrete-token, vector-quantization, hierarchical, high-dimensional]
primary_category: category/Time-domain/Transformer
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_HDT.pdf
pdf_source: arxiv
---

## TL;DR
HDT는 다변량 시계열을 이산 토큰(discrete tokens)으로 모델링하는 계층적 생성 모델이다. L2 정규화 강화 벡터 양자화로 고차원 MTS를 이산 표현으로 변환하고, 계층 구조로 장기 추세를 조건으로 고해상도 예측을 생성한다.

## 1. 기존 모델의 한계 / 가설
- 고차원 다변량 시계열은 변수 수 확장에 어려움
- 장기 예측 길이 확장이 기존 모델에서 성능 저하 유발
- 연속적 예측값 생성 방식은 복잡한 분포를 캡처하기 어려움
- 가설: 이산 토큰 기반 생성 모델이 고차원 MTS의 복잡한 분포를 더 잘 표현 가능

## 2. 제안 방법론
- **핵심 아이디어**: 벡터 양자화 + 계층적 이산 Transformer로 MTS 예측을 토큰 생성으로 변환
- **아키텍처 구성요소**:
  1. **L2-normalized Vector Quantization (VQ)**: 시계열을 이산 코드북(codebook) 토큰으로 변환; L2 정규화로 VQ 안정성 향상
  2. **Low-level Discrete Component**: 장기 추세 이산 표현 캡처
  3. **High-level Discrete Component**: 장기 추세를 조건으로 목표 이산 표현 생성; 자체 특징 도입으로 예측 길이 확장
  4. **Hierarchical Generation**: 저해상도 → 고해상도 계층적 예측 생성
- **저자**: Shibo Feng, Peilin Zhao, Liu Liu, Pengcheng Wu, Zhiqi Shen

## 3. ETTh1 실험 결과
> 본 논문은 ETTh1 결과를 보고하지 않음.  
> 사용 데이터셋: Solar, Electricity, Traffic, Taxi, Wikipedia (5개 고차원 MTS 데이터셋)  
> `reports_etth1: false`

## 4. 주요 기여
- L2 정규화 강화 벡터 양자화로 이산 시계열 표현
- 계층적 구조로 장기 추세 조건화 및 예측 길이 확장
- Traffic, Taxi 데이터셋에서 기존 SOTA 대비 15.3%, 15.6% NRMSEsum 개선
- 고차원 MTS 예측의 확장성 문제 해결

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 코드북 크기 및 VQ 하이퍼파라미터 민감도
- ※ 메모: Traffic·Electricity는 우리 scope 내 데이터셋이나 ETTh1/Weather/ILI 벤치마크 미사용. 고차원 MTS 특화 모델.

## 6. 참고
- [arXiv 2502.08302](https://arxiv.org/abs/2502.08302)
- [GitHub](https://github.com/hdtkk/HDT)
- 비교: MGTSD, TimesNet 등 고차원 MTS 기반 모델
