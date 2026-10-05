# Provider APIs - Setup Guides

APIs run by the companies that train or fine-tune the models themselves.

Models and limits below mirror `data.json` as of 2026-10-05. Check `data.json` for the current list.

## Google Gemini

**Models:** Gemini 3.8 / 3.7 / 3.6 / 3.5 Flash, Gemini 3.5 / 3.1 Flash-Lite, Gemini 2.5 Pro / Flash / Flash-Lite, Gemma 4 31B, Gemma 4 26B A4B
**Context:** 1M (Gemini), 256K (Gemma). Text + image + audio + video input.
**Limits:** Google no longer publishes per-model free-tier limits; last published figures were 15-30 RPM and 1,500 RPD for Flash / Flash-Lite, 5 RPM and 50 RPD for 2.5 Pro. Check quotas in AI Studio.
**Data use:** Free-tier prompts may be used to improve Google products, except for users in the EEA, Switzerland and the UK.
**Region note:** The free tier is available in the EU, UK and Switzerland, but Google's terms require Paid Services if you make an API client available to users there.

### Get your API key

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Sign in with a Google account.
3. Click "Create API Key" and select or create a Google Cloud project.
4. Copy the generated key.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_GEMINI_API_KEY",
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

response = client.chat.completions.create(
    model="gemini-3.5-flash",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export GEMINI_API_KEY="your-key-here"
# Or for OpenAI-compatible usage:
export OPENAI_API_KEY="your-key-here"
export OPENAI_BASE_URL="https://generativelanguage.googleapis.com/v1beta/openai/"
```

---

## Mistral AI

**Models:** Mistral Medium 3.5, Mistral Large 3, Mistral Small 4, Ministral 3 (3B / 8B / 14B), Codestral
**Context:** 256K (128K for Codestral)
**Limits:** Free mode includes $10/month in API credits, shared across Studio, the API and Vibe Code. Last published rate limits were ~1 RPS and 500K TPM; current limits are on the Limits page of the admin panel.
**Data use:** Free-mode prompts may be used to train Mistral models unless you opt out.

### Get your API key

1. Go to the [Mistral Console](https://console.mistral.ai/api-keys).
2. Create an account or sign in. Free mode is enabled by default, no credit card needed.
3. Go to API Keys and create a new key.
4. Copy the key.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_MISTRAL_API_KEY",
    base_url="https://api.mistral.ai/v1/"
)

response = client.chat.completions.create(
    model="mistral-small-2603",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export MISTRAL_API_KEY="your-key-here"
```

---

## Cohere

**Models:** Command A+ (218B), Command A, Command A Reasoning, Command A Vision, Command A Translate, Command R+, Command R, Command R7B, Command R7B Arabic, Aya Expanse 32B, Aya Vision 32B
**Limits:** 20 RPM, 1,000 API calls/month
**Restriction:** Trial keys are for non-commercial use only.

### Get your API key

1. Go to the [Cohere Dashboard](https://dashboard.cohere.com/api-keys).
2. Create an account or sign in.
3. Go to API Keys and generate a trial key.
4. Copy the key.

### Usage example

The native API lives at `https://api.cohere.com/v2`. For the OpenAI SDK, use Cohere's Compatibility API:

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_COHERE_API_KEY",
    base_url="https://api.cohere.ai/compatibility/v1"
)

response = client.chat.completions.create(
    model="command-a-03-2025",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export CO_API_KEY="your-key-here"
```

---

## Z AI (Zhipu AI)

**Models:** GLM-4.7-Flash (200K, reasoning), GLM-4.6V-Flash (multimodal), GLM-4.5-Flash (retirement announced; requests will be auto-routed to GLM-4.7-Flash)
**Limits:** 1 concurrent request

### Get your API key

1. Go to [Zhipu AI](https://open.bigmodel.cn/usercenter/apikeys), or the international platform at [z.ai](https://z.ai).
2. Sign up. Overseas phone numbers are accepted, and the chat API does not require real-name verification (the Batch API does).
3. Go to API Keys and create a new key.
4. Copy the key.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_ZHIPU_API_KEY",
    base_url="https://open.bigmodel.cn/api/paas/v4/"
    # International endpoint: "https://api.z.ai/api/paas/v4/"
)

response = client.chat.completions.create(
    model="glm-4.7-flash",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export ZHIPU_API_KEY="your-key-here"
```

---

## Aion Labs

**Models:** Aion 3.0, Aion 3.0 Mini, Aion 2.0 (reasoning, 128K), Aion RP Llama 3.1 8B (32K)
**Limits:** 15 RPM, 20K tokens/day
**Specialty:** Roleplay and storytelling.

### Get your API key

1. Go to [Aion Labs](https://www.aionlabs.ai/app/api-keys/).
2. Create an account. No credit card required.
3. Create a new API key and copy it.

### Usage example

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_AION_API_KEY",
    base_url="https://api.aionlabs.ai/v1/"
)

response = client.chat.completions.create(
    model="aion-labs/aion-3.0-mini",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Environment variable

```bash
export AION_API_KEY="your-key-here"
```
