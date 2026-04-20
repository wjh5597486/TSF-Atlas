---
title: "In-context Time Series Predictor"
authors: Jiecheng Lu et al.
venue: ICLR 2025
year: 2025
paper_url: https://openreview.net/forum?id=0Q8KsPrNPT
code_url: https://github.com/LJC-FVNR/In-context-Time-Series-Predictor
tags: [foundation-model, in-context-learning, transformer, few-shot, zero-shot, multivariate]
primary_category: category/Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_ICTSP.pdf
pdf_source: openreview
---

## TL;DR
In-context Time Series Predictor(ICTSP)는 In-Context Learning(ICL) 패러다임을 시계열 예측에 적용한 모델이다. (lookback, future) 쌍을 "시계열 예측 태스크" 토큰으로 변환하여 Transformer에 입력하며, full-data·few-shot·zero-shot 시나리오 모두에서 기존 모델을 능가한다.

## 1. 기존 모델의 한계 / 가설
- 기존 시계열 Transformer는 ICL 설정을 직접 지원하지 않아 few-shot/zero-shot 성능 한계
- 예측 Transformer들의 long-standing 문제(과적합, 정보 누출 등)가 ICL 도입으로 해결 가능하다는 가설
- 다변량 시계열 예측에서 few-shot 일반화 어려움

## 2. 제안 방법론
- 핵심 아이디어: (lookback, future) 쌍을 ICL 토큰으로 만들어 Transformer에 입력 → ICL로 예측
- **Token 설계**: 원본 시계열에서 (룩백, 미래) 쌍 생성, 샘플링 스텝 m 사용
- **Token Retrieval**: q% 유사 토큰 검색(검색 비율 r=30%)으로 관련 문맥 선택
- K=3 Transformer layers, d=128, 8 heads, LI=1440, Lb=512
- 손실함수: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): Lb=512
- Forecast horizons: 96, 192, 336, 720 (LP)
- Normalization: 표준
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE
- 기타: ETTh1, ETTh2, ETTm1, ETTm2, Weather, ECL(Electricity) 평가

### 3.2 Results
ETTh1 포함 6개 표준 벤치마크에서 full-data/few-shot/zero-shot 시나리오 모두 평가. 상세 수치는 논문 참고.

출처: 논문 Table (ICLR 2025 proceedings).

## 4. 주요 기여
- ICL 패러다임을 시계열 예측에 적용한 최초 접근 중 하나
- (lookback, future) 쌍 기반 토큰화로 시계열 예측을 ICL 태스크로 재정의
- Few-shot/Zero-shot 포함 모든 데이터 시나리오에서 SOTA 성능
- Token retrieval로 관련 컨텍스트 선택적 활용

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 컨텍스트 길이와 샘플링 설정에 민감
- 확장 아이디어: LLM과의 결합으로 더 강력한 ICL 시계열 예측기

## 6. 참고
- ICLR 2025 Paper: https://proceedings.iclr.cc/paper_files/paper/2025/file/c699878945bbc2380ea9f421337bea93-Paper-Conference.pdf
- GitHub: https://github.com/LJC-FVNR/In-context-Time-Series-Predictor
- Slides: https://iclr.cc/media/iclr-2025/Slides/28999.pdf
