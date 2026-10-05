# Inference Providers - Setup Guides

Third-party platforms that host open-weight models from various sources.

Models and limits below mirror `data.json` as of 2026-10-05. Check `data.json` for the current list.

## Groq

**Models:** GPT-OSS 120B, GPT-OSS 20B, Qwen3.8 27B (all 131K context)
**Limits:** 30 RPM, 1,000 RPD per model
**Speed:** Ultra-fast LPU inference.
**Note:** Groq retires models often; check its [deprecations page](https://console.groq.com/docs/deprecations).

### Get your API key

1. Go to the [Groq Console](https://console.groq.com/keys).
2. Sign up with email or Google, no credit card needed.
3. Click "Create API Key".
4. Copy the key.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_GROQ_API_KEY",
    base_url="https://api.groq.com/openai/v1/"
)

response = client.chat.completions.create(
    model="openai/gpt-oss-120b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export GROQ_API_KEY="your-key-here"
```

---

## NVIDIA NIM

**Models:** Nemotron 3 Ultra 550B, Nemotron 3 Super 120B (1M context), Llama 3.1 Nemotron Ultra 253B, Gemma 4 31B, Mistral Large 2, GPT-OSS 20B, +92 more
**Limits:** 40 RPM, 10,000 RPD per model
**Requirement:** Free NVIDIA Developer Program membership.

### Get your API key

1. Go to [NVIDIA Build](https://build.nvidia.com/explore/discover).
2. Create an NVIDIA account or sign in, and join the Developer Program.
3. Pick any model and click "Get API Key".
4. Copy the key.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_NVIDIA_API_KEY",
    base_url="https://integrate.api.nvidia.com/v1/"
)

response = client.chat.completions.create(
    model="nvidia/nemotron-3-super-120b-a12b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export NVIDIA_API_KEY="your-key-here"
```

---

## Cloudflare Workers AI

**Models:** Llama 3.3 70B, Llama 4 Scout, GPT-OSS 120B, Gemma 4 26B, GLM-4.7-Flash, Mistral Small 3.1, DeepSeek R1 Distill Qwen 32B, +55 more
**Limits:** 10,000 Neurons/day, shared across all models, reset at 00:00 UTC
**Note:** Going over the limit fails the request, it doesn't bill you. Some large models (Kimi K2.6/K2.7, GLM-5.x, DeepSeek V4) need the Workers Paid plan.

### Get your API key

1. Go to the [Cloudflare Dashboard](https://dash.cloudflare.com/profile/api-tokens).
2. Sign in or create a Cloudflare account.
3. Create an API token with the "Workers AI" permission.
4. Note your Account ID from the dashboard sidebar.
5. Copy both the token and the account ID.

### Usage example

The native endpoint is `.../ai/run`; the OpenAI-compatible one is `.../ai/v1`.

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_CLOUDFLARE_API_TOKEN",
    base_url="https://api.cloudflare.com/client/v4/accounts/YOUR_ACCOUNT_ID/ai/v1/"
)

response = client.chat.completions.create(
    model="@cf/meta/llama-3.3-70b-instruct-fp8-fast",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export CLOUDFLARE_API_TOKEN="your-token-here"
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

---

## OpenRouter

**Models:** 17 named free models (`:free` suffix) including Nemotron 3 Ultra / Super / 3.5 Lightning, Gemma 4 26B / 31B, Qwen3.8 27B, Laguna S / XS 2.1, Inkling, plus the `openrouter/free` router
**Limits:** 20 RPM, 50 RPD per free model (1,000 RPD after a one-time $10+ credit purchase)
**Data use:** Free providers may log prompts for training.

### Get your API key

1. Go to [OpenRouter Keys](https://openrouter.ai/keys).
2. Sign up with email, Google, or GitHub.
3. Create a new key.
4. Copy the key.

### Usage example

`openrouter/free` picks a free model for you. Use a specific `:free` id (e.g. `qwen/qwen3.8-27b:free`) to pin one.

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_OPENROUTER_API_KEY",
    base_url="https://openrouter.ai/api/v1/"
)

response = client.chat.completions.create(
    model="openrouter/free",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export OPENROUTER_API_KEY="your-key-here"
```

---

## Kilo Code

**Models:** Free pool including Nemotron 3 Ultra 550B, Nemotron 3.5 Lightning (1M), Step 3.7 Flash, Qwen3.8 27B, Laguna S / XS 2.1, North Mini Code, Hy3, plus the `kilo-auto/free` router
**Limits:** 200 requests/hour per IP
**No API key required** for free models.
**Data use:** `kilo-auto/free` may route to providers that log prompts and outputs. NVIDIA endpoints are trial use only: don't send personal or confidential data.
**Note:** The free pool changes frequently.

### Get your API key

1. Optional: create an account at [Kilo](https://app.kilo.ai/profile) to get a key. Free models work without one.

### Usage example

```bash
curl https://api.kilo.ai/api/gateway/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "kilo-auto/free", "messages": [{"role": "user", "content": "Hello!"}]}'
```

With a key, the OpenAI SDK works too:

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_KILO_API_KEY",
    base_url="https://api.kilo.ai/api/gateway"
)

response = client.chat.completions.create(
    model="kilo-auto/free",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export KILO_API_KEY="your-key-here"  # optional
```

---

## Hugging Face

**Models:** Llama 3.1 8B, Gemma 3 4B, Phi-4, Qwen2.5 7B, Qwen2.5 Coder 7B, plus thousands of community models
**Limits:** $0.10/month in Inference Provider credits (subject to change)
**Routing:** Requests go to Fireworks, Together, Hyperbolic, Nebius, Novita, DeepInfra and others.

### Get your API key

1. Go to [Hugging Face Tokens](https://huggingface.co/settings/tokens).
2. Create an account or sign in.
3. Create a new token with "Make calls to Inference Providers" permission.
4. Copy the token.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_HF_TOKEN",
    base_url="https://router.huggingface.co/v1/"
)

response = client.chat.completions.create(
    model="meta-llama/Llama-3.1-8B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export HF_TOKEN="your-token-here"
```

---

## Ollama Cloud

**Models:** DeepSeek V4 Pro / Flash, Kimi K3, MiniMax M3, GPT-OSS 120B / 20B, Nemotron 3 Ultra, Mistral Large 3 675B, Qwen3.5 397B, +7 more
**Limits:** Unpublished session limits (reset every 5 hours) and weekly limits (reset every 7 days), weighted by tokens per model

### Get your API key

1. Go to [Ollama Keys](https://ollama.com/settings/keys).
2. Create an account or sign in.
3. Create a new API key.
4. Copy the key.

### Usage example

The native API is `https://ollama.com/api`; the OpenAI-compatible one is `https://ollama.com/v1`.

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_OLLAMA_API_KEY",
    base_url="https://ollama.com/v1/"
)

response = client.chat.completions.create(
    model="gpt-oss:120b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export OLLAMA_API_KEY="your-key-here"
```

---

## OVHcloud AI Endpoints

**Models:** Qwen3.5 397B, Qwen3.8 27B, Qwen3.6 27B, GPT-OSS 120B / 20B, Llama 3.3 70B, Qwen3 32B, Qwen2.5 VL 72B, Mistral Small 3.2, Mistral Nemo, +more
**Limits:** 2 RPM per IP per model (anonymous)
**No API key required** for the free tier. An API key raises the limit to 400 RPM but is billed per token.
**Region:** Hosted in EU data centers.

### Get your API key

1. Nothing needed for the free tier. Browse models in the [catalog](https://www.ovhcloud.com/en/public-cloud/ai-endpoints/catalog/).

### Usage example

The OpenAI SDK requires a non-empty `api_key`; pass any placeholder for the anonymous tier, or use curl.

```bash
curl https://oai.endpoints.kepler.ai.cloud.ovh.net/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "gpt-oss-120b", "messages": [{"role": "user", "content": "Hello!"}]}'
```

Or with the OpenAI SDK:

```python
from openai import OpenAI

client = OpenAI(
    api_key="unused",
    base_url="https://oai.endpoints.kepler.ai.cloud.ovh.net/v1/"
)

response = client.chat.completions.create(
    model="gpt-oss-120b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
# No key needed for the anonymous tier
```

---

## LLM7.io

**Models:** GPT-OSS 20B, Mistral Nemo, MiniMax M2.7, DeepSeek V4 Flash (`turbo` tier; the catalog rotates often)
**Limits:** Free token: 1 RPS, 60 RPM, 250 requests/hour, 100K tokens/day. Anonymous limits are unpublished.
**No API key required** for `turbo` models; a free token only raises limits. `pro` models need the paid plan.

### Get your API key

1. Optional: go to [token.llm7.io](https://token.llm7.io) and sign in to get a free token.
2. Copy the token.

### Usage example

```bash
curl https://api.llm7.io/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "gpt-oss:20b", "messages": [{"role": "user", "content": "Hello!"}]}'
```

With a key, the OpenAI SDK works too:

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_LLM7_TOKEN",
    base_url="https://api.llm7.io/v1/"
)

response = client.chat.completions.create(
    model="gpt-oss:20b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export LLM7_API_KEY="your-token-here"  # optional
```

---

## ModelScope

**Models:** Qwen3.5 35B A3B, Qwen3.5 27B, plus other API-Inference-enabled models
**Limits:** 2,000 requests/day total, up to 500/day per model (dynamic); dynamic concurrency limits
**Requirement:** Alibaba Cloud account binding and real-name verification.

### Get your API key

1. Go to [ModelScope Access Tokens](https://modelscope.cn/my/myaccesstoken).
2. Sign up, bind an Alibaba Cloud account and complete real-name verification.
3. Copy your access token.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_MODELSCOPE_TOKEN",
    base_url="https://api-inference.modelscope.cn/v1/"
)

response = client.chat.completions.create(
    model="Qwen/Qwen3.5-27B",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export MODELSCOPE_API_KEY="your-token-here"
```

---

## SiliconFlow

**Models:** Qwen3 8B (free); most of the 100+ catalog models are paid
**Limits:** 1,000 RPM, 50,000 TPM
**Requirement:** Real-name identity verification. It supports mainland-Chinese documents; international users must contact support.

### Get your API key

1. Go to [SiliconFlow API Keys](https://cloud.siliconflow.cn/account/ak).
2. Sign up and complete identity verification.
3. Create a new API key and copy it.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_SILICONFLOW_API_KEY",
    base_url="https://api.siliconflow.cn/v1/"
)

response = client.chat.completions.create(
    model="Qwen/Qwen3-8B",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export SILICONFLOW_API_KEY="your-key-here"
```
