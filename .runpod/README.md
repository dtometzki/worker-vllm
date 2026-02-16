![vLLM worker banner](https://cpjrphpz3t5wbwfe.public.blob.vercel-storage.com/worker-vllm_banner.jpeg)

Run LLMs using [vLLM](https://docs.vllm.ai) with an OpenAI-compatible API

---

[![RunPod](https://api.runpod.io/badge/runpod-workers/worker-vllm)](https://www.runpod.io/console/hub/runpod-workers/worker-vllm)

---
# vLLM Worker Deployment

This repository provides a high-performance LLM serving solution using [vLLM](https://docs.vllm.ai) with an OpenAI-compatible API layer, optimized for RunPod Serverless environments.

## 🛠 Configuration

Behavior is managed through environment variables. Below are the primary settings used in this deployment:

| Environment Variable | Description | Default / Current |
| --- | --- | --- |
| `MODEL_NAME` | Path or Hugging Face repo ID of the model weights | *User Defined* |
| `MAX_MODEL_LEN` | Model's maximum context length | `8192` |
| `HF_TOKEN` | Access token for gated or private models | `{{ RUNPOD_SECRET_HF_TOKEN }}` |
| `QUANTIZATION` | Quantization method (e.g., "awq", "gptq") | *Optional* |
| `GPU_MEMORY_UTILIZATION` | Fraction of GPU memory to reserve | `0.95` |
| `TENSOR_PARALLEL_SIZE` | Number of GPUs to use | `1` |

---

## 🚀 API Usage

The worker supports both **OpenAI-compatible** and **RunPod Native** API formats.

### OpenAI-Compatible API

Use this for seamless integration with existing SDKs. Set your base URL to:
`https://api.runpod.ai/v2/<ENDPOINT_ID>/openai/v1`

**Python Example:**

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.getenv("RUNPOD_API_KEY"),
    base_url="https://api.runpod.ai/v2/<ENDPOINT_ID>/openai/v1",
)

response = client.chat.completions.create(
    model="your-model-name",
    messages=[{"role": "user", "content": "Hello, how are you?"}],
    temperature=0.7,
    max_tokens=256
)

print(response.choices[0].message.content)

```

### RunPod Native API

For direct requests via the RunPod UI or specialized integrations:

**Endpoint:** `https://api.runpod.ai/v2/<ENDPOINT_ID>/run`

```json
{
  "input": {
    "messages": [
      { "role": "user", "content": "Explain the benefits of vLLM." }
    ],
    "sampling_params": {
      "max_tokens": 500,
      "temperature": 0.8
    }
  }
}

```

---

## ℹ️ Additional Features

* **Streaming:** Supported on both API paths by setting `"stream": true`.
* **Compatibility:** Supports all architectures recognized by the vLLM engine.
* **Auto-Tool Choice:** Can be enabled via `ENABLE_AUTO_TOOL_CHOICE` if the model supports it.

---
