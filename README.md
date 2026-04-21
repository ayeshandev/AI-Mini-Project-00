# Week 01: Prompt Engineering Essentials

> **Tokenization, decoding strategies, prompt structure patterns, and reasoning techniques (Zero-shot, Few-shot, CoT, ToT) with structured JSON outputs.**

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![OpenAI](https://img.shields.io/badge/LLM-OpenAI-green.svg)](https://platform.openai.com/)
[![Google Gemini](https://img.shields.io/badge/LLM-Gemini-blue.svg)](https://aistudio.google.com/)
[![Groq](https://img.shields.io/badge/LLM-Groq-orange.svg)](https://console.groq.com/)

---

## What You'll Build

- Multi-provider LLM client (OpenAI, Google Gemini, Groq) with automatic retry and token tracking
- 11 reusable prompt templates (skeleton, zero-shot, few-shot, CoT, ToT, JSON extraction, tool calling)
- Automatic model routing (reasoning techniques routed to reasoning-tier models)
- Token estimation, cost tracking, and CSV logging

---

## Notebooks (run in order)

| # | Notebook | Focus |
|---|----------|-------|
| 00 | `00_setup.ipynb` | Verify API keys, test connectivity, initialize logging |
| 01 | `01_decoding_and_tokens.ipynb` | Tokenization (tiktoken), temperature sweeps, max_tokens control |
| 02 | `02_prompt_structure_patterns.ipynb` | Unstructured vs skeleton pattern, zero-shot classification |
| 03 | `03_zero_few_cot_tot.ipynb` | Zero-shot, Few-shot, Chain-of-Thought, Tree-of-Thought |
| 04 | `04_structured_outputs_json_schema.ipynb` | JSON extraction, schema validation, auto-repair, Pydantic models |

---

## Project Structure

```
Week 01/
├── config/
│   ├── config.yaml              # Providers, defaults, retry, safety, logging
│   └── models.yaml              # Model tiers per provider (general/strong/reason)
├── data/
│   └── tiny_corpus/             # CloudSync Pro sample docs
│       ├── 01_policy.md         # Remote work policy
│       ├── 02_product_specs.md  # Product specifications
│       └── 03_faq.md            # FAQ document
├── logs/
│   └── runs.csv                 # LLM call logs (tokens, latency, cost)
├── notebooks/
│   ├── 00_setup.ipynb
│   ├── 01_decoding_and_tokens.ipynb
│   ├── 02_prompt_structure_patterns.ipynb
│   ├── 03_zero_few_cot_tot.ipynb
│   └── 04_structured_outputs_json_schema.ipynb
├── utils/
│   ├── config_loader.py         # YAML config with dot-notation access
│   ├── llm_client.py            # Multi-provider LLM abstraction + retry
│   ├── prompts.py               # 11 prompt templates (PromptSpec catalog)
│   ├── router.py                # Auto model selection by technique
│   ├── token_utils.py           # Token counting, context overflow handling
│   ├── json_utils.py            # JSON extraction, repair, schema validation
│   └── logging_utils.py         # CSV logging with cost estimation
├── .env.sample                  # API key template
├── pyproject.toml               # Dependencies (Python 3.11+)
└── uv.lock
```

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

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **"No module named X"** | Run `uv sync` or `pip install -e .` from `Week 01/` |
| **"API key not found"** | Check `.env` exists and has the right variable names; restart kernel |
| **Groq rate limits** | Groq free tier has low RPM — add delays between calls or switch to OpenAI |
| **Token count mismatch** | Estimated vs actual tokens may differ; this is expected and logged in `runs.csv` |
| **Slow responses on CoT/ToT** | Reasoning models are slower by design — reduce `max_tokens` if needed |
| **Import errors in Jupyter** | Ensure you launched Jupyter from the `Week 01/` directory |

---

**Contact:** hi@zuucrew.ai · *Built with Zuu Crew*
