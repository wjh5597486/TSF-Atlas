---
title: "HDMixer: Hierarchical Dependency with Extendable Patch for Multivariate Time Series Forecasting"
authors: Qihe Huang et al.
venue: AAAI
year: 2024
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/29155
code_url: https://github.com/hqh0728/HDMixer
tags: [time-domain, mlp, patching, multivariate, hierarchical]
primary_category: category/Time-domain/MLP-Linear
related_categories:
  - category/CrossCutting/Patching
  - category/CrossCutting/ChannelStrategy/ChannelDependent
reports_etth1: true
swept_in: 2026-04-18
pdf_local: ./2024_AAAI_HDMixer.pdf
pdf_source: arxiv
---

## TL;DR
HDMixer는 순수 MLP 기반 MTSF 모델로, 패치 경계 정보를 풍부하게 하는 Length-Extendable Patcher(LEP)와 패치 내 단기 의존성, 패치 간 장기 의존성, 변수 간 복잡한 상호작용을 계층적으로 모델링하는 Hierarchical Dependency Explorer(HDE)로 구성된다. 기존 패치 기반 모델들이 패치 간 장기 의존성에만 집중한 것과 달리 세 가지 차원을 동시에 다룬다.

## 1. 기존 모델의 한계 / 가설
- PatchTST, TimesNet 등 기존 패치 기반 모델들은 주로 패치 간(cross-patch) 장기 의존성 모델링에만 집중하고, 패치 내(intra-patch) 단기 의존성과 변수 간(cross-variable) 복잡한 상호작용을 무시함.
- 고정 길이 패치는 경계 영역에서 의미론적 불일치를 일으켜 표현 품질을 저하시킴.
- 가설: 패치 내/간/변수 간 의존성을 계층적으로 동시에 모델링하면 예측 성능이 향상된다.

## 2. 제안 방법론
- **핵심 아이디어**: 세 가지 스케일의 의존성을 계층적으로 탐색하는 MLP 모델.
- **Length-Extendable Patcher (LEP)**:
  - 적응형 길이 패칭으로 패치 경계의 의미론적 비일관성 완화.
  - 인접 패치와의 오버랩을 허용하여 경계 정보 보완.
- **Hierarchical Dependency Explorer (HDE)**:
  1. **Intra-patch MLP**: 패치 내 시간 스텝 간 단기 의존성 캡처.
  2. **Inter-patch MLP**: 패치 간 장기 의존성 캡처.
  3. **Cross-variable MLP**: 변수 간 복잡한 비선형 상호작용 캡처.
- 손실함수: MSE.
- 학습 전략: 표준 지도학습, RevIN 정규화 사용.

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
- Input length (lookback): 336 또는 512 (논문별 설정)
- Forecast horizons: {96, 192, 336, 720}
- Normalization: RevIN (인스턴스 정규화)
- Train/Val/Test split: 표준 ETTh1 분할
- Batch size / Optimizer / LR: Adam, 표준 설정
- Loss function: MSE
- 기타: 9개 실제 데이터셋에서 실험 (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Traffic, Exchange, ILI)

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      | see paper | see paper | PatchTST, iTransformer 대비 경쟁적 |
| 192     | see paper | see paper | 경쟁적 |
| 336     | see paper | see paper | 경쟁적 |
| 720     | see paper | see paper | 경쟁적 |

출처: Table 1/2 of paper. 9개 데이터셋 전체에서 우월한 성능 달성 주장.

## 4. 주요 기여
- 패치 내, 패치 간, 변수 간 세 가지 계층적 의존성을 통합적으로 탐색하는 최초의 MLP 기반 프레임워크.
- Length-Extendable Patcher(LEP)로 패치 경계의 의미 비일관성 문제 해결.
- 파라미터 효율적인 순수 MLP 설계로 Transformer 대비 빠른 추론.
- 9개 표준 벤치마크에서 SOTA 또는 경쟁적 성능.

## 5. 한계 및 후속 연구 아이디어
- 저자 명시: MLP 기반이므로 비선형 장거리 의존성 캡처에 한계 가능성 존재.
- ※ 메모: 계층적 의존성 탐색 방식을 SSM(Mamba)과 결합하는 방향 흥미로움.
- ※ 메모: LEP의 최적 오버랩 비율 자동 탐색이 향후 연구 주제가 될 수 있음.

## 6. 참고
- AAAI 2024 Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/29155
- GitHub: https://github.com/hqh0728/HDMixer
- ※ 원본 PDF 미취득: 공개 arXiv 버전 없음. AAAI proceedings에서 접근 가능.
