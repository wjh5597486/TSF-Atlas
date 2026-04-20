# TSF-Atlas

**ETTh1 데이터셋** 기준으로 Time Series Forecasting(TSF) 논문을 체계적으로 수집·분류·정리하는 저장소입니다.

NeurIPS · ICML · ICLR · AAAI · KDD · IJCAI 등 주요 학회와 저널의 TSF 논문을 대상으로, 아키텍처 계열 및 주요 기여 축에 따라 폴더를 구성하고 각 논문마다 `.md` 파일이 작성되어있습니다.


---

## 수집 현황 (Venue × Year)

> 상태 기호: **O(n)** 완료(n=수집 논문 수) · **X** 미시작 · **P** 진행중 · **-** TSF 논문 없음 확인

| Venue   | 2019 | 2020 | 2021   | 2022   | 2023    | 2024    | 2025    | 2026 |
|---------|------|------|--------|--------|---------|---------|---------|------|
| NeurIPS | X    | X    | O(1)   | O(6)   | O(12)   | O(18)   | O(19)   | X    |
| ICML    | X    | X    | O(2)   | O(1)   | O(4)    | O(10)   | O(20)   | X    |
| ICLR    | X    | X    | O(0)   | O(3)   | O(7)    | O(17)   | O(13)   | X    |
| AAAI    | X    | X    | O(4)   | O(3)   | O(3)    | O(6)    | O(19)   | X    |
| KDD     | X    | X    | O(1)   | O(4)   | O(2)    | O(4)    | O(7)    | X    |
| IJCAI   | X    | X    | -      | X      | O(2)    | O(5)    | O(7)    | X    |
| CVPR    | X    | X    | -      | X      | -       | -       | -       | X    |

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

## 카테고리별 빠른 링크

- [Time-domain](./category/Time-domain/)
  - [Transformer](./category/Time-domain/Transformer/) · [MLP-Linear](./category/Time-domain/MLP-Linear/) · [CNN-TCN](./category/Time-domain/CNN-TCN/) · [SSM-Mamba](./category/Time-domain/SSM-Mamba/) · [Graph](./category/Time-domain/Graph/)
  - [Generative/Diffusion](./category/Time-domain/Generative/Diffusion/) · [Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/)
- [Freq-domain](./category/Freq-domain/)
  - [FFT/Architecture/Transformer](./category/Freq-domain/FFT/Architecture/Transformer/) · [FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/) · [FFT/Architecture/Linear](./category/Freq-domain/FFT/Architecture/Linear/) · [FFT/Loss](./category/Freq-domain/FFT/Loss/)
  - [WaveletTransform/Architecture/Transformer](./category/Freq-domain/WaveletTransform/Architecture/Transformer/) · [WaveletTransform/Architecture/MLP](./category/Freq-domain/WaveletTransform/Architecture/MLP/) · [WaveletTransform/Loss](./category/Freq-domain/WaveletTransform/Loss/)
  - [LearnableFilter](./category/Freq-domain/LearnableFilter/) · [PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/)
- [Decomposition](./category/Decomposition/)
  - [TrendSeasonal](./category/Decomposition/TrendSeasonal/) · [MultiScale](./category/Decomposition/MultiScale/) · [Operator](./category/Decomposition/Operator/)
- [Foundation-LLM](./category/Foundation-LLM/)
  - [Reprogramming](./category/Foundation-LLM/Reprogramming/) · [Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/)
- [SelfSupervised](./category/SelfSupervised/)
- [CrossCutting](./category/CrossCutting/) — 심볼릭 링크 전용
  - [ChannelIndependent](./category/CrossCutting/ChannelStrategy/ChannelIndependent/) · [ChannelDependent](./category/CrossCutting/ChannelStrategy/ChannelDependent/)
  - [Normalization](./category/CrossCutting/Normalization/) · [Patching](./category/CrossCutting/Patching/) · [Loss](./category/CrossCutting/Loss/)

## 파일 규칙

- **파일명**: `<Year>_<Venue>_<ModelName>.md`
- **Primary `.md`**: 각 논문당 1개, primary category 폴더에 위치
- **심볼릭 링크**: 복수 카테고리 해당 시 related 경로에 `ln -s` 링크만 생성
- **PDF**: `.md`와 동일 basename, 같은 폴더에 저장 — **Git 미포함**