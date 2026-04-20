# TSF-Atlas

**ETTh1 데이터셋** 기준으로 Time Series Forecasting(TSF) 논문을 체계적으로 수집·분류·정리하는 저장소입니다.

NeurIPS · ICML · ICLR · AAAI · KDD · IJCAI 등 주요 학회와 저널의 TSF 논문을 대상으로, 아키텍처 계열 및 주요 기여 축에 따라 폴더를 구성하고 각 논문마다 `.md` 파일이 작성되어있습니다.


---

## 수집 현황 (Venue × Year)

> 상태 기호: **O** 완료 · **X** 미시작 · **P** 진행중 · **-** TSF 논문 없음 확인

| 구분       | Venue    | 2015 | 2016 | 2017 | 2018 | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 | 2025 | 2026 |
|------------|----------|------|------|------|------|------|------|------|------|------|------|------|------|
| Conference | NeurIPS  | X    | X    | X    | X    | X    | X    | O    | O    | O    | O    | O    | X    |
| Conference | ICML     | X    | X    | X    | X    | X    | X    | O    | O    | O    | O    | O    | X    |
| Conference | ICLR     | X    | X    | X    | X    | X    | X    | O    | O    | O    | O    | O    | X    |
| Conference | AAAI     | X    | X    | X    | X    | X    | X    | O    | O    | O    | O    | O    | X    |
| Conference | KDD      | X    | X    | X    | X    | X    | X    | O    | O    | O    | O    | O    | X    |
| Conference | IJCAI    | X    | X    | X    | X    | X    | X    | -    | X    | O    | O    | O    | X    |
| Conference | CVPR     | X    | X    | X    | X    | X    | X    | -    | X    | -    | -    | -    | X    |
| Journal    | TPAMI    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | P    |
| Journal    | TNNLS    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | P    |
| Journal    | TKDE     | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | P    |
| Journal    | JMLR     | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | X    | P    |

---


---
## 폴더 구조

각 논문의 `.md` 파일은 **핵심 기여에 해당하는 폴더(primary)에 1개** 존재합니다.
한 논문이 여러 축(예: 아키텍처 + 정규화 기법)에 걸칠 경우, 나머지 경로에는 내용을 복사하지 않고 원본을 가리키는 **심볼릭 링크**만 만들어 둡니다.

`CrossCutting/` 폴더는 "채널 전략", "Normalization", "Patching" 등 아키텍처 계열과 무관하게 횡단적으로 묶고 싶을 때 사용하며, 여기에는 링크만 있고 실제 파일은 없습니다.

```
category/
│
├── Time-domain/
│   ├── Transformer/            # Informer, Autoformer, iTransformer, ...
│   ├── MLP-Linear/             # TSMixer, SparseTSF, SOFTS, ...
│   ├── CNN-TCN/                # ModernTCN, ...
│   ├── SSM-Mamba/              # Chimera, Attraos, STanHop-Net, ...
│   └── Generative/
│       ├── Diffusion/          # TimeGrad, Diffusion-TS, mr-Diff, ...
│       └── VAE-GAN/            # TLAE, D3VAE, pTSE, ...
│
├── Freq-domain/
│   ├── FFT/
│   │   └── Architecture/
│   │       ├── Transformer/    # Fredformer, SDformer, ...
│   │       ├── MLP/            # FAN, DERITS, ...
│   │       └── Linear/         # FITS, FRNet, ...
│   ├── LearnableFilter/        # TSLANet, FilterNet, ...
│   └── PeriodicityGuided/      # CycleNet, ...
│
├── Decomposition/
│   ├── TrendSeasonal/          # Autoformer, TimeMixer, Leddam, ...
│   └── Operator/               # PIKAN, CONTIME, ...
│
├── Foundation-LLM/
│   ├── Reprogramming/          # Time-LLM, TEMPO, AutoTimes, ...
│   └── Pretrained-Native/      # MOIRAI, MOMENT, TimesFM, ...
│
├── SelfSupervised/             # TimeSiam, ScalingLaw, ...
│
└── CrossCutting/               ← 심볼릭 링크 전용 (primary 파일 없음)
    ├── ChannelStrategy/
    │   ├── ChannelDependent/   # iTransformer, CARD, TMDM, ...
    │   └── ChannelIndependent/ # FITS, ModernTCN, Time-LLM, ...
    ├── Normalization/          # ST-Norm, DAM, FITS, ...
    ├── Patching/               # PETformer, ElasTST, TimeXer, ...
    └── Loss/                   # CARD, Diffusion-TS, RobustTSF, ...
```


## 파일 규칙

- **파일명**: `<Year>_<Venue>_<ModelName>.md`
- **Primary `.md`**: 각 논문당 1개, primary category 폴더에 위치
- **심볼릭 링크**: 복수 카테고리 해당 시 related 경로에 `ln -s` 링크만 생성
- **PDF**: `.md`와 동일 basename, 같은 폴더에 저장 — **Git 미포함**