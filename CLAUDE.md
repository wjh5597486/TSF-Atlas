
작성 원칙·템플릿·수집 워크플로 → [CLAUDE.md](./CLAUDE.md)

# TSF-Atlas

ETTh1 데이터셋을 사용하는 Time Series Forecasting(TSF) 모델들을 조사·정리하는 저장소.

## 1. 조사 범위 (Scope)

- **대상 학회**: NeurIPS, ICML, ICLR, AAAI (필요 시 KDD, CVPR, IJCAI 포함)
- **대상 저널**: Q1 이상 (TPAMI, TNNLS, TKDE, JMLR 등)
- **연도**: 제한 없음 (단, 최근 5년 우선)
- **ETTh1 실험**: 필수는 아님. 단, **ETTh1 결과를 보고한 논문이라면 재현에 필요한 setting(input length, horizon, normalization, lookback, batch size 등)을 반드시 기록**.

## 2. 폴더 구조 규칙

```
category/
├── Time-domain/
│   ├── Transformer/              # Informer, Autoformer, PatchTST, iTransformer, Crossformer,
│   │                             # NS-Transformer, Pyraformer, Reformer, SAMformer, ContiFormer ...
│   ├── MLP-Linear/               # DLinear, NLinear, RLinear, TSMixer, TiDE, LightTS, SparseTSF, TimeMixer(MLP) ...
│   ├── CNN-TCN/                  # SCINet, MICN, ModernTCN
│   ├── SSM-Mamba/                # TimeMachine, MambaTS, SiMBA, Chimera, S-Mamba, Bi-Mamba+
│   ├── Graph/                    # MTGNN, CrossGNN, StemGNN(시간 분지)
│   └── Generative/
│       ├── Diffusion/            # TimeGrad, CSDI, TimeDiff, Diffusion-TS, TMDM, mr-Diff, RATD, NSDiff
│       └── VAE-GAN/              # D3VAE, TimeVAE, SSSD
├── Freq-domain/
│   ├── FFT/
│   │   └── Architecture/
│   │       ├── Transformer/      # Fredformer
│   │       ├── MLP/              # FreTS, FiLM
│   │       └── Linear/           # FITS
│   ├── WaveletTransform/
│   │   └── Architecture/
│   │       ├── Transformer/      # W-Transformer, FEDformer-wavelet 변형
│   │       └── MLP/              # WPMixer
│   ├── LearnableFilter/          # FilterNet, TSLANet
│   └── PeriodicityGuided/        # TimesNet(FFT로 주기 선택), CycleNet
├── Decomposition/
│   ├── TrendSeasonal/            # ETSformer, Autoformer(symlink), DLinear(symlink), FEDformer(symlink), TimeMixer(symlink)
│   ├── MultiScale/               # Scaleformer, Pathformer(symlink), Pyraformer(symlink), TimeMixer(symlink), MICN(symlink)
│   └── Operator/                 # Koopa(Koopman), ContiFormer(Neural ODE, symlink)
├── Foundation-LLM/
│   ├── Reprogramming/            # Time-LLM, GPT4TS/OFA, LLM4TS, TEMPO, AutoTimes, CALF, S2IP-LLM
│   └── Pretrained-Native/        # Timer, MOMENT, Chronos, TimesFM, Lag-Llama, MOIRAI, MOIRAI-MoE, Toto
├── SelfSupervised/               # SimMTM, TS2Vec, CoST, LaST, BTSF
└── CrossCutting/                 # primary .md는 위 트리에 존재. 여기는 전부 **심볼릭 링크**.
    ├── ChannelStrategy/
    │   ├── ChannelIndependent/   # PatchTST, DLinear, TiDE
    │   └── ChannelDependent/     # iTransformer, Crossformer, TSMixer, SOFTS
    ├── Normalization/            # RevIN, Non-stationary Transformer, Fredformer, SAN
    ├── Patching/                 # PatchTST, PETformer, MOIRAI
    └── Loss/                     # Diffusion-TS(Fourier recon), SAMformer(SAM), wavelet-loss 후보
```

### 분류 원칙
- **중복 배치 허용**. 한 논문이 여러 기여(예: 새 아키텍처 + 새 loss)를 가지면 해당 카테고리 각각에 배치 가능.
  - 실제 `.md`는 **primary 경로에만 1개**, 나머지 경로에는 **상대경로 심볼릭 링크**(`ln -s`). 내용 중복 작성 금지.
  - `.md` frontmatter `primary_category`에 원본 경로, `related_categories`에 링크로 참조되는 경로들을 모두 기재.
