---
title: "Fast and Slow Streams for Online Time Series Forecasting Without Information Leakage"
authors: Ying-yee Ava Lau et al.
venue: ICLR 2025
year: 2025
paper_url: https://openreview.net/forum?id=I0n3EyogMi
code_url: https://github.com/yyalau/iclr2025_dsof
tags: [time-domain, online-forecasting, teacher-student, dual-stream, temporal-difference, residual]
primary_category: category/Time-domain/MLP-Linear
related_categories: []
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_DSOF.pdf
pdf_source: arxiv
---

## TL;DR
DSOF(Dual-Stream Online Forecasting)는 온라인 시계열 예측에서 정보 누출(information leakage) 문제를 해결하는 이중 스트림 프레임워크이다. Slow stream(경험 재생 기반 완전 데이터 갱신)과 fast stream(시간차 학습 기반 최신 데이터 적응)을 결합하여 교사-학생 모델로 coarse-to-fine 잔차 예측을 수행한다.

## 1. 기존 모델의 한계 / 가설
- 기존 온라인 예측 방법들이 **정보 누출(information leakage)** 문제를 간과: 예측 후 이미 역전파에 사용된 과거 시간 스텝에 대해 재평가하는 문제
- 완전한 데이터 갱신(slow)과 빠른 최신 적응(fast)을 동시에 처리하기 어려움
- Temporal difference learning이 온라인 시계열 적응에 효과적이라는 가설

## 2. 제안 방법론
- 핵심 아이디어: Slow+Fast 이중 스트림으로 완전 갱신과 빠른 적응을 분리
- **Slow stream**: Experience replay로 완전 데이터 배치에서 모델 업데이트
- **Fast stream**: Temporal difference learning으로 최신 데이터 빠른 적응
- **Teacher-Student**: Slow 스트림이 teacher, Fast 스트림이 student
- **Coarse-to-fine residual**: teacher 예측 + student 잔차 보정으로 최종 예측
- 손실함수: MSE + TD 학습 목표

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 온라인 설정 (슬라이딩 윈도우)
- Forecast horizons: 온라인 예측 지평
- Normalization: 표준
- Train/Val/Test split: 온라인 시나리오 (순차 평가)
- Loss function: MSE
- 기타: ECL(Electricity), ETTh2, ETTm1 포함; ECL에서 prediction length 48에서 MSE 최소 15% 감소

### 3.2 Results
ECL 예측 길이 48에서 기준 모델 대비 MSE 15% 이상 감소. ETTh 포함 여러 벤치마크에서 상위 2위 이내 일관된 성능. 상세 수치는 논문 참고.

출처: 논문 Table (ICLR 2025, HKUST).

## 4. 주요 기여
- 온라인 시계열 예측의 정보 누출 문제 명확히 정의·해결
- Slow+Fast 이중 스트림으로 완전 갱신과 빠른 적응 동시 달성
- Teacher-Student + coarse-to-fine residual로 예측 품질 향상
- 다양한 backbone 모델에 독립적으로 적용 가능

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 이중 스트림 유지에 따른 계산 비용 증가
- 확장 아이디어: 비정상(non-stationary) 시계열에 대한 더 강력한 적응 전략

## 6. 참고
- OpenReview: https://openreview.net/forum?id=I0n3EyogMi
- ICLR 2025 Poster: https://iclr.cc/virtual/2025/poster/30196
- Paper PDF: https://proceedings.iclr.cc/paper_files/paper/2025/file/46e624c244cff669223d488defd4e835-Paper-Conference.pdf
- GitHub: https://github.com/yyalau/iclr2025_dsof
