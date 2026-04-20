---
title: CATN: Cross Attentive Tree-Aware Network for Multivariate Time Series Forecasting
authors: Hui He et al.
venue: AAAI 2022
year: 2022
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/20320
code_url: 
tags: [time-domain | transformer | attention | tree-network | multivariate | hierarchical]
primary_category: category/Time-domain/Transformer/
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_AAAI_CATN.pdf
pdf_source: aaai
---

## TL;DR
CATN proposes an end-to-end Cross Attentive Tree-Aware Network that jointly captures inter-series correlation and intra-series temporal patterns using a tree structure and multi-level dependency learning. The paper is the first to introduce a tree-based neural network for multivariate time series forecasting.

## 1. 기존 모델의 한계 / 가설
- 기존 LSTM, GRU 기반 방법들은 장기 의존성(long-range dependency) 학습 부족
- Vanilla Transformer는 모든 시점과 시계열 간의 관계를 균등하게 처리하여 비효율적
- 실제 multivariate 시계열에는 계층적 구조와 grouped 상관관계가 존재하는데, 이를 명시적으로 모델링하지 못함
- Tree 구조를 통해 inter-series correlation의 계층적 특성을 활용할 필요

## 2. 제안 방법론
### 핵심 아이디어 한 줄 요약
Tree 구조를 기반으로 inter-series correlation을 계층화하고, 다층 dependency learning으로 global, local, cross-level 의존성을 통합하여 multivariate 시계열 예측

### 아키텍처 구성요소
- **Tree Construction Module**: 
  - 시계열들 간의 correlation을 기반으로 tree structure 구성
  - Similarity-based tree 구조로 related series 계층화
  
- **Multi-level Dependency Learning**:
  - **Local Level**: Subtree 내의 시계열들 간 관계
  - **Global Level**: 전체 tree 구조에서의 의존성
  - **Cross-level**: 계층 간 information flow
  
- **Cross Attention Mechanism**:
  - 각 시계열이 다른 시계열들로부터 relevant information 선택적으로 학습
  - Multi-head attention으로 다양한 의존성 패턴 포착
  
- **Temporal Encoder**: 
  - 각 시계열의 temporal pattern을 sequence 형태로 인코딩

### 주요 알고리즘
1. Tree 구조에 기반하여 series 그룹화
2. 각 노드에서 multi-level attention 계산
3. Cross attention으로 서로 다른 시계열의 정보 fusion
4. Decoder에서 future steps 예측

## 3. ETTh1 실험 결과

### 3.1 Setting
논문이 ETTh1을 포함한 표준 LTSF 벤치마크에서 평가함으로 보고되나, 구체적인 hyperparameter setting이 공개 자료에서 명확하지 않음.
- Input length (lookback): 표준 설정 (96 예상)
- Forecast horizons: 표준 {96, 192, 336, 720} (LTSF 설정)
- Normalization: 표준 (z-score 또는 MinMax)
- Batch size / Optimizer: 논문 참고 필요

### 3.2 Results
논문은 여러 datasets (ETTh1, ETTh2, Electricity, Weather, Traffic 포함)에서 평가하고 있으며:
- MAE/RMSE/MAPE에서 11.7%/3.62%/39.03% improvements 달성 (특정 dataset 대비)
- Tree-based 구조로 인한 efficiency improvement (computation cost 감소)
- 기존 Transformer/CNN/RNN 기반 방법 대비 경쟁력 있는 성능

출처: AAAI 2022 proceedings paper, specific table 확인 필요

## 4. 주요 기여
- **First tree-based approach for MTS forecasting**: 계층적 correlations를 tree 구조로 모델링
- **Multi-level dependency learning**: Global, local, cross-level dependencies를 통합
- **Cross attention mechanism**: 각 series가 다른 series의 관련 정보 선택적 학습
- **Computational efficiency**: Tree 구조로 인한 복잡도 감소
- **Comprehensive experiments**: 다양한 datasets에서 평가 (ETT, Electricity, Weather, Traffic 포함)

## 5. 한계 및 후속 연구 아이디어
- Tree 구조 자동 구성의 안정성: Similarity 기반 tree construction이 모든 data에 적합한지 검증 필요
- Scalability: 매우 높은 차원(high-dimensional) 시계열에서의 성능 미평가
- Interpretability: Tree 구조의 의미 해석 및 시각화 제한
- ※ 메모: Tree 구조는 domain knowledge 통합 가능성 있음 (예: 시계열들 간의 물리적 관계를 prior로 사용)

## 6. 참고
- Paper: https://ojs.aaai.org/index.php/AAAI/article/view/20320
- AAAI 2022 Virtual Chair: https://aaai-2022.virtualchair.net/poster_aaai7403
- Related work: Graph neural networks for time series (GNN-based approaches), Transformer architectures for multivariate forecasting
