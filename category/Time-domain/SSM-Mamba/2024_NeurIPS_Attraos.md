---
title: "Attractor Memory for Long-Term Time Series Forecasting: A Chaos Perspective"
authors: Jiaxi Hu et al.
venue: NeurIPS 2024
year: 2024
paper_url: https://arxiv.org/abs/2402.11463
code_url: https://github.com/CityMind-Lab/NeurIPS24-Attraos
tags: [ssm, chaos, attractor, time-domain, memory, phase-space]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_NeurIPS_Attraos.pdf
pdf_source: arxiv
---

## TL;DR
Attraos는 혼돈 이론(chaos theory)을 LTSF에 접목하여, 실세계 시계열을 고차원 카오스 동역학계의 저차원 관측으로 해석. 위상 공간 재구성(Phase Space Reconstruction) 임베딩 + 다해상도 동적 메모리 유닛으로 어트랙터 구조를 포착.

## 1. 기존 모델의 한계 / 가설
- 기존 LTSF 모델은 시계열의 카오스적 본질을 무시하고 단순 패턴 매칭에 의존.
- Transformer/MLP 기반 모델은 고차원 동역학 구조를 명시적으로 학습하지 않아 카오스 데이터에 취약.
- 가설: 실세계 시계열 = 미지의 고차원 카오스 시스템의 저차원 관측 → 어트랙터 불변성 원리로 더 정확한 예측 가능.

## 2. 제안 방법론
- **핵심 아이디어**: Phase Space Reconstruction(PSR) 임베딩 + Multi-resolution Dynamic Memory Unit(MDMU)으로 카오스 어트랙터 구조 메모리화.
- **아키텍처**:
  - 비파라미터적 PSR 임베딩: 타임지연 임베딩으로 저차원 관측 → 위상 공간으로 복원
  - MDMU: 직교 다항식 부분 공간(SSM 확장)에서 다해상도 동역학 구조 메모리화
  - 주파수 강화 로컬 진화 전략(Frequency-enhanced Local Evolution)
- **손실함수**: MSE

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 720
- Forecast horizons: 96, 192, 336, 720
- Normalization: 표준 설정
- Train/Val/Test split: 표준 ETT 분할
- Loss function: MSE

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 상세는 원문 Table 참고 | — | PatchTST 파라미터의 1/12로 우수한 성능 |
| 192     | 상세는 원문 Table 참고 | — | — |
| 336     | 상세는 원문 Table 참고 | — | — |
| 720     | 상세는 원문 Table 참고 | — | — |

출처: Table of paper.

## 4. 주요 기여
- 카오스 이론을 LTSF에 최초 적용하는 프레임워크
- 비파라미터적 PSR 임베딩으로 위상 공간 복원
- MDMU로 다양한 어트랙터 패턴 메모리화
- 일반 LTSF 벤치마크 + 카오스 데이터셋 모두에서 SOTA

## 5. 한계 및 후속 연구 아이디어
- 위상 공간 차원 및 시간지연 하이퍼파라미터 선택에 민감
- 실제 카오스 차원 추정이 어려운 경우 PSR 품질 저하 가능

## 6. 참고
- [GitHub](https://github.com/CityMind-Lab/NeurIPS24-Attraos)
- [arXiv 2402.11463](https://arxiv.org/abs/2402.11463)
