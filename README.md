# AI - Mini Project 00

> **This is a mini project to get familiar with Prompt Engineering Essentials - Tokenization, decoding strategies, prompt structure patterns, and reasoning techniques (Zero-shot, Few-shot, CoT, ToT) with structured JSON outputs.**

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![OpenAI](https://img.shields.io/badge/LLM-OpenAI-green.svg)](https://platform.openai.com/)
[![Google Gemini](https://img.shields.io/badge/LLM-Gemini-blue.svg)](https://aistudio.google.com/)
[![Groq](https://img.shields.io/badge/LLM-Groq-orange.svg)](https://console.groq.com/)

---

## Quick Start

```bash
cd "Week 01"

# 1. Install
uv sync   # or: pip install -e .

# 2. Environment
cp .env.sample .env
# Fill in: OPENAI_API_KEY, GEMINI_API_KEY, GROQ_API_KEY

# 3. Launch
jupyter notebook
# Open notebooks/00_setup.ipynb first
```

---

## Configuration

### Model Routing

The router (`utils/router.py`) automatically selects model tiers based on technique:

| Provider | General | Strong | Reasoning |
|----------|---------|--------|-----------|
| OpenAI | gpt-4o-mini | gpt-4o | o3-mini |
| Google | gemini-2.0-flash-exp | gemini-2.0-flash-thinking-exp | gemini-3-pro-preview |
| Groq | llama-3.1-8b-instant | llama-3.1-70b-versatile | deepseek-r1-distill-llama-70b |

CoT/ToT techniques are auto-routed to reasoning-tier models.

### config.yaml highlights

- **Retry**: 3 attempts, exponential backoff (0.5-30s), 60s timeout
- **Token management**: tiktoken estimation, context overflow handling (truncate/summarize)
- **Safety**: Prompt injection detection, input sanitization (max 100K chars)
- **Logging**: CSV to `logs/runs.csv` with token counts, latency, cost estimates

---

## Key Components

| Module | Purpose |
|--------|---------|
| `utils/llm_client.py` | Unified `LLMClient` — OpenAI, Gemini, Groq with retry + token reconciliation |
| `utils/prompts.py` | 11 `PromptSpec` templates: skeleton, zero/few-shot, CoT, ToT, JSON, tool calling |
| `utils/router.py` | `pick_model()` — auto-routes techniques to appropriate model tiers |
| `utils/token_utils.py` | Token counting, context fitting, overflow handling |
| `utils/json_utils.py` | JSON extraction from markdown blocks, auto-repair, JSONSchema validation |
| `utils/logging_utils.py` | CSV logging with per-model cost estimation |

---

## API Keys

| Provider | Variable | Get Key |
|----------|----------|---------|
| OpenAI | `OPENAI_API_KEY` | [platform.openai.com](https://platform.openai.com/api-keys) |
| Google AI | `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com/app/apikey) |
| Groq | `GROQ_API_KEY` | [console.groq.com](https://console.groq.com/keys) |
