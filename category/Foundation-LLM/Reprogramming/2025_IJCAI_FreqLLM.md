---
title: FreqLLM - Frequency-Aware Large Language Models for Time Series Forecasting
authors: Shunnan Wang, Min Gao, et al.
venue: IJCAI 2025
year: 2025
paper_url: https://www.ijcai.org/proceedings/2025/0377.pdf
code_url: https://github.com/biya0105/FreqLLM
tags: [foundation-llm, reprogramming, frequency-domain, semantic-alignment]
primary_category: Foundation-LLM/Reprogramming
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_FreqLLM.pdf
pdf_source: ijcai
---

## TL;DR
FreqLLM integrates frequency-domain semantic alignment into Large Language Models to enhance time series forecasting. By bridging the gap between frequency signals and textual embeddings, it captures multi-scale temporal patterns, achieving improvements of 3.15% and 9.39% over Time-LLM and GPT4TS respectively on benchmark datasets.

## 1. 기존 모델의 한계 / 가설
- LLMs struggle to directly capture frequency-domain patterns in time series
- Gap exists between numerical frequency signals and textual embeddings in LLMs
- Multi-scale temporal patterns require both time-domain and frequency-domain understanding
- Standard LLM reprogramming approaches miss valuable frequency information

## 2. 제안 방법론
### Core Framework
- Integrates frequency-domain semantic alignment into LLM-based forecasting
- Bridges gap between frequency signals and textual embeddings
- Refines prompts for improved time series analysis using frequency information

### Key Components
1. Frequency domain feature extraction from time series
2. Semantic alignment between frequency representations and language embeddings
3. Enhanced prompting mechanism informed by frequency patterns
4. Multi-scale temporal pattern fusion through frequency decomposition

### Innovation
The approach combines the pattern understanding capabilities of LLMs with explicit frequency-domain information, allowing the model to leverage both semantic and spectral characteristics of time series for improved forecasting.

## 3. ETTh1 실험 결과
### 3.1 Setting
- Datasets evaluated: ETTh1, ETTh2 (hourly electricity transformer data)
- Forecast horizons: Standard horizons including 96, 192, 336, 720
- Normalization: Standard normalization
- Comparison baseline: Time-LLM, GPT4TS

※ Note: Specific hyperparameter details not fully available in accessible summaries.

### 3.2 Results
| Comparison | Improvement |
|------------|------------|
| vs Time-LLM | 3.15% |
| vs GPT4TS | 9.39% |

Ablation study on ETTh1 and ETTh2:
- FreqLLM consistently outperforms all baseline ablation settings
- Multi-scale temporal pattern fusion shows clear benefits
- Frequency-domain semantic alignment is critical component

## 4. 주요 기여
- First work systematically integrating frequency-domain information into LLM-based time series forecasting
- Proposes semantic alignment mechanism between frequency signals and language embeddings
- Demonstrates significant improvements over existing LLM baselines (Time-LLM, GPT4TS)
- Shows generalization across multiple benchmark datasets

## 5. 한계 및 후속 연구 아이디어
- Computational cost of frequency analysis combined with LLM inference
- Optimal frequency band selection for different dataset characteristics
- Cross-domain frequency pattern transfer learning

## 6. 참고
- IJCAI Proceedings: https://www.ijcai.org/proceedings/2025/377
- GitHub: https://github.com/biya0105/FreqLLM
- Paper PDF: https://www.ijcai.org/proceedings/2025/0377.pdf
