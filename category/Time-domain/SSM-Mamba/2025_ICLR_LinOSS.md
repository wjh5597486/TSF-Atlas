---
title: "Oscillatory State-Space Models"
authors: T. Konstantin Rusch et al.
venue: ICLR 2025
year: 2025
paper_url: https://arxiv.org/abs/2410.03943
code_url: https://github.com/tk-rusch/linoss
tags: [ssm, oscillatory, state-space, long-range, sequence-modeling, stable, universal]
primary_category: category/Time-domain/SSM-Mamba
related_categories: []
reports_etth1: false
swept_in: 2026-04-18
pdf_local: ./2025_ICLR_LinOSS.pdf
pdf_source: arxiv
---

## TL;DR
LinOSS(Linear Oscillatory State-Space models)는 생물학적 신경망의 피질 진동 역학에서 영감을 받은 강제 조화 진동자 시스템 기반 SSM이다. nonnegative 대각 상태 행렬만으로 안정적 역학을 보장하며 보편 근사기(universal approximator)임이 증명되었다. 분류·회귀·장기 예측에서 Mamba/LRU 대비 일관된 우위를 보이며 ICLR 2025 Oral로 발표되었다.

## 1. 기존 모델의 한계 / 가설
- 기존 SSM(S4, Mamba, LRU 등)은 제한적 파라미터화나 복잡한 제약에 의존
- 장기(long-range) 시퀀스에서 Mamba 계열이 불안정하거나 성능이 저하되는 문제
- 신경망 역학에서의 진동(oscillation)이 장거리 의존성 모델링에 유리하다는 관찰

## 2. 제안 방법론
- 핵심 아이디어: 강제 조화 진동자 기반 SSM으로 nonnegative 대각 행렬만으로 안정성 보장
- **LinOSS-IMEX**: implicit-explicit 이산화로 시간 가역성(time reversibility) 보존
- **LinOSS-IM**: implicit 이산화
- 이론적 보장: 안정성(nonnegative diagonal), 보편성(universal approximation), 시간 가역성
- Fast parallel associative scan으로 효율적 계산
- 손실함수: 태스크별 CE/MSE 등

## 3. ETTh1 실험 결과
> 본 논문은 ETTh1/ETTh2/ETTm1/ETTm2/Electricity/Traffic 등 표준 LTSF 벤치마크를 평가하지 않으므로 본 섹션 전체 생략. frontmatter `reports_etth1: false`.

※ 메모: 논문에서 평가한 예측 벤치마크는 Weather(기온·기후 변수 예측, 720 타임스텝 horizon)에 한정됨. 표준 ETT/Electricity/Traffic 벤치마크는 평가하지 않음. MSE 기준 LinOSS-IMEX 0.508 (Table 3).

## 4. 주요 기여
- 강제 조화 진동자 기반 선형 SSM으로 안정성·보편성·시간 가역성 동시 보장
- nonnegative 대각 제약만으로 안정 역학 달성 (기존 모델의 restrictive 파라미터화 불필요)
- 분류(UEA), 회귀(PPG), 예측(Weather) 태스크에서 Mamba·LRU 대비 일관된 우위
- ICLR 2025 Oral 발표 (상위 발표)

## 5. 한계 및 후속 연구 아이디어
- 저자 명시 한계: 표준 LTSF 벤치마크(ETT, Electricity 등) 평가 미포함
- 확장 아이디어: ETTh1 등 표준 다변량 예측 벤치마크 적용 및 iTransformer/PatchTST와 직접 비교

## 6. 참고
- arXiv: https://arxiv.org/abs/2410.03943
- OpenReview: https://openreview.net/forum?id=GRMfXcAAFh
- ICLR 2025 Oral: https://iclr.cc/virtual/2025/oral/31880
- GitHub: https://github.com/tk-rusch/linoss
- MIT CSAIL 블로그: https://www.csail.mit.edu/news/novel-ai-model-inspired-neural-dynamics-brain
