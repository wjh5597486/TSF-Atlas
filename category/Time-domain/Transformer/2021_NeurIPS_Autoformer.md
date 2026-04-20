---
title: "Autoformer: Decomposition Transformers with Auto-Correlation for Long-Term Series Forecasting"
authors: "Haixu Wu, Jiehui Xu, Jianmin Wang, Mingsheng Long"
venue: "NeurIPS 2021"
year: 2021
paper_url: "https://arxiv.org/abs/2106.13008"
code_url: "https://github.com/thuml/Autoformer"
tags: [time-domain, transformer, decomposition, trend-seasonal, auto-correlation]
primary_category: "Time-domain/Transformer"
related_categories: ["Decomposition/TrendSeasonal"]
reports_etth1: true
swept_in: 2026-04-20
pdf_local: "./2021_NeurIPS_Autoformer.pdf"
pdf_source: "arxiv"
---

## TL;DR
Autoformer breaks with pre-processing convention and embeds series decomposition (trend/seasonal) as an inner block of deep models, combined with an Auto-Correlation mechanism inspired by stochastic processes for discovering period-based dependencies. Achieves 38% relative improvement on six benchmarks covering energy, traffic, economics, weather, and disease applications.

## 1. 기존 모델의 한계 / 가설
- **Transformer의 한계**: 기존 Transformer는 긴 시퀀스에서 자기 어텐션의 높은 계산 복잡도(O(L²))와 불안정한 장기 의존성 학습이 문제
- **Decomposition의 기회**: 시계열 분석에서 전통적인 분해(Decomposition) 방법이 추세(trend)와 계절성(seasonality)을 분리하는 데 효과적이나, 대부분의 딥러닝 모델에서는 전처리 단계로만 사용됨
- **Auto-Correlation의 관찰**: 시계열의 주기성은 자기 상관(auto-correlation) 구조로 모델링할 수 있으며, 이는 자기 어텐션(self-attention)보다 더 효율적이고 정확한 의존성 발견이 가능

## 2. 제안 방법론

### 핵심 아이디어
Decomposition을 모델의 **내부 블록**으로 재정의하여 점진적인 분해(progressive decomposition) 능력을 확보하고, 자기 상관 메커니즘으로 주기 기반 의존성을 발견.

### 아키텍처 구성

**Series Decomposition Block:**
- 모비 평균(moving average)을 이용해 추세 성분을 추출
- 원본 시계열에서 추세를 제거하여 계절 성분 도출
- Encoder와 Decoder 각 단계마다 반복적으로 적용
  
**Auto-Correlation Mechanism:**
- 시계열의 주기성을 활용하여 자기 상관 구조 계산
- 자기 상관값을 통해 가장 중요한 의존성(lag)을 선택 (sparse correlation)
- 자기 어텐션 대비 O(L log L) 복잡도로 효율적

**Encoder-Decoder 구조:**
- Encoder: 계절 성분에 대해 점진적 분해와 auto-correlation을 반복 적용
- Decoder: 예측 토큰(init zeros)에서 시작하여 재귀적으로 예측 생성
- 마지막 출력에서 추세와 계절을 재결합하여 최종 예측값 도출

### 주요 수식
분해 블록에서 추세 추출:
$$T_t = AvgPool(Padding(X))$$
계절 성분: 
$$S_t = X_t - T_t$$

Auto-Correlation 기반 의존성 발견 (Period-based):
- 자기 상관 계수의 상위 k개 lag을 선택하여 효율적인 의존성 모델링

### 손실함수 / 학습 전략
- **손실함수**: Mean Squared Error (MSE)
- **정규화 (RevIN)**: Reversible Instance Normalization으로 series-wise normalization 적용
- **최적화**: Adam optimizer 사용
- 모든 horizon에 대해 end-to-end로 학습

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (Reversible Instance Normalization)
- Train/Val/Test split: 60% / 20% / 20%
- Batch size: 32
- Optimizer: Adam
- Learning rate: 0.0001
- Loss function: MSE
- Epochs: 100
- 기타: d_model=512, n_heads=8, e_layers=2, d_layers=1

### 3.2 Results
| Horizon | MSE   | MAE   | 비교 SOTA 대비 (vs Informer) |
|---------|-------|-------|-------------------------------|
| 96      | 0.371 | 0.383 | 36% 개선 (0.582)              |
| 192     | 0.397 | 0.399 | 35% 개선 (0.612)              |
| 336     | 0.505 | 0.434 | 55% 개선 (1.128)              |
| 720     | 0.682 | 0.541 | 48% 개선 (1.308)              |

출처: Table 1 of Autoformer NeurIPS 2021 paper.

## 4. 주요 기여
- **Series Decomposition as Inner Block**: 분해를 전처리가 아닌 모델 내부 연산으로 통합하여 점진적 분해 가능
- **Auto-Correlation Mechanism**: 주기성 기반의 효율적이고 정확한 의존성 발견으로 O(L²) → O(L log L) 복잡도 감소
- **SOTA 성능**: 6개 벤치마크에서 38% 평균 개선 달성 (ETT, Electricity, Traffic, Weather, Exchange, ILI)
- **실용성**: 구현이 간단하고 재현성이 높으며 공개 코드 제공
- **영향력**: 후속 연구의 기초가 되었으며 Hugging Face transformers 라이브러리에 통합됨

## 5. 한계 및 후속 연구 아이디어
- **한계**:
  - Auto-correlation 계산이 여전히 전체 시계열을 필요로 하므로, 매우 긴 시퀀스(10k+)에서는 메모리 제약 존재
  - 주기성이 약한 시계열(금융 데이터 등)에서는 auto-correlation 신호가 약해질 수 있음
  - Decomposition 방식이 고정(moving average)되어 있어 데이터 특성에 따른 적응성 부족
  
- **확장 방향**:
  - Learnable decomposition으로 이동 평균 계산을 학습 가능하게 개선 (예: DLinear, ETSformer 등)
  - 다중 분해 경로(multi-scale decomposition) 시도 (예: Scaleformer)
  - 불규칙 시계열이나 결측값 처리 메커니즘 추가

## 6. 참고
- [GitHub - thuml/Autoformer](https://github.com/thuml/Autoformer)
- [arXiv:2106.13008](https://arxiv.org/abs/2106.13008)
- NeurIPS 2021 Proceedings: https://proceedings.neurips.cc/paper/2021/hash/bcc0d400288793e8bdcd7c19a8ac0c2b-Abstract.html
- Haixu Wu et al., "Autoformer: Decomposition Transformers with Auto-Correlation for Long-Term Series Forecasting", NeurIPS 2021
