---
name: free-llm-apis
description: Guide users through obtaining and configuring free API keys for LLM providers. Use when the user wants to set up a free LLM API, get a free API key, connect to a free model provider, configure an OpenAI-compatible endpoint at no cost, or asks about free tiers for AI models. Triggers on "free API key", "free LLM", "set up Gemini/Groq/Mistral/etc.", "which free provider", "how to get an API key", "free model access", "configure LLM for free".
---

# Free LLM API Setup

Help users pick a free LLM provider and configure their API key. Every provider here has a permanent free tier, no credit card needed.

The source of truth is `data.json` at the repo root (rendered into `README.md`). Model lists and limits change often: when a figure matters, check `data.json` or the provider's docs rather than the summaries below.

## Provider Selection

Ask the user what matters most, then recommend accordingly:

| Priority | Best picks |
|---|---|
| No signup or API key | Kilo Code (200 req/hr per IP), OVHcloud AI Endpoints (2 RPM per model), LLM7.io (`turbo` models) |
| Highest daily request budget | NVIDIA NIM (40 RPM, 10,000 RPD), Groq (30 RPM, 1,000 RPD), Gemini Flash / Flash-Lite (1,500 RPD) |
| Fastest inference | Groq (LPU inference) |
| Largest context | Google Gemini (1M, plus image/audio/video input), NVIDIA NIM and Kilo Code Nemotron models (1M) |
| Largest model selection | Hugging Face (thousands, credit-metered), NVIDIA NIM (100+), Cloudflare Workers AI (60+) |
| Strongest proprietary models | Google Gemini (Gemini 3.x Flash, 2.5 Pro), Mistral AI (Mistral Large 3, Medium 3.5), Cohere (Command A+) |
| Coding | Mistral Codestral, Poolside Laguna and Cohere North Mini Code (via Kilo Code or OpenRouter) |
| EU data residency | OVHcloud AI Endpoints (EU data centers), Mistral AI (France) |
| Roleplay / storytelling | Aion Labs |

### Caveats to mention when relevant

- **Training on prompts:** Gemini and Mistral free tiers may use prompts to improve or train models (Mistral allows opt-out; Gemini excludes users in the EEA, UK and Switzerland). Some OpenRouter and Kilo Code free models route to providers that log prompts.
- **Non-commercial only:** Cohere's trial key.
- **Identity verification:** SiliconFlow (mainland-Chinese documents) and ModelScope (Alibaba Cloud account + real-name verification).
- **Gemini in EEA/UK/CH:** the free tier works there, but Google's terms require Paid Services if you make an API client available to users in those regions.
- **Volatile catalogs:** Kilo Code, OpenRouter and LLM7.io free pools rotate frequently.

### Provider categories

**Provider APIs** -- run by the companies that train the models:
- Google Gemini, Mistral AI, Cohere, Z AI (Zhipu), Aion Labs
- See [references/provider-apis.md](references/provider-apis.md) for setup instructions.

**Inference providers** -- third-party platforms hosting open-weight models:
- Groq, NVIDIA NIM, Cloudflare Workers AI, OpenRouter, Kilo Code, Hugging Face, Ollama Cloud, OVHcloud AI Endpoints, LLM7.io, ModelScope, SiliconFlow
- See [references/inference-providers.md](references/inference-providers.md) for setup instructions.

## Workflow

1. Ask what models, rate limits, or features the user cares about. If they already know which provider they want, skip to step 3.
2. Match their priorities against the table above. Suggest 1-2 options with a short reason, plus any caveat that applies.
3. Load the right reference file and walk through the setup steps for that provider (API key, code example, env var).
4. Offer a quick test script or curl command so they can confirm the key works.

## Quick Test Template

After setup, use this to verify any OpenAI-compatible provider:

```python
from openai import OpenAI

client = OpenAI(api_key="KEY", base_url="BASE_URL")

response = client.chat.completions.create(
    model="MODEL_NAME",
    messages=[{"role": "user", "content": "Say hello in one sentence."}],
    max_tokens=50
)
print(response.choices[0].message.content)
```

Replace KEY, BASE_URL, and MODEL_NAME with values from the provider's setup guide. For no-key providers, use the curl example in their section instead.

## Key Notes

- All endpoints work with the OpenAI SDK unless noted.
- RPM = requests per minute. RPD = requests per day. TPM / TPD = tokens per minute / day. RPS = requests per second.
- Several providers no longer publish numeric free-tier limits (Gemini, Mistral, Ollama Cloud); point users to their dashboards.
- Don't hardcode API keys. Use environment variables or a secrets manager.
