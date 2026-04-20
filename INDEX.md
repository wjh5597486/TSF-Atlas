# INDEX — ETTh1 TSF Papers

TSF-Atlas에 정리된 논문 전체 목록.
새 논문을 추가하거나 카테고리 심볼릭 링크를 만들 때마다 이 표를 갱신할 것.
작성 규칙은 [CLAUDE.md](./CLAUDE.md) §6 참고.

## 통계
- 총 논문 수: 200
- ETTh1 reports: 171 / 미보고: 15 (PIKAN, RATD, MOMENT, FSNet, LLMTime, TSDiff, GPT4MTS, Time-MoE, LinOSS, P-sLSTM, HDT, TiRex, Sundial, WaveToken, RLMC, DSARF)
- (Venue, Year) 완주: 26 (NeurIPS 2021, ICML 2021, ICLR 2022, ICLR 2024, KDD 2024, NeurIPS 2024, IJCAI 2024, AAAI 2023, KDD 2023, IJCAI 2023, ICML 2024, CVPR 2024, ICLR 2023, NeurIPS 2023, ICML 2023, AAAI 2024, ICLR 2025, AAAI 2025, NeurIPS 2025, ICML 2025, KDD 2025, IJCAI 2025, ICML 2022, AAAI 2022, KDD 2022, **AAAI 2021**) — 상세는 [COVERAGE.md](./COVERAGE.md)
- 마지막 업데이트: 2026-04-20

## 논문 목록

