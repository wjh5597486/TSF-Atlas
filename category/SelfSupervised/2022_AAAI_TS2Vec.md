---
title: TS2Vec: Towards Universal Representation of Time Series
authors: Zhihan Yue et al.
venue: AAAI 2022
year: 2022
paper_url: https://ojs.aaai.org/index.php/AAAI/article/view/20881
code_url: https://github.com/zhihanyue/ts2vec
tags: [self-supervised | contrastive-learning | time-domain | representation-learning]
primary_category: category/SelfSupervised/
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2022_AAAI_TS2Vec.pdf
pdf_source: aaai
---

## TL;DR
TS2Vec introduces a universal time series representation learning framework using hierarchical contrastive learning over augmented context views. The learned timestamp-level representations achieve superior results in time series forecasting and anomaly detection tasks without task-specific fine-tuning.

## 1. 기존 모델의 한계 / 가설
- 기존 representation learning 방법들은 시계열의 구조적 특성(temporal dependencies, patterns)을 충분히 활용하지 못함
- 대부분의 작업이 task-specific으로 설계되어 있어 universality 부족
- 통일된 표현 학습 프레임워크의 필요성

## 2. 제안 방법론
### 핵심 아이디어 한 줄 요약
계층적 contrastive learning을 통해 각 timestamp마다 robust한 contextual representation을 학습하는 universal 프레임워크

### 아키텍처 구성요소
- **Encoder**: 기본 neural network(예: Transformer, GRU) 기반 인코더
- **Context View Augmentation**: 입력 시계열에서 overlapping subseries를 randomly sampling하여 두 개의 context view 생성
- **Hierarchical Contrastive Loss**: 
  - Temporal Contrastive Loss (TCL): 시간 영역에서 인접한 representations 간 consistency 강화
  - Instance-wise Contrastive Loss (ICL): 전체 시계열 레벨에서 서로 다른 instance 구분

### 주요 수식
$$\mathcal{L}_{TCL} = -\log \frac{\exp(\text{sim}(z_t, z'_t) / \tau)}{\sum_k \exp(\text{sim}(z_t, z'_k) / \tau)}$$

여기서 $z_t$와 $z'_t$는 overlapping segment의 $t$번째 timestamp에 대한 representations

### 학습 전략
- 두 overlapping subseries의 공통 segment에 대해 consistency 강화
- Raw input을 encoder에 직접 입력하여 representation 학습
- Downstream task에 맞춰 fine-tuning 가능

## 3. ETTh1 실험 결과

### 3.1 Setting (필수)
논문이 ETTh1 결과를 **명확하게 보고하지 않음**. 대신 forecasting 및 anomaly detection 응용에서 superior performance를 시연함.
- Input length (lookback): 문제별로 다름 (forecasting에서 96 사용)
- Forecast horizons: 96 (표준 LTSF 설정)
- Normalization: 표준 (z-score)
- 기타: linear regression을 learned representation 위에서 학습

### 3.2 Results
논문은 주로 UCR/UEA 데이터셋, Energy/Traffic/ECL/Weather 등 다양한 데이터셋에서 평가함.
- **SOTA 시계열 분류**: UCR 125 datasets에서 평균 2.4% accuracy 향상, UEA 29 datasets에서 3.0% 향상
- **Forecasting**: Linear regression on top of learned representations이 기존 SOTA 방법들 outperform
- **Anomaly Detection**: Unsupervised anomaly detection에서 SOTA 달성

## 4. 주요 기여
- Universal representation learning framework for time series (forecasting, classification, anomaly detection 모두 지원)
- Hierarchical contrastive learning 통한 robust timestamp-level representation 학습
- Task-specific fine-tuning 없이도 다양한 downstream task에 적용 가능
- 광범위한 실험 (UCR/UEA + multiple application domains)을 통한 generalization 입증

## 5. 한계 및 후속 연구 아이디어
- Overlapping subseries를 fixed ratio로 sampling하는데, adaptive sampling 전략이 성능을 높일 수 있을 것으로 예상
- Computational cost 분석 부족 (특히 고차원 시계열)
- 특정 domain-specific patterns에 대한 explicit modeling이 없음 (예: seasonality, trend)
- ※ 메모: TS2Vec은 self-supervised learning이므로 labeled data 부족 환경에서 특히 유용할 것으로 예상됨

## 6. 참고
- GitHub: https://github.com/zhihanyue/ts2vec
- arXiv: https://arxiv.org/abs/2106.10466
- 관련 논문: CoST (seasonal-trend disentanglement), SimMTM (self-supervised variant for forecasting)
