---
title: Beyond Statistical Analysis - Multimodal Framework for Time Series Forecasting
authors: Jiahong Xiong et al.
venue: IJCAI 2025
year: 2025
paper_url: https://www.ijcai.org/proceedings/2025/0745.pdf
code_url: ""
tags: [foundation-llm, multimodal, time-series-analysis, text-integration]
primary_category: Foundation-LLM/Pretrained-Native
related_categories: []
reports_etth1: true
swept_in: 2026-04-19
pdf_local: ./2025_IJCAI_Multimodal.pdf
pdf_source: ijcai
---

## TL;DR
This paper proposes a multimodal framework that synergistically integrates LLMs with conventional time series forecasting methods. By leveraging both numerical forecasting predictions and textual pattern descriptions from LLMs, the approach constructs a hybrid framework that combines the strengths of statistical methods and language models for enhanced forecasting accuracy.

## 1. 기존 모델의 한계 / 가설
- LLMs alone as direct time series forecasters have shown inconsistent performance
- Conventional statistical/neural forecasting methods miss semantic pattern understanding
- Text-based reasoning can complement numerical pattern recognition
- Multimodal integration can leverage complementary strengths
- Time patterns can be effectively described and reasoned about in natural language
- Synergistic combination of LLM and conventional methods can improve overall performance

## 2. 제안 방법론
### Multimodal Integration Strategy
- Combines two complementary approaches:
  1. **Conventional Methods**: Traditional statistical or neural time series forecasting
  2. **LLM-based Approach**: Text-driven reasoning about temporal patterns and trends

### Framework Components
1. **Numerical Forecasting Branch**: Conventional time series prediction models
2. **Textual Analysis Branch**: LLM-based semantic understanding of temporal patterns
3. **Hybrid Integration**: Synergistic combination to produce final predictions

### Key Approach
- Grounded in conventional forecasting methods' numerical predictions
- Complemented by LLM textual capabilities for pattern reasoning
- Text patterns forecasted by LLMs based on language understanding capabilities
- Joint framework leveraging strengths of both modalities

### Innovation
Rather than replacing conventional methods with LLMs, the approach recognizes that numerical and textual modalities provide complementary information for time series analysis, enabling hybrid predictions that leverage both.

## 3. ETTh1 실험 결果
### 3.1 Setting
- Datasets evaluated:
  - **ETT series**: ETTh1, ETTh2, ETTm1, ETTm2
  - **Standard benchmarks**: Traffic, Electricity, Weather, Illness (ILI)
  - **Exchange rate, Station datasets**
  - **Multimodal datasets**: Time-MMD, From News to Forecast

- Forecast horizons: Standard horizons including 96, 192, 336, 720 steps
- Normalization: Standard normalization protocols
- Baselines: Compared against 8 state-of-the-art time series forecasting methods

※ Specific hyperparameter details (batch size, optimizer, LR, input length) not fully available in accessible sources.

### 3.2 Results
**Performance Characteristics:**
- Competitive performance on ETTh1, ETTh2, ETTm1, ETTm2 datasets
- Evaluation across diverse dataset types (electricity, traffic, weather, illness)
- Multimodal dataset evaluation showing framework effectiveness
- Results indicate model does not consistently achieve top-tier rankings on all datasets

**Key Finding:**
The authors note that performance limitations on attention mechanism network framework aspects suggest opportunities for architectural improvements in attention-based hybrid designs.

## 4. 주요 기여
- Proposes multimodal framework integrating LLMs with conventional TSF methods
- Demonstrates complementarity of numerical and textual pattern understanding
- Addresses limitations of LLM-only or conventional-only approaches
- Comprehensive evaluation across diverse dataset types including multimodal data
- Establishes foundation for hybrid numerical-semantic forecasting approaches

## 5. 한계 및 후속 연구 아이디어
- Performance not consistently top-tier, suggesting architectural refinements needed
- Attention mechanism limitations identified in hybrid framework
- Computational cost of dual-branch (numerical + textual) processing
- Optimal weighting and fusion strategies for multimodal predictions
- Better attention mechanisms for text-numerical integration
- Enhanced prompt engineering for temporal pattern descriptions

## 6. 참고
- IJCAI Proceedings: https://www.ijcai.org/proceedings/2025/745
- Paper PDF: https://www.ijcai.org/proceedings/2025/0745.pdf
- Related survey: "Towards Cross-Modality Modeling for Time Series Analytics" IJCAI 2025
