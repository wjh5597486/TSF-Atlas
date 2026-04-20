---
title: "WPMixer: Efficient Multi-Resolution Mixing for Long-Term Time Series Forecasting"
authors: Md Mahmuddun Nabi Murad et al.
venue: AAAI
year: 2025
paper_url: https://arxiv.org/abs/2412.17176
code_url: https://github.com/Secure-and-Intelligent-Systems-Lab/WPMixer
tags: [freq-domain, wavelet, mlp, mixer, patching, channel-independent]
primary_category: category/Freq-domain/WaveletTransform/Architecture/MLP
related_categories:
  - category/CrossCutting/Patching
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2025_AAAI_WPMixer.pdf
pdf_source: arxiv
---

## TL;DR
WPMixer는 다중 해상도 웨이블릿 분해, 패칭, MLP 믹싱을 결합한 경량 MLP 기반 장기 시계열 예측 모델이다. 기존 Transformer 기반 모델 대비 계산 효율성이 크게 향상되면서도 7개 벤치마크에서 SOTA 성능을 달성한다.

## 1. 기존 모델의 한계 / 가설
- Transformer 기반 모델은 복잡하고 연산 비용이 높아 실용성이 낮음
- 단순 MLP 기반 모델(DLinear 등)은 패턴 캡처 능력이 제한적
- 시계열의 다중 해상도(주파수 대역별) 정보를 충분히 활용하지 못함
- 가설: 웨이블릿 분해로 다중 해상도 특징을 추출하고 패칭·믹싱으로 결합하면 효율적이면서 강력한 예측이 가능함

## 2. 제안 방법론
- **핵심 아이디어**: 웨이블릿 다중 해상도 분해 → 패치 임베딩 → MLP 믹싱
- **아키텍처 구성요소**:
  1. **Multi-level Wavelet Decomposition**: 입력 시계열을 여러 해상도의 approximation/detail 계수로 분해
  2. **Patching and Embedding**: 각 해상도 계수 시리즈에 패칭 적용 후 임베딩
  3. **MLP Mixing**: patch mixer와 embedding mixer로 로컬/글로벌 정보 통합
  4. 각 해상도 브랜치는 독립적인 normalization, patching, mixing, head 모듈로 구성
- **일반 프레임워크**: MLP, LSTM, Transformer 등 임의 딥러닝 아키텍처에 적용 가능
- **TS-Learner**: WPMixer에 최적화된 MLP 기반 서브모델

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 (기본값)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: RevIN 적용
- Train/Val/Test split: 표준 chronological split
- Batch size / Optimizer / LR: Adam, 논문 참조
- Loss function: MSE
- 기타: 단일 GPU 환경

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | 0.347 | 0.383 | TimeMixer (0.361/0.390) 대비 우수 |
| 192     | see paper | see paper | — |
| 336     | see paper | see paper | — |
| 720     | see paper | see paper | — |

출처: Table 1 of paper.

## 4. 주요 기여
- 웨이블릿 다중 해상도 분해를 시계열 예측에 효과적으로 통합
- MLP 기반으로 Transformer 대비 낮은 계산 복잡도 달성
- 패칭과 믹싱을 결합한 새로운 프레임워크 제안
- 7개 벤치마크에서 SOTA 달성
- 임의 딥러닝 아키텍처에 plug-in 가능한 범용 프레임워크

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: 웨이블릿 분해 level 수 및 타입 선택에 대한 hyperparameter 민감도
- ※ 메모: WaveletTransform 기반 MLP이므로 Freq-domain/WaveletTransform/Architecture/MLP primary; 패칭 사용으로 CrossCutting/Patching 심볼릭 링크

## 6. 참고
- [arXiv 2412.17176](https://arxiv.org/abs/2412.17176)
- [GitHub](https://github.com/Secure-and-Intelligent-Systems-Lab/WPMixer)
- 비교 논문: TimeMixer, WaveletMixer, PatchTST, iTransformer