| # | Model | Year | Venue | Primary Category | Related Categories | ETTh1 | ETTh1 best MSE (horizon) |
|---|-------|------|-------|------------------|--------------------|-------|--------------------------|
| 1 | Informer | 2021 | AAAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2021_AAAI_Informer.md) | - | ✓ | 1.161 (336) |
| 2 | TLAE | 2021 | AAAI | [Time-domain/Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/2021_AAAI_TLAE.md) | - | ✓ | see paper |
| 3 | DSARF | 2021 | AAAI | [Time-domain/Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/2021_AAAI_DSARF.md) | - | ✗ | — |
| 4 | DynamicGaussianMixture | 2021 | AAAI | [Time-domain/Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/2021_AAAI_DynamicGaussianMixture.md) | - | ✓ | 1.168 (336) |
| 4.5 | TimeGrad | 2021 | ICML | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2021_ICML_TimeGrad.md) | - | ✓ | see paper |
| 4.6 | VarianceReduced | 2021 | ICML | [CrossCutting/Loss](./category/CrossCutting/Loss/2021_ICML_VarianceReduced.md) | - | ✗ | — |
| 5 | iTransformer | 2024 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICLR_iTransformer.md) | ChannelDependent | ✓ | 0.386 (96) |
| 1 | TimeMixer | 2024 | ICLR | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2024_ICLR_TimeMixer.md) | MLP-Linear, MultiScale | ✓ | 0.447 (avg) |
| 2 | ModernTCN | 2024 | ICLR | [Time-domain/CNN-TCN](./category/Time-domain/CNN-TCN/2024_ICLR_ModernTCN.md) | ChannelIndependent | ✓ | 0.404 (avg) |
| 3 | FITS | 2024 | ICLR | [Freq-domain/FFT/Architecture/Linear](./category/Freq-domain/FFT/Architecture/Linear/2024_ICLR_FITS.md) | ChannelIndependent, Normalization | ✓ | see Table 4 |
| 4 | Diffusion-TS | 2024 | ICLR | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_ICLR_Diffusion-TS.md) | Loss, TrendSeasonal | ✓ | see paper |
| 5 | Pathformer | 2024 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICLR_Pathformer.md) | MultiScale | ✓ | see Table 1 |
| 6 | mr-Diff | 2024 | ICLR | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_ICLR_mr-Diff.md) | TrendSeasonal, MultiScale | ✓ | see paper |
| 7 | TMDM | 2024 | ICLR | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_ICLR_TMDM.md) | ChannelDependent | ✓ | see paper |
| 8 | Time-LLM | 2024 | ICLR | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2024_ICLR_Time-LLM.md) | ChannelIndependent | ✓ | see Table 2 |
| 9 | MG-TSD | 2024 | ICLR | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_ICLR_MG-TSD.md) | MultiScale | ✓ | see paper |
| 10 | DAM | 2024 | ICLR | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_ICLR_DAM.md) | Normalization | ✓ | see Table 2 |
| 11 | TEMPO | 2024 | ICLR | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2024_ICLR_TEMPO.md) | TrendSeasonal | ✓ | see paper |
| 12 | RobustTSF | 2024 | ICLR | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_ICLR_RobustTSF.md) | Loss | ✓ | see paper |
| 13 | STanHop-Net | 2024 | ICLR | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2024_ICLR_STanHop-Net.md) | ChannelDependent | ✓ | see paper |
| 14 | CARD | 2024 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICLR_CARD.md) | ChannelDependent, Loss | ✓ | 0.443 (avg) |
| 15 | PETformer | 2024 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICLR_PETformer.md) | Patching | ✓ | see paper |
| 16 | PIKAN | 2024 | ICLR | [Decomposition/Operator](./category/Decomposition/Operator/2024_ICLR_PIKAN.md) | Loss | ✗ | — |
| 17 | GPHT | 2024 | KDD | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_KDD_GPHT.md) | - | ✓ | see paper |
| 18 | Fredformer | 2024 | KDD | [Freq-domain/FFT/Architecture/Transformer](./category/Freq-domain/FFT/Architecture/Transformer/2024_KDD_Fredformer.md) | Normalization | ✓ | see paper |
| 19 | FRNet | 2024 | KDD | [Freq-domain/FFT/Architecture/Linear](./category/Freq-domain/FFT/Architecture/Linear/2024_KDD_FRNet.md) | TrendSeasonal | ✓ | see paper |
| 20 | SOLID | 2024 | KDD | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_KDD_SOLID.md) | Normalization | ✓ | see paper |
| 21 | TimeXer | 2024 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_NeurIPS_TimeXer.md) | Patching | ✓ | see Table 1 |
| 22 | SOFTS | 2024 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_NeurIPS_SOFTS.md) | ChannelDependent | ✓ | see Table 1 |
| 23 | CycleNet | 2024 | NeurIPS | [Freq-domain/PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/2024_NeurIPS_CycleNet.md) | TrendSeasonal | ✓ | see Table 1 |
| 24 | Chimera | 2024 | NeurIPS | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2024_NeurIPS_Chimera.md) | - | ✓ | see paper |
| 25 | Attraos | 2024 | NeurIPS | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2024_NeurIPS_Attraos.md) | - | ✓ | see paper |
| 26 | CCM | 2024 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_NeurIPS_CCM.md) | ChannelDependent | ✓ | see Table 1 |
| 27 | ElasTST | 2024 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_NeurIPS_ElasTST.md) | Patching | ✓ | see paper |
| 28 | TPGN | 2024 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_NeurIPS_TPGN.md) | - | ✓ | see paper |
| 29 | RATD | 2024 | NeurIPS | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_NeurIPS_RATD.md) | - | ✗ | — |
| 30 | FilterNet | 2024 | NeurIPS | [Freq-domain/LearnableFilter](./category/Freq-domain/LearnableFilter/2024_NeurIPS_FilterNet.md) | - | ✓ | see paper |
| 31 | UniTS | 2024 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_NeurIPS_UniTS.md) | - | ✓ | see paper |
| 32 | TTMs | 2024 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_NeurIPS_TTMs.md) | - | ✓ | see paper |
| 33 | FAN | 2024 | NeurIPS | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2024_NeurIPS_FAN.md) | Normalization | ✓ | see paper |
| 34 | S3 | 2024 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_NeurIPS_S3.md) | - | ✓ | see paper |
| 35 | DeformableTST | 2024 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_NeurIPS_DeformableTST.md) | - | ✓ | see paper |
| 36 | AreLLMsUseful | 2024 | NeurIPS | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2024_NeurIPS_AreLLMsUseful.md) | - | ✓ | see paper |
| 37 | AutoTimes | 2024 | NeurIPS | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2024_NeurIPS_AutoTimes.md) | - | ✓ | see paper |
| 38 | ScalingLaw | 2024 | NeurIPS | [SelfSupervised](./category/SelfSupervised/2024_NeurIPS_ScalingLaw.md) | - | ✓ | see paper |
| 39 | SDformer | 2024 | IJCAI | [Freq-domain/FFT/Architecture/Transformer](./category/Freq-domain/FFT/Architecture/Transformer/2024_IJCAI_SDformer.md) | - | ✓ | see paper |
| 40 | VCformer | 2024 | IJCAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_IJCAI_VCformer.md) | ChannelDependent | ✓ | see paper |
| 41 | DERITS | 2024 | IJCAI | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2024_IJCAI_DERITS.md) | Normalization | ✓ | see paper |
| 42 | Skip-Timeformer | 2024 | IJCAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_IJCAI_Skip-Timeformer.md) | - | ✓ | see paper |
| 43 | DIAN | 2024 | IJCAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_IJCAI_DIAN.md) | ChannelDependent | ✓ | see paper |
| 44 | Autoformer | 2021 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2021_NeurIPS_Autoformer.md) | TrendSeasonal | ✓ | 0.371 (96) |
| 45 | ST-Norm | 2021 | KDD | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2021_KDD_ST-Norm.md) | Normalization | ✓ | see paper |
| 46 | SCINet | 2022 | NeurIPS | [Time-domain/CNN-TCN](./category/Time-domain/CNN-TCN/2022_NeurIPS_SCINet.md) | - | ✓ | see paper |
| 47 | FiLM | 2022 | NeurIPS | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2022_NeurIPS_FiLM.md) | - | ✓ | see paper |
| 48 | Non-stationary Transformer | 2022 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2022_NeurIPS_Non-stationary-Transformer.md) | Normalization | ✓ | see paper |
| 49 | LaST | 2022 | NeurIPS | [SelfSupervised](./category/SelfSupervised/2022_NeurIPS_LaST.md) | TrendSeasonal | ✓ | see paper |
| 50 | TPGNN | 2022 | NeurIPS | [Time-domain/Graph](./category/Time-domain/Graph/2022_NeurIPS_TPGNN.md) | - | ✓ | see paper |
| 51 | D3VAE | 2022 | NeurIPS | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2022_NeurIPS_D3VAE.md) | - | ✓ | see paper |
| 52 | FEDformer | 2022 | ICML | [Freq-domain/FFT/Architecture/Transformer](./category/Freq-domain/FFT/Architecture/Transformer/2022_ICML_FEDformer.md) | Decomposition/TrendSeasonal | ✓ | 0.360 (96) |
| 53 | Pyraformer | 2022 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2022_ICLR_Pyraformer.md) | MultiScale | ✓ | see paper |
| 54 | CoST | 2022 | ICLR | [SelfSupervised](./category/SelfSupervised/2022_ICLR_CoST.md) | TrendSeasonal, Loss | ✓ | see paper |
| 55 | RevIN | 2022 | ICLR | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2022_ICLR_RevIN.md) | Normalization | ✓ | see paper |
| 56 || TSMixer | 2023 | KDD | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_KDD_TSMixer.md) | ChannelIndependent, ChannelDependent | ✓ | see paper |
| 57 || SBT | 2023 | KDD | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_KDD_SBT.md) | - | ✓ | see paper |
| 58 || SMARTformer | 2023 | IJCAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_IJCAI_SMARTformer.md) | - | ✓ | see paper |
| 59 || pTSE | 2023 | IJCAI | [Time-domain/Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/2023_IJCAI_pTSE.md) | - | ✓ | see paper |
| 60 || SAMformer | 2024 | ICML | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICML_SAMformer.md) | Loss | ✓ | see paper |
| 61 || CATS | 2024 | ICML | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_ICML_CATS.md) | - | ✓ | see paper |
| 62 || SparseTSF | 2024 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_ICML_SparseTSF.md) | TrendSeasonal | ✓ | see paper |
| 63 || LinearAnalysis | 2024 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_ICML_LinearAnalysis.md) | - | ✓ | see paper |
| 64 || Leddam | 2024 | ICML | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2024_ICML_Leddam.md) | - | ✓ | see paper |
| 65 || MOIRAI | 2024 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_ICML_MOIRAI.md) | - | ✓ | see paper |
| 66 || MOMENT | 2024 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_ICML_MOMENT.md) | - | ✗ | — |
| 67 || TimesFM | 2024 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2024_ICML_TimesFM.md) | - | ✓ | see paper |
| 68 || TSLANet | 2024 | ICML | [Freq-domain/LearnableFilter](./category/Freq-domain/LearnableFilter/2024_ICML_TSLANet.md) | - | ✓ | see paper |
| 69 || TimeSiam | 2024 | ICML | [SelfSupervised](./category/SelfSupervised/2024_ICML_TimeSiam.md) | - | ✓ | see paper |
| 70 || PatchTST | 2023 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_ICLR_PatchTST.md) | Patching, ChannelIndependent | ✓ | 0.370 (96) |
| 71 || TimesNet | 2023 | ICLR | [Freq-domain/PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/2023_ICLR_TimesNet.md) | CNN-TCN | ✓ | 0.384 (96) |
| 72 || Crossformer | 2023 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_ICLR_Crossformer.md) | ChannelDependent | ✓ | 0.404 (96) |
| 73 || MICN | 2023 | ICLR | [Time-domain/CNN-TCN](./category/Time-domain/CNN-TCN/2023_ICLR_MICN.md) | MultiScale | ✓ | 0.383 (96) |
| 74 || Scaleformer | 2023 | ICLR | [Decomposition/MultiScale](./category/Decomposition/MultiScale/2023_ICLR_Scaleformer.md) | - | ✓ | see paper |
| 75 || SpaceTime | 2023 | ICLR | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2023_ICLR_SpaceTime.md) | - | ✓ | see paper |
| 76 || FSNet | 2023 | ICLR | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_ICLR_FSNet.md) | - | ✗ | — |
| 77 || GPT4TS | 2023 | NeurIPS | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2023_NeurIPS_GPT4TS.md) | Normalization | ✓ | 0.376 (96) |
| 78 || Koopa | 2023 | NeurIPS | [Decomposition/Operator](./category/Decomposition/Operator/2023_NeurIPS_Koopa.md) | - | ✓ | see paper |
| 79 || FreTS | 2023 | NeurIPS | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2023_NeurIPS_FreTS.md) | ChannelDependent | ✓ | see paper |
| 80 || FourierGNN | 2023 | NeurIPS | [Time-domain/Graph](./category/Time-domain/Graph/2023_NeurIPS_FourierGNN.md) | - | ✓ | see paper |
| 81 || BasisFormer | 2023 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_NeurIPS_BasisFormer.md) | - | ✓ | see paper |
| 82 || CrossGNN | 2023 | NeurIPS | [Time-domain/Graph](./category/Time-domain/Graph/2023_NeurIPS_CrossGNN.md) | - | ✓ | see paper |
| 83 || WITRAN | 2023 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_NeurIPS_WITRAN.md) | - | ✓ | see paper |
| 84 || SAN | 2023 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_NeurIPS_SAN.md) | Normalization | ✓ | see paper |
| 84 || OneNet | 2023 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_NeurIPS_OneNet.md) | - | ✓ | see paper |
| 85 || SimMTM | 2023 | NeurIPS | [SelfSupervised](./category/SelfSupervised/2023_NeurIPS_SimMTM.md) | - | ✓ | see paper |
| 86 || LLMTime | 2023 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2023_NeurIPS_LLMTime.md) | - | ✗ | — |
| 87 || TSDiff | 2023 | NeurIPS | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2023_NeurIPS_TSDiff.md) | - | ✗ | — |
| 88 || DeepTime | 2023 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_ICML_DeepTime.md) | - | ✓ | see paper |
| 89 || TimeDiff | 2023 | ICML | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2023_ICML_TimeDiff.md) | - | ✓ | see paper |
| 90 || CounTS | 2023 | ICML | [Time-domain/Transformer](./category/Time-domain/Transformer/2023_ICML_CounTS.md) | - | ✓ | see paper |
| 91 || FeatureP | 2023 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_ICML_FeatureP.md) | - | ✓ | see paper |
| 92 || TS2Vec | 2022 | AAAI | [SelfSupervised](./category/SelfSupervised/2022_AAAI_TS2Vec.md) | - | ✓ | forecasting benchmark results |
| 93 || CATN | 2022 | AAAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2022_AAAI_CATN.md) | - | ✓ | see paper |
| 94 || RLMC | 2022 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2022_AAAI_RLMC.md) | - | ✗ | — |
| 95 || DLinear | 2023 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_AAAI_DLinear.md) | TrendSeasonal, ChannelIndependent, Normalization | ✓ | 0.356 (96) |
| 96 || DishTS | 2023 | AAAI | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2023_AAAI_DishTS.md) | Normalization | ✓ | 0.386 (96) |
| 97 || NHITS | 2023 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2023_AAAI_NHITS.md) | ChannelIndependent | ✓ | 0.335 (96) |
| 98 || MSGNet | 2024 | AAAI | [Time-domain/Graph](./category/Time-domain/Graph/2024_AAAI_MSGNet.md) | MultiScale, ChannelDependent | ✓ | see paper |
| 99 || HDMixer | 2024 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_AAAI_HDMixer.md) | Patching, ChannelDependent | ✓ | see paper |
| 100 || DAN | 2024 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2024_AAAI_DAN.md) | - | ✓ | see paper |
| 101 || LDT | 2024 | AAAI | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2024_AAAI_LDT.md) | - | ✓ | see paper |
| 102 || GPT4MTS | 2024 | AAAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2024_AAAI_GPT4MTS.md) | - | ✗ | — |
| 103 || HTV-Trans | 2024 | AAAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2024_AAAI_HTV-Trans.md) | Normalization | ✓ | see paper |
| 104 || TimeMixerPP | 2025 | ICLR | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2025_ICLR_TimeMixerPP.md) | MLP-Linear, MultiScale | ✓ | see Table 2 |
| 105 || Time-MoE | 2025 | ICLR | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICLR_Time-MoE.md) | - | ✗ | — |
| 106 || Timer-XL | 2025 | ICLR | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICLR_Timer-XL.md) | - | ✓ | see paper |
| 107 || ScalingLaw-TSFM | 2025 | ICLR | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICLR_ScalingLaw-TSFM.md) | - | ✓ | see paper |
| 108 || SimpleTM | 2025 | ICLR | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICLR_SimpleTM.md) | - | ✓ | see paper |
| 109 || ICTSP | 2025 | ICLR | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICLR_ICTSP.md) | - | ✓ | see paper |
| 110 || FreDF | 2025 | ICLR | [Freq-domain/FFT/Architecture/Linear](./category/Freq-domain/FFT/Architecture/Linear/2025_ICLR_FreDF.md) | Loss | ✓ | see paper |
| 111 || TimeKAN | 2025 | ICLR | [Freq-domain/PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/2025_ICLR_TimeKAN.md) | TrendSeasonal | ✓ | see paper |
| 112 || CoMRes | 2025 | ICLR | [SelfSupervised](./category/SelfSupervised/2025_ICLR_CoMRes.md) | MultiScale | ✓ | see paper |
| 113 | | TS-LIF | 2025 | ICLR | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_ICLR_TS-LIF.md) | - | ✓ | see paper |
| 114 | | DynDiff | 2025 | ICLR | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_ICLR_DynDiff.md) | - | ✓ | see paper |
| 115 | | LinOSS | 2025 | ICLR | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_ICLR_LinOSS.md) | - | ✗ | — |
| 116 | | DSOF | 2025 | ICLR | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICLR_DSOF.md) | - | ✓ | see paper |
| 117 | | WPMixer | 2025 | AAAI | [Freq-domain/WaveletTransform/Architecture/MLP](./category/Freq-domain/WaveletTransform/Architecture/MLP/2025_AAAI_WPMixer.md) | Patching | ✓ | 0.347 (96) |
| 118 | | PatchMLP | 2025 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_AAAI_PatchMLP.md) | Patching, TrendSeasonal | ✓ | 0.438 (avg) |
| 119 | | xPatch | 2025 | AAAI | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2025_AAAI_xPatch.md) | Patching, Loss | ✓ | 0.428 (avg) |
| 120 | | ARMD | 2025 | AAAI | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_AAAI_ARMD.md) | - | ✓ | see paper |
| 121 | | TAFAS | 2025 | AAAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_AAAI_TAFAS.md) | Normalization | ✓ | see paper |
| 122 | | CALF | 2025 | AAAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_AAAI_CALF.md) | - | ✓ | 0.432 (avg) |
| 123 | | TimeCMA | 2025 | AAAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_AAAI_TimeCMA.md) | - | ✓ | see paper |
| 124 | | ChatTime | 2025 | AAAI | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_AAAI_ChatTime.md) | - | ✓ | see paper |
| 125 | | FilterTS | 2025 | AAAI | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2025_AAAI_FilterTS.md) | ChannelDependent | ✓ | 0.433 (avg) |
| 126 | | Affirm | 2025 | AAAI | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_AAAI_Affirm.md) | LearnableFilter | ✓ | see paper |
| 127 | | Amplifier | 2025 | AAAI | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2025_AAAI_Amplifier.md) | TrendSeasonal | ✓ | 0.430 (avg) |
| 128 | | P-sLSTM | 2025 | AAAI | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_AAAI_PsLSTM.md) | Patching, ChannelIndependent | ✗ | — |
| 129 | | IRPA | 2025 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_AAAI_IRPA.md) | Patching, TrendSeasonal | ✓ | see paper |
| 130 | | TimePFN | 2025 | AAAI | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_AAAI_TimePFN.md) | - | ✓ | see paper |
| 131 | | Times2D | 2025 | AAAI | [Freq-domain/PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/2025_AAAI_Times2D.md) | TrendSeasonal | ✓ | see paper |
| 132 | | Apollo-Forecast | 2025 | AAAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_AAAI_ApolloForecast.md) | - | ✓ | see paper |
| 133 | | HCAN | 2025 | AAAI | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_AAAI_HCAN.md) | Loss | ✓ | see paper |
| 134 | | WaveletMixer | 2025 | AAAI | [Freq-domain/WaveletTransform/Architecture/MLP](./category/Freq-domain/WaveletTransform/Architecture/MLP/2025_AAAI_WaveletMixer.md) | - | ✓ | see paper |
| 135 | | HDT | 2025 | AAAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_AAAI_HDT.md) | - | ✗ | — |
| 136 | | SRSNet | 2025 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_NeurIPS_SRSNet.md) | Patching, ChannelIndependent | ✓ | see paper |
| 137 | | TFPS | 2025 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_NeurIPS_TFPS.md) | Patching | ✓ | see paper |
| 138 | | MoFo | 2025 | NeurIPS | [Freq-domain/PeriodicityGuided](./category/Freq-domain/PeriodicityGuided/2025_NeurIPS_MoFo.md) | - | ✓ | see paper |
| 139 | | OLinear | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_OLinear.md) | ChannelDependent | ✓ | see paper |
| 140 | | SEMPO | 2025 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_NeurIPS_SEMPO.md) | - | ✓ | see paper |
| 141 | | SelectiveLearning | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_SelectiveLearning.md) | Loss | ✓ | see paper |
| 142 | | PIR | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_PIR.md) | - | ✓ | see paper |
| 143 | | Time-o1 | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_Time-o1.md) | Loss | ✓ | see paper |
| 144 | | AliO | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_AliO.md) | Loss | ✓ | see paper |
| 145 | | DecompNet | 2025 | NeurIPS | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2025_NeurIPS_DecompNet.md) | - | ✓ | see paper |
| 146 | | ImplicitForecaster | 2025 | NeurIPS | [Freq-domain/FFT/Architecture/MLP](./category/Freq-domain/FFT/Architecture/MLP/2025_NeurIPS_ImplicitForecaster.md) | - | ✓ | see paper |
| 147 | | TimeEmb | 2025 | NeurIPS | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_NeurIPS_TimeEmb.md) | Normalization | ✓ | see paper |
| 148 | | TimePerceiver | 2025 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_NeurIPS_TimePerceiver.md) | - | ✓ | see paper |
| 149 | | DMMV | 2025 | NeurIPS | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_NeurIPS_DMMV.md) | TrendSeasonal | ✓ | see paper |
| 150 | | TimeXL | 2025 | NeurIPS | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_NeurIPS_TimeXL.md) | - | ✓ | see paper |
| 151 | | TS-RAG | 2025 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_NeurIPS_TS-RAG.md) | - | ✓ | see paper |
| 152 | | Toto | 2025 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_NeurIPS_Toto.md) | - | ✓ | see paper |
| 153 | | PIH | 2025 | NeurIPS | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_NeurIPS_PIH.md) | - | ✓ | see paper |
| 154 | | TiRex | 2025 | NeurIPS | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_NeurIPS_TiRex.md) | - | ✗ | — |
| 155 | | TQNet | 2025 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICML_TQNet.md) | ChannelDependent | ✓ | see paper |
| 156 | | TimePro | 2025 | ICML | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_ICML_TimePro.md) | ChannelDependent | ✓ | see paper |
| 157 | | TimeBridge | 2025 | ICML | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_ICML_TimeBridge.md) | ChannelDependent, Normalization | ✓ | see paper |
| 158 | | K²VAE | 2025 | ICML | [Time-domain/Generative/VAE-GAN](./category/Time-domain/Generative/VAE-GAN/2025_ICML_K2VAE.md) | Operator | ✓ | see paper |
| 159 | | NsDiff | 2025 | ICML | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_ICML_NsDiff.md) | - | ✓ | see paper |
| 160 | | TimeFilter | 2025 | ICML | [Time-domain/Graph](./category/Time-domain/Graph/2025_ICML_TimeFilter.md) | Patching, ChannelDependent | ✓ | see paper |
| 161 | | TimeBase | 2025 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICML_TimeBase.md) | PeriodicityGuided | ✓ | see paper |
| 162 | | RAFT | 2025 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICML_RAFT.md) | - | ✓ | see paper |
| 163 | | LightGTS | 2025 | ICML | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_ICML_LightGTS.md) | PeriodicityGuided | ✓ | see paper |
| 164 | | MorphingFlow | 2025 | ICML | [Decomposition/Operator](./category/Decomposition/Operator/2025_ICML_MorphingFlow.md) | Normalization | ✓ | see paper |
| 165 | | PSLoss | 2025 | ICML | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_ICML_PSLoss.md) | Loss, Patching | ✓ | see paper |
| 166 | | ELF | 2025 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICML_ELF.md) | - | ✓ | see paper |
| 167 | | SKOLR | 2025 | ICML | [Decomposition/Operator](./category/Decomposition/Operator/2025_ICML_SKOLR.md) | SSM-Mamba | ✓ | see paper |
| 168 | | Sundial | 2025 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICML_Sundial.md) | - | ✗ | — |
| 169 | | WaveToken | 2025 | ICML | [Freq-domain/WaveletTransform/Architecture/MLP](./category/Freq-domain/WaveletTransform/Architecture/MLP/2025_ICML_WaveToken.md) | Foundation-LLM/Pretrained-Native | ✗ | — |
| 170 | | TimesFM-ICF | 2025 | ICML | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_ICML_TimesFM-ICF.md) | - | ✓ | see paper |
| 171 | | TimeVLM | 2025 | ICML | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_ICML_TimeVLM.md) | - | ✓ | see paper |
| 172 | | CNDiff | 2025 | ICML | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_ICML_CNDiff.md) | - | ✓ | see paper |
| 173 | | TimeMCL | 2025 | ICML | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_ICML_TimeMCL.md) | Loss | ✗ | — |
| 174 | | TimeDART | 2025 | ICML | [SelfSupervised](./category/SelfSupervised/2025_ICML_TimeDART.md) | Generative/Diffusion | ✓ | see paper |
| 175 | | DUET | 2025 | KDD | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_KDD_DUET.md) | ChannelDependent | ✓ | 0.352 (96) |
| 176 | | TimeCapsule | 2025 | KDD | [Decomposition/MultiScale](./category/Decomposition/MultiScale/2025_KDD_TimeCapsule.md) | MLP-Linear | ✓ | 0.362 (96) |
| 177 | | SDE | 2025 | KDD | [Time-domain/SSM-Mamba](./category/Time-domain/SSM-Mamba/2025_KDD_SDE.md) | - | ✓ | see paper |
| 178 | | IN-Flow | 2025 | KDD | [CrossCutting/Normalization](./category/CrossCutting/Normalization/2025_KDD_IN-Flow.md) | - | ✓ | see paper |
| 179 | | ST-MTM | 2025 | KDD | [SelfSupervised](./category/SelfSupervised/2025_KDD_ST-MTM.md) | TrendSeasonal | ✓ | see paper |
| 180 | | DistPred | 2025 | KDD | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_KDD_DistPred.md) | Loss | ✓ | 0.288 (96) |
| 181 | | CrossLinear | 2025 | KDD | [Time-domain/MLP-Linear](./category/Time-domain/MLP-Linear/2025_KDD_CrossLinear.md) | Patching | ✓ | see paper |
| 182 | | FreEformer | 2025 | IJCAI | [Freq-domain/FFT/Architecture/Transformer](./category/Freq-domain/FFT/Architecture/Transformer/2025_IJCAI_FreEformer.md) | - | ✓ | see paper |
| 183 | | FreqLLM | 2025 | IJCAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_IJCAI_FreqLLM.md) | - | ✓ | see paper |
| 184 | | LLM-TPF | 2025 | IJCAI | [Foundation-LLM/Reprogramming](./category/Foundation-LLM/Reprogramming/2025_IJCAI_LLM-TPF.md) | - | ✓ | see paper |
| 185 | | SoP | 2025 | IJCAI | [Time-domain/Transformer](./category/Time-domain/Transformer/2025_IJCAI_SoP.md) | - | ✓ | see paper |
| 186 | | CDPM | 2025 | IJCAI | [Decomposition/TrendSeasonal](./category/Decomposition/TrendSeasonal/2025_IJCAI_CDPM.md) | - | ✓ | see paper |
| 187 | | TCDM | 2025 | IJCAI | [Time-domain/Generative/Diffusion](./category/Time-domain/Generative/Diffusion/2025_IJCAI_TCDM.md) | - | ✓ | see paper |
| 188 | | Multimodal | 2025 | IJCAI | [Foundation-LLM/Pretrained-Native](./category/Foundation-LLM/Pretrained-Native/2025_IJCAI_Multimodal.md) | - | ✓ | see paper |
| 189 | | AMRC | 2025 | NeurIPS | [CrossCutting/Loss](./category/SelfSupervised/2025_NeurIPS_AMRC.md) | - | ✓ | 0.372 (96) |
| 190 | Quatformer | 2022 | KDD | [Time-domain/Transformer](./category/Time-domain/Transformer/2022_KDD_Quatformer.md) | - | ✓ | see paper |
| 191 | TARNet | 2022 | KDD | [Time-domain/Transformer](./category/Time-domain/Transformer/2022_KDD_TARNet.md) | - | ✓ | see paper |
| 192 | STEP | 2022 | KDD | [Time-domain/Graph](./category/Time-domain/Graph/2022_KDD_STEP.md) | - | ✓ | see paper |
| 193 | EvoMTS | 2022 | KDD | [Time-domain/Graph](./category/Time-domain/Graph/2022_KDD_EvoMTS.md) | MultiScale | ✓ | see paper |

> 컬럼 설명
> - **Model**: 논문에서 제시한 모델명 (없으면 `<1저자>_etal`)
> - **Primary Category**: 실제 `.md` 파일이 위치한 경로 (`category/...`)
> - **Related Categories**: 심볼릭 링크로 걸린 다른 경로들 (콤마 구분, 없으면 `-`)
> - **ETTh1**: `✓` = 결과 보고, `✗` = 미보고
> - **ETTh1 best MSE (horizon)**: 보고한 horizon 중 가장 낮은 MSE와 그 horizon (예: `0.370 (96)`)

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