- **Primary 경로 선택 규칙**
  1. 자체 프레임워크/아키텍처가 핵심 novelty면 해당 구조 경로가 primary. 예: PatchTST → `Time-domain/Transformer/` (patching은 CrossCutting 심볼릭).
  2. 주파수 변환 자체가 핵심 novelty면 `Freq-domain/`이 primary. 예: FEDformer → `Freq-domain/FFT/Architecture/Transformer/` (Decomposition은 심볼릭).
  3. 같은 비중이면 논문 제목/abstract에서 앞서 언급되는 축을 따름.
  4. 애매하면 사용자에게 확인.
- `CrossCutting/`은 원칙적으로 **심볼릭 링크 전용** 브랜치. primary 파일을 여기 직접 두지 않음.
- 새 카테고리 신설은 논문이 **2편 이상** 모일 때 수행. 1편뿐이면 상위 폴더에 먼저 배치.
- 폴더명은 **PascalCase 또는 Hyphen-Separated**(기존 `Time-domain`, `Freq-domain` 관례 유지). 공백/언더스코어 금지.

## 3. 파일명 규칙

- 형식: `<Year>_<Venue>_<ModelName>.md`
  - 예: `2023_NeurIPS_PatchTST.md`, `2024_ICLR_iTransformer.md`
- 모델명이 없는 경우 1저자 last name 사용: `2022_AAAI_Zhou_etal.md`
- 소문자/대문자는 원 논문 표기 유지.

## 4. 논문 .md 템플릿

각 `.md` 최상단에 YAML frontmatter를 두고, 본문은 아래 섹션을 따름.

```markdown
---
title: <논문 원제목>
authors: <제1저자 et al.>
venue: <NeurIPS 2023 / ICLR 2024 / ...>
year: <YYYY>
paper_url: <arxiv/openreview 링크>
code_url: <있으면 github 링크, 없으면 빈값>
tags: [time-domain | freq-domain | wavelet | fft | transformer | mlp | ssm | diffusion | ...]
primary_category: <실제 .md가 위치한 경로>
related_categories: []   # 심볼릭 링크로 참조되는 다른 카테고리 경로들
reports_etth1: <true | false>
swept_in: <YYYY-MM-DD>        # 이 논문이 수집된 sweep 일자 (COVERAGE.md Sweep Log와 연결)
pdf_local: <./<same_basename>.pdf 또는 빈값>
pdf_source: <arxiv | openreview | acm | publisher | none>
---

## TL;DR
(2~3문장. 무엇을 했고 왜 중요한가.)

## 1. 기존 모델의 한계 / 가설
- 저자가 지적하는 선행 연구의 문제점
- 이 논문이 전제하는 가설 또는 관찰(motivation)

## 2. 제안 방법론
- 핵심 아이디어 한 줄 요약
- 아키텍처 구성요소(블록 단위로)
- 주요 수식(필요 시 LaTeX)
- 손실함수 / 학습 전략

## 3. ETTh1 실험 결과
> 논문이 ETTh1 결과를 **보고한 경우에만 작성**. 보고하지 않으면 본 섹션 전체 생략하고 frontmatter `reports_etth1: false`로만 표시.
> 보고한 경우 **설정(setting)은 필수**. 빠뜨리면 안 됨.

### 3.1 Setting (필수)
- Input length (lookback):
- Forecast horizons:
- Normalization:
- Train/Val/Test split:
- Batch size / Optimizer / LR:
- Loss function:
- 기타 재현 관련 하이퍼파라미터:

### 3.2 Results
| Horizon | MSE | MAE | 비교 SOTA 대비 |
|---------|-----|-----|----------------|
| 96      |     |     |                |
| 192     |     |     |                |
| 336     |     |     |                |
| 720     |     |     |                |

출처: Table X of paper.

## 4. 주요 기여
- bullet 3~5개

## 5. 한계 및 후속 연구 아이디어
- 저자가 명시한 한계
- 읽으면서 든 확장 아이디어(본인 메모)

## 6. 참고
- 관련 논문 / 블로그 / 구현체
```

## 5. 작성 원칙

- **사실과 해석을 분리**: 논문이 주장한 내용은 그대로, 개인 해석은 `※ 메모:` 접두사로 구분.
- ETTh1 결과표는 **숫자를 그대로 옮기되 출처 표(Table X)를 함께 적을 것**. 추정/반올림은 명시.
- 수식은 의미가 불분명한 부분만 선별 기록. 전체 유도식 복사는 지양.
- 그림은 필요 시 `assets/<paper_slug>/` 에 저장하고 상대경로로 참조.

## 6. 인덱스 관리

- 루트의 `INDEX.md`에 전체 논문 목록을 표로 유지.
- 컬럼: 모델명 / 연도 / 학회 / primary 경로 / related 경로 / ETTh1 reports / ETTh1 best MSE.
- 중복 배치 논문은 **INDEX에 1행만** 기재하고 related 경로 컬럼에 콤마로 나열.
- 새 논문을 추가하거나 심볼릭 링크를 만들 때마다 `INDEX.md`도 같이 갱신.

## 7. 작업 시 Claude에게

- 새 논문을 정리해 달라고 요청하면:
  1. 위 스키마에 맞춰 primary 경로에 `.md` 작성
  2. 같은 디렉터리에 **원본 PDF**를 동일 basename으로 저장(§9 참고)
  3. 적절한 카테고리 경로 제안(없으면 신설 여부 확인 후 생성)
  4. 기여가 여러 카테고리에 걸치면 primary 외 경로에 **심볼릭 링크** 생성 (`ln -s ../../../<primary>/file.md <related>/file.md`). PDF는 복제하지 않음.
  5. `INDEX.md` 갱신
  6. sweep 단위 작업 중이었다면 `COVERAGE.md`도 갱신
- primary 경로 선택이 애매할 때는 임의 배치하지 말고 **사용자에게 확인**.

## 8. 수집 워크플로 (Venue-Year Sweep)

한 번의 작업 단위는 **`(venue, year)` 조합**이다. 목표는 해당 조합에서 TSF(ETTh1 관련 또는 ETTh1 비교 가능) 논문을 **전수조사**하여 해당되는 모든 논문에 `.md`가 생성/갱신된 상태로 만드는 것.

### 절차
1. **개시**: 대상 `(venue, year)`를 사용자와 확정. `COVERAGE.md`에서 해당 셀 상태를 `P`(진행중)로 변경.
2. **논문 목록 확보**: OpenReview / Proceedings / dblp / Papers-with-Code 등에서 accepted paper 목록 확보.
3. **필터**: 제목·abstract 기준으로 "time series forecasting / long-term forecasting / multivariate forecasting / ETTh(1-2) / ETTm(1-2) / Electricity / Traffic / Weather / ILI" 키워드가 걸리는 논문 선별.
4. **각 논문 처리**:
   - 이미 `.md`가 존재(같은 논문)면 스킵 또는 보강.
   - 없으면 §4 템플릿으로 작성, §2 Primary 경로 선택 규칙에 따라 primary 결정, 필요 시 심볼릭 링크 생성.
   - 원본 PDF 취득(§9).
5. **갱신**: `INDEX.md`에 각 논문 1행 추가.
6. **완주 마감**: `COVERAGE.md` 해당 셀을 `O`로 변경, `## Sweep Log`에 `YYYY-MM-DD (구분, Venue, Year): n papers collected. notes.` 한 줄 append.

### 상태 기호 (COVERAGE.md와 동일)
- `O` 완료 · `X` 미시작 · `P` 진행중 · `-` 해당 연도에 TSF 논문 없음 확인

### 저널 취급
- 저널은 누적 발행이므로 `(journal, year)` 또는 `(journal, year, 분기)` 단위 스윕 허용. 저널 셀은 해당 연도 전체 분기가 커버되어야 `O`.
- 아직 종료되지 않은 해의 저널은 기본 `P`.

## 9. README.md 유지 규칙

- `README.md`의 폴더 구조 트리에서 각 폴더 주석(`# ...`)에 나열하는 논문은 **대표 3개 + `...`** 형식으로 제한한다.
  - 예: `# iTransformer, CARD, TMDM, ...`
  - 전체 목록은 `INDEX.md`에서 관리하므로 README에서 나열하지 않는다.
- 새 논문을 추가해 CrossCutting 심볼릭 링크를 만들 때, README의 해당 줄 주석이 아직 3개 미만이면 추가하고, 이미 3개 이상이면 `...`만 유지한다.

## 10. 원본 PDF 수집 규칙

- **위치**: 각 논문 `.md`와 **같은 디렉터리**. 파일명은 `.md`와 동일 basename.
  - 예: `.../Time-domain/Transformer/2024_ICLR_iTransformer.md` + `.../Time-domain/Transformer/2024_ICLR_iTransformer.pdf`
- **취득 우선순위**: (1) arXiv PDF → (2) OpenReview PDF → (3) 학회 proceedings PDF → (4) 저자 프로젝트 페이지 / GitHub release.
- **심볼릭 링크 경로**: related 경로에는 `.md` 심볼릭 링크만 둔다. **PDF는 복제하지 않음** (저장소 비대화 방지).
- **취득 실패** 시(유료/접근 불가/링크 부재):
  - frontmatter `pdf_local`은 빈값, `pdf_source: none`
  - `.md` §6 참고 섹션에 `※ 원본 PDF 미취득: <사유>` 한 줄 기록.
- **도구**: 공개 URL은 `WebFetch` 또는 `curl`로 시도. 대량 다운로드 시 사용자 일괄 승인.
- **크기**: 저장소가 방대해지면 향후 `.gitignore` 또는 git-lfs 전환 검토. 현시점에서는 raw 저장.