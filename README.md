# Sharing LLM

## Introduction

This project leverages LiteLLM proxy to create a secure and efficient system for sharing Large Language Model (LLM) credits among friends and those in need. By pooling our resources, we aim to democratize access to powerful AI language models while maintaining control and security over our shared credits.

Our solution allows us to:
- Securely share LLM credits without exposing individual API keys
- Manage and monitor credit usage across multiple users
- Provide controlled access to those who need LLM capabilities but may not have the resources

This system is ideal for collaborative projects, educational purposes, or supporting AI research and development in resource-constrained environments.

## Usage

Main reference: https://docs.litellm.ai/docs/proxy/user_keys#request-format

### Chat

```python
from openai import OpenAI

client = OpenAI(
    api_key="please-contact", base_url="https://llm.tqtensor.com"
)

response = client.chat.completions.create(
    model="bedrock/anthropic.claude-3-sonnet-20240229-v1:0",
    messages=[
        {"role": "user", "content": "this is a test request, write a short poem"}
    ],
)

print(response)
```

### Embeddings

```python
from openai import OpenAI

client = OpenAI(
    api_key="please-contact", base_url="https://llm.tqtensor.com"
)

response = client.embeddings.create(
    input=["hello from litellm"], model="bedrock/cohere.embed-english-v3"
)

print(response)
```
