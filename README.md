# TokenWatch

> Track, optimize, and control your AI API spending. Free and open-source (MIT).

[![Version](https://img.shields.io/badge/version-1.0.0-blue)](https://github.com/vedantsingh60/tokenwatch/releases)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE.md)
[![VirusTotal](https://img.shields.io/badge/VirusTotal-0%2F77-brightgreen)](https://github.com/vedantsingh60/tokenwatch)
[![ClawhHub](https://img.shields.io/badge/ClawhHub-Token%20Budget%20Monitor-orange)](https://clawhub.ai/unisai/tokenwatch)

Know exactly what you're spending on AI APIs. Set budgets, get alerts before you overspend, compare model costs, and get actionable optimization suggestions — all running locally with zero dependencies.

---

## What It Does

- **Cost Tracking** — Record every API call with automatic cost calculation
- **Budget Alerts** — Set daily/weekly/monthly limits with threshold warnings
- **Model Comparison** — Compare costs across 11 models before making a call
- **Optimization Suggestions** — Get ranked tips to reduce spend with estimated savings
- **Dashboard** — Visual spending overview with budget status bars
- **Provider Hooks** — Auto-record from Anthropic and OpenAI response objects

---

## Supported Models & Pricing (Feb 2026)

| Provider | Model | Input ($/1M) | Output ($/1M) |
|----------|-------|-------------|--------------|
| **Anthropic** | `claude-opus-4-6` | $15.00 | $75.00 |
| **Anthropic** | `claude-sonnet-4-5-20250929` | $3.00 | $15.00 |
| **Anthropic** | `claude-haiku-4-5-20251001` | $0.80 | $4.00 |
| **OpenAI** | `gpt-4o` | $2.50 | $10.00 |
| **OpenAI** | `gpt-4o-mini` | $0.15 | $0.60 |
| **OpenAI** | `o1` | $15.00 | $60.00 |
| **OpenAI** | `o3-mini` | $1.10 | $4.40 |
| **Google** | `gemini-2.5-pro` | $1.25 | $10.00 |
| **Google** | `gemini-2.5-flash` | $0.075 | $0.30 |
| **Mistral** | `mistral-large-2501` | $2.00 | $6.00 |
| **Mistral** | `mistral-small-2501` | $0.10 | $0.30 |

---

## Quick Start

No installation needed — pure Python, zero dependencies.

```python
from tokenwatch import TokenWatch

monitor = TokenWatch()

# Set a monthly budget
monitor.set_budget(daily_usd=1.00, weekly_usd=5.00, monthly_usd=15.00)

# Record usage after each API call
monitor.record_usage(
    model="claude-haiku-4-5-20251001",
    input_tokens=1200,
    output_tokens=400,
    task_label="summarize article"
)

# View spending dashboard
print(monitor.format_dashboard())
```

**Auto-record from API responses:**

```python
from tokenwatch import TokenWatch, record_from_anthropic_response
import anthropic

client = anthropic.Anthropic()
monitor = TokenWatch()

response = client.messages.create(
    model="claude-haiku-4-5-20251001",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello!"}]
)

# Auto-extracts model, input_tokens, output_tokens from response
record_from_anthropic_response(monitor, response, task_label="greeting")
```

**Compare models before choosing:**

```python
# See cost for 2000 input + 500 output tokens across all models
for m in monitor.compare_models(2000, 500)[:5]:
    print(f"{m['model']:<40} ${m['cost_usd']:.6f}")
```

---

## Example Output

```
╔═══════════════════════════════════════════════════════════════╗
║              TOKEN BUDGET MONITOR — DASHBOARD                 ║
╚═══════════════════════════════════════════════════════════════╝

💰 SPENDING SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Today:   $0.0042  (4 calls, 13,600 tokens)
  Week:    $0.0231  (18 calls, 67,200 tokens)
  Month:   $0.1847  (92 calls, 438,000 tokens)

📋 BUDGET STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Daily:   [████░░░░░░░░░░░░░░░░] 42% $0.0042 / $1.00 ✅
  Monthly: [███████░░░░░░░░░░░░░] 37% $0.1847 / $0.50 ✅

💡 OPTIMIZATION TIPS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🔴 Swap Opus → Sonnet for non-reasoning tasks (save ~$8.20/mo)
  🟡 High avg cost/call on gpt-4o — consider reducing prompt length
  🟢 ✅ Spending looks efficient overall
```

---

## Privacy & Security

- **Local-only** — all data stored in `.tokenwatch/` on your machine
- **No external calls** — works completely offline
- **No API keys required** — the monitor itself needs no credentials
- **Full transparency** — MIT licensed, source code included

---

## Available on ClawhHub

Install directly via [ClawhHub](https://clawhub.ai/unisai/tokenwatch) for integration with Claude Code and OpenClaw.

---

## License

MIT License — see [LICENSE.md](LICENSE.md) for details.

© 2026 UnisAI Community
