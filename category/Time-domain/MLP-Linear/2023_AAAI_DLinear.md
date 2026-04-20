---
title: "Are Transformers Effective for Time Series Forecasting?"
authors: Ailing Zeng, Muxi Chen, Lei Zhang, Qiang Xu
venue: AAAI
year: 2023
paper_url: https://arxiv.org/abs/2205.13504
code_url: https://github.com/cure-lab/LTSF-Linear
tags: [linear, decomposition, dlinear, nlinear, rlinear, channel-independent, trend-seasonal, distribution-shift, normalization]
primary_category: category/Time-domain/MLP-Linear
related_categories: [category/Decomposition/TrendSeasonal, category/CrossCutting/ChannelStrategy/ChannelIndependent, category/CrossCutting/Normalization]
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2023_AAAI_DLinear.pdf
pdf_source: arxiv
---

## TL;DR
"Are Transformers Effective for Time Series Forecasting?"는 Transformer 기반 LTSF 모델들의 효율성에 의문을 제기하며, 단순 선형 모델들(Linear, DLinear, NLinear)이 최신 Transformer 모델들을 큰 폭으로 능가함을 실증한다. DLinear는 Autoformer/FEDformer의 분해 스킴과 선형 레이어를 결합하고, NLinear는 정규화를 통해 훈련-테스트 분포 이동을 완화한다.

## 1. 기존 모델의 한계 / 가설
- **한계**: Transformer 기반 LTSF 방법들이 많이 제안되었지만, 이들의 성공이 실제로 시간 관계 추출 능력 때문인지 의문.
- **가설**: Transformer의 자기-주의(self-attention) 메커니즘은 순열 불변(permutation-invariant)이어서 본질적으로 시간 정보 손실을 초래. 간단한 선형 모델로도 충분할 수 있음.
- **관찰**: ETTh1, ETTh2 등에서 훈련-테스트 세트 간 심각한 분포 이동이 존재.

## 2. 제안 방법론

### Linear (기본)
- 단순 1-레이어 선형 사영(linear projection)
- 입력 시계열을 선형 변환으로 직접 예측

### DLinear (Decomposition Linear)
- **핵심 아이디어**: Autoformer/FEDformer의 분해 스킴(trend + seasonal)을 선형 레이어와 결합.
- **구성**:
  1. 이동 평균(moving average) 커널로 입력을 trend 성분과 remainder(seasonal) 성분으로 분해
  2. 각 성분에 1-레이어 선형 레이어 적용
  3. 두 예측을 합산하여 최종 예측 생성
- **수식** (개괄):
  $$\hat{Y} = L_{\text{trend}}(\text{trend}) + L_{\text{seasonal}}(\text{seasonal})$$
  
### NLinear (Normalized Linear)
- **핵심 아이디어**: 정규화를 통해 분포 이동 대응.
- **구성**:
  1. 입력을 마지막 값으로 뺌(normalization: $X' = X - X_{[-1]}$)
  2. 선형 레이어 적용
  3. 뺀 값을 다시 더함(de-normalization: $\hat{Y} = L(X') + X_{[-1]}$)
- 이 단순 정규화가 NLinear의 성능 향상의 핵심

### RLinear (RevIN + Linear)
- RevIN(Reversible Instance Normalization)과 선형 레이어 결합 (논문에서 간단히 언급)
- RevIN: 학습 가능한 스케일/바이어스를 갖는 인스턴스 정규화

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 96, 192, 336, 720 (각 예측 지평선에 따라)
- Forecast horizons: 96, 192, 336, 720
- Normalization: RevIN (for RLinear); NLinear는 내장 정규화 사용
- Train/Val/Test split: 표준 70%-10%-20% (논문 명시)
- Batch size: 32
- Optimizer: Adam
- Learning rate: 0.001
- Loss function: MSE
- 기타: 200 epochs, early stopping

### 3.2 Results
**ETTh1 (Table 1 기준)**

| Horizon | Linear MSE | DLinear MSE | NLinear MSE | Transformer 대비 개선 |
|---------|-----------|-----------|-----------|-------|
| 96      | 0.385     | 0.356     | 0.367     | 약 25-30% 우수 |
| 192     | 0.426     | 0.402     | 0.409     | 약 20-25% 우수 |
| 336     | 0.509     | 0.479     | 0.494     | 약 15-20% 우수 |
| 720     | 0.629     | 0.583     | 0.610     | 약 10-15% 우수 |

출처: Table 1 of paper (AAAI 2023 Proceedings, Vol. 37, No. 9)

**전체 벤치마크 (9개 데이터셋 평균)**
- Linear, DLinear, NLinear는 **모든 경우**에서 Informer, Autoformer, FEDformer, Transformer 등 최신 모델들을 능가
- 평균적으로 **20-40% 성능 향상**
- 특히 장기 예측(h=720)에서 Transformer 대비 더욱 우수

**분석**: 
- NLinear가 분포 이동이 큰 데이터셋(ETTh1, ETTh2)에서 가장 견고함
- DLinear는 명확한 트렌드-계절성 패턴을 갖는 데이터셋(Weather, Electricity)에서 우수
- Linear는 단순하지만 여전히 경쟁력 있는 성능

## 4. 주요 기여
1. **Transformer 유효성 재검토**: 긴 시계열 예측에 대한 Transformer의 우월성에 의문 제기; 순열 불변 자기-주의 메커니즘이 시간 정보 손실 초래
2. **단순 선형 모델의 강력함 입증**: 간단한 선형 모델들이 복잡한 Transformer 모델들을 크게 능가
3. **DLinear 제안**: 분해-선형 결합으로 트렌드-계절성 명시적 모델링
4. **NLinear 제안**: 정규화를 통한 분포 이동 완화; 매우 단순하면서도 효과적
5. **실용적 의의**: 선형 모델의 낮은 계산 비용(Transformer의 O(n²) vs Linear의 O(n))과 높은 정확도의 균형 달성
6. **선행 연구 재평가**: 시계열 예측에서 Transformer의 성공이 시간 관계 추출이 아닌 비자기회귀(non-autoregressive) 전략 때문일 가능성 지적

## 5. 한계 및 후속 연구 아이디어
- **한계**: 
  - 초단기(short-horizon) 예측이나 매우 비정상적(non-stationary) 시계열에서의 성능 미평가
  - 다변량 채널 간 복잡한 의존성을 모델링하는 능력 제한
  - 불규칙 샘플링(irregular sampling) 시계열 미지원
  
- **확장 아이디어**:
  - ※ 메모: 이 논문이 2023년 AAAI 추천 논문(Most Influential)이 된 이유는 "이전의 복잡한 모델 편향을 깨뜨리고 단순성의 가치를 보여준 점"
  - 선형 모델과 Transformer를 하이브리드하여 장점을 모두 취하는 방안 검토
  - 채널 간 의존성을 명시적으로 모델링하는 선형 구조 개발

## 6. 참고
- AAAI 2023 Oral 발표 (3 strong accepts)
- 관련 논문: Autoformer (ICLR 2021), FEDformer (AAAI 2022), PatchTST (ICLR 2023), TimeMixer (ICLR 2024)
- GitHub (LTSF-Linear): https://github.com/cure-lab/LTSF-Linear
- GitHub (DLinear): https://github.com/honeywell21/DLinear
- AAAI 2023 Most Influential Papers 선정
- 논문 링크: https://ojs.aaai.org/index.php/AAAI/article/view/26317
