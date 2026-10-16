# 🚀 Complete AI Stack Guide: Inference Servers, Gateways & Agentic Tools

> A beginner-friendly, super detailed guide to building your own AI infrastructure stack — from running models locally to managing multiple providers through a unified gateway, all powered by cutting-edge agentic coding tools.

---

## 📋 Table of Contents

1. [What Is This Stack?](#-what-is-this-stack)
2. [Inference Servers — Run AI Models Anywhere](#️-inference-servers--run-ai-models-anywhere)
   - [BitNet](#bitnet)
   - [llama.cpp](#llamacpp)
   - [Ollama](#ollama)
   - [vLLM](#vllm)
3. [Gateway Services — One API, Many Providers](#-gateway-services--one-api-many-providers)
   - [LiteLLM](#litellm)
   - [Claude Code Provider Gateway (CCPG)](#claude-code-provider-gateway-ccpg)
   - [MLPal Gateway](#mlpal-gateway)
   - [Cloudflare AI Gateway](#cloudflare-ai-gateway)
4. [Agentic Tools — AI-Powered Coding Agents](#-agentic-tools--ai-powered-coding-agents)
   - [Claude Code](#claude-code)
   - [OpenAI Codex CLI](#openai-codex-cli)
   - [Gemini CLI](#gemini-cli)
   - [Other Agentic Tools](#other-agentic-tools)
5. [How to Combine Everything](#-how-to-combine-everything)
6. [FAQ & Glossary](#-faq--glossary)
7. [Resources](#-resources)

---

## 🤔 What Is This Stack?

When you work with AI, you don't just "use ChatGPT." Real developers and teams build **stacks** — a combination of tools that work together to:

- **Run AI models** on their own hardware (servers, laptops, GPUs)
- **Route requests** to different AI providers (OpenAI, Anthropic, Google, etc.)
- **Control costs** and manage usage
- **Use AI agents** that can write code, debug issues, and automate tasks

This guide covers three layers:

```
┌─────────────────────────────────────────────────────────┐
│  Agentic Tools (Claude Code, Codex CLI, Gemini CLI)     │ ← You interact here
├─────────────────────────────────────────────────────────┤
│  Gateway Layer (LiteLLM, MLPal, Cloudflare, CCPG)      │ ← Routes requests
├─────────────────────────────────────────────────────────┤
│  Inference Servers (llama.cpp, Ollama, vLLM, BitNet)   │ ← Runs the models
└─────────────────────────────────────────────────────────┘
```

Think of it like a restaurant:
- **Inference servers** = The kitchen (where the food is cooked)
- **Gateways** = The waiters (they take your order and deliver it to the right kitchen)
- **Agentic tools** = The customers (they tell the waiter what they want to eat)

---

## 🖥️ Inference Servers — Run AI Models Anywhere

Inference servers are programs that take an AI model and make it "think" — they process your input and generate a response. Think of them as the engine that powers the AI.

---

### BitNet

🔗 [https://github.com/microsoft/BitNet](https://github.com/microsoft/BitNet)

**What is it?**
BitNet (specifically `bitnet.cpp`) is Microsoft's official inference framework for **1-bit LLMs**. These are ultra-efficient models that use only 1.58 bits per parameter instead of the usual 16 or 32 bits.

**Why use it?**
- **Insanely fast on CPUs**: Up to 6.17x speedup on x86 CPUs and 5.07x on ARM CPUs
- **Super energy efficient**: Reduces energy consumption by 55% to 82%
- **Runs huge models locally**: Can run a 100B parameter model on a single CPU
- **Lossless**: Same quality as full-precision models but much faster

**Supported platforms:**
- x86 CPUs (Intel, AMD)
- ARM CPUs (Apple Silicon, Raspberry Pi)
- GPU support (official kernels available)
- NPU support (coming soon)

**Best for:** Running AI on low-power devices, edge computing, or when you need maximum efficiency without a GPU.

**Quick example:**
```bash
# Install and run BitNet models
git clone https://github.com/microsoft/BitNet
cd BitNet
# Follow the build instructions for your platform
./run_model.py --model BitNet-b1.58-2B-4T
```

---

### llama.cpp

🔗 [https://github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)

**What is it?**
llama.cpp is the most popular open-source inference engine. It's written in pure C/C++ with no external dependencies, making it incredibly lightweight and portable.

**Why use it?**
- **Works on everything**: Runs on Macs, Windows, Linux, phones, servers, and even Raspberry Pis
- **Multiple quantization levels**: Supports 1.5-bit to 8-bit quantization (smaller files, less memory)
- **Hardware optimized**: Special optimizations for Apple Silicon, NVIDIA, AMD, Intel GPUs
- **Hybrid inference**: Can split work between CPU and GPU when the model is too big for VRAM
- **Community support**: Used by Ollama, LM Studio, and dozens of other tools

**Supported platforms:**
| Backend | Devices |
|---------|---------|
| CUDA | NVIDIA GPUs |
| Metal | Apple Silicon (M1, M2, M3, M4) |
| Vulkan | Most modern GPUs |
| HIP | AMD GPUs |
| CPU | All architectures (x86, ARM, RISC-V) |

**Best for:** Running local models on any hardware, developers who want maximum control, and projects that need to work offline.

**Quick example:**
```bash
# Download a model (GGUF format)
wget https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q4_K_M.gguf

# Run it
./main -m llama-2-7b-chat.Q4_K_M.gguf -p "Hello, how are you?" -n 128
```

---

### Ollama

🔗 [https://github.com/ollama/ollama](https://github.com/ollama/ollama)

**What is it?**
Ollama is the easiest way to run AI models locally. Think of it as "Docker for AI models" — one command and you're chatting with an AI on your laptop.

**Why use it?**
- **One-command install**: `curl -fsSL https://ollama.com/install.sh | sh`
- **Simple model management**: `ollama run llama3.1` and you're done
- **REST API included**: Perfect for integrating with apps
- **Huge model library**: Thousands of models ready to download
- **Privacy-first**: Everything runs locally, no data leaves your machine

**Supported platforms:**
- macOS (Apple Silicon and Intel)
- Windows
- Linux
- Docker containers

**Best for:** Beginners, developers who want a quick local setup, and teams that need privacy without complexity.

**Quick example:**
```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Download and chat with a model
ollama run llama3.1
>>> Hello! How can I help you today?

# Or use the API
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.1",
  "messages": [{"role": "user", "content": "Tell me a joke"}]
}'
```

**Community integrations:** Works with VS Code, Cursor, LibreChat, Open WebUI, and 50+ other tools.

---

### vLLM

🔗 [https://github.com/vllm-project/vllm](https://github.com/vllm-project/vllm)

**What is it?**
vLLM is a high-performance inference engine built for production workloads. It's what you use when you need to serve thousands of requests per second.

**Why use it?**
- **State-of-the-art throughput**: Uses "PagedAttention" for maximum efficiency
- **Production-ready**: Built-in batching, caching, and streaming
- **OpenAI-compatible API**: Works with any OpenAI SDK
- **Supports 200+ model architectures**: Llama, Qwen, Gemma, Mistral, Mixtral, and more
- **Advanced features**: Tool calling, structured output, multi-LoRA support

**Key technology — PagedAttention:**
Traditional inference wastes memory. PagedAttention splits attention memory into pages (like RAM pages), reducing waste from 60-80% to less than 4%. This means you can serve way more users with the same GPU.

**Supported platforms:**
- NVIDIA GPUs (CUDA)
- AMD GPUs (HIP)
- Intel GPUs (SYCL)
- Google TPUs
- AWS Trainium/Inferentia
- CPU (for testing)

**Best for:** Production deployments, high-traffic APIs, enterprise environments, and when you need maximum throughput.

**Quick example:**
```bash
# Install vLLM
pip install vllm

# Start an OpenAI-compatible server
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-3.1-8B-Instruct \
    --port 8000

# Use it like OpenAI
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="dummy")
response = client.chat.completions.create(
    model="meta-llama/Llama-3.1-8B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

---

## 🔀 Gateway Services — One API, Many Providers

Gateways sit between your app and the AI models. They handle routing, load balancing, cost tracking, and protocol translation. Instead of integrating with OpenAI, Anthropic, and Google separately, you use ONE gateway.

---

### LiteLLM

🔗 [https://github.com/BerriAI/litellm](https://github.com/BerriAI/litellm)

**What is it?**
LiteLLM is a Python library and proxy server that lets you call 100+ LLMs using a single, consistent OpenAI-style API.

**Why use it?**
- **100+ providers**: OpenAI, Anthropic, Google, Azure, Bedrock, Cohere, Hugging Face, and many more
- **One interface**: Same code works for every provider
- **Cost tracking**: Built-in budget management and spend tracking
- **Load balancing**: Distribute requests across multiple providers
- **Fallbacks**: If one provider fails, automatically try another

**Example — Switching providers with one line:**
```python
import litellm

# Call OpenAI
response = litellm.completion(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hello"}]
)

# Same code, different provider — just change the model name
response = litellm.completion(
    model="claude-3-5-sonnet-20241022",
    messages=[{"role": "user", "content": "Hello"}]
)
```

**Best for:** Developers who want flexibility, teams using multiple providers, and anyone who wants to avoid vendor lock-in.

---

### Claude Code Provider Gateway (CCPG)

🔗 [https://github.com/danielalves96/claude-code-provider-gateway](https://github.com/danielalves96/claude-code-provider-gateway)

**What is it?**
CCPG is a desktop app that lets you use Claude Code (Anthropic's coding agent) with ANY AI provider — OpenAI, DeepSeek, Ollama, local models, and more.

**Why use it?**
- **Keep Claude Code's workflow**: Same tools, commands, and UI
- **Choose your backend**: Use cheaper models for simple tasks, stronger ones for hard tasks
- **Local-first**: No telemetry, no cloud account, completely private
- **Model Chains**: Automatic fallback if a provider fails
- **Token savers**: Compress tool outputs to save money

**How it works:**
```
Claude Code  →  CCPG Desktop App  →  Your chosen provider (OpenAI, DeepSeek, Ollama, etc.)
```

**Quick example:**
```bash
# Download CCPG, install, and launch
ccpg --DeepSeek        # Use DeepSeek as backend
ccpg --OpenRouter      # Use OpenRouter
ccpg --Ollama          # Use local models
ccpg --all             # Access all enabled providers
```

**Best for:** Claude Code power users who want flexibility, cost savings, or need to use local/private models.

---

### MLPal Gateway

🔗 [https://github.com/mlpal-ai/mlpal-gateway](https://github.com/mlpal-ai/mlpal-gateway)

**What is it?**
MLPal is an enterprise-grade, self-hosted AI gateway that unifies Anthropic, OpenAI, Google, AWS Bedrock, and custom providers under a single API.

**Why use it?**
- **One API format for everything**: Use Anthropic Messages API for any provider
- **Cost metering**: Every request is tracked in Compute Units (1 CU = $10)
- **Per-key budgets**: Set spending limits for each API key
- **Admin console**: Full UI for managing keys, policies, and routing
- **Curated catalog**: 75+ models, automatically kept up-to-date
- **Cloud backend support**: Route through Azure, Vertex AI, or AWS Bedrock

**Key features:**
| Feature | Description |
|---------|-------------|
| Router tags | `"model": "mlpal"` automatically picks the best model |
| Cost tracking | X-MLPal-Compute-Units header on every response |
| Observability | Cache hit rates, latency p50/p95, TTFT metrics |
| Per-key policies | Allow/deny specific models per API key |

**Quick example:**
```bash
# Self-host with Docker
docker compose up

# Use the Anthropic SDK with MLPal
from anthropic import Anthropic
client = Anthropic(
    base_url="http://localhost:8000",
    api_key="mlpal_sk_..."
)
```

**Best for:** Enterprises, teams with complex infrastructure, and anyone who needs production-grade observability and cost control.

---

### Cloudflare AI Gateway

🔗 [https://www.cloudflare.com/products/ai-gateway/](https://www.cloudflare.com/products/ai-gateway/)

**What is it?**
A managed (cloud-hosted) AI gateway built on Cloudflare's global network. No servers to maintain, no infrastructure to run.

**Why use it?**
- **Zero setup**: Just create an account and start using it
- **Global CDN**: Low latency from 330+ cities worldwide
- **Built-in caching**: Save money by caching repeated requests
- **Unified billing**: One bill for all providers
- **Security**: DDoS protection, rate limiting, WAF included
- **Observability**: Rich logs and analytics dashboard

**Key capabilities:**
- **Dynamic routing**: Route based on cost, latency, or provider availability
- **Caching**: Automatic caching for identical requests
- **Fallbacks**: Try provider A, then B, then C if failures happen
- **Rate limiting**: Control how many requests each user can make
- **Logging**: Track every request with user, cost, and duration

**Quick example:**
```javascript
// In a Cloudflare Worker
const openai = new OpenAI({
  apiKey: "your-key",
  baseURL: await env.AI.gateway("my-gateway").getUrl("openai"),
});
```

**Best for:** Teams that don't want to manage infrastructure, startups that need to scale quickly, and anyone who wants enterprise features without enterprise complexity.

---

## 🤖 Agentic Tools — AI-Powered Coding Agents

Agentic tools are AI assistants that don't just answer questions — they **take action**. They can read your code, edit files, run commands, search the web, and complete complex tasks autonomously.

---

### Claude Code

🔗 [https://claude.ai/product/claude-code](https://claude.ai/product/claude-code)

**What is it?**
Claude Code is Anthropic's official terminal-based coding agent. It understands your entire codebase, edits files, runs shell commands, and helps you ship features faster.

**Key features:**
- **Full codebase understanding**: Indexes your entire project
- **File editing**: Reads, writes, and modifies any file
- **Shell access**: Runs `git`, `npm`, `python`, and any command
- **MCP support**: Connects to external tools (databases, APIs, etc.)
- **Custom commands**: Build your own workflows
- **IDE integration**: Works with VS Code, Cursor, and more

**How to use it:**
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Start it in your project folder
cd my-project
claude

# Ask it to do anything
> Refactor the authentication module to use JWT tokens
> Fix the failing tests in the payment module
> Write documentation for the new API endpoints
```

**Pricing:** Uses your Anthropic API key or Claude Pro subscription.

**Best for:** Developers who want the most powerful coding agent, teams that need advanced agentic capabilities, and anyone building complex software.

---

### OpenAI Codex CLI

🔗 [https://github.com/openai/codex](https://github.com/openai/codex)

**What is it?**
Codex CLI is OpenAI's open-source terminal coding agent. It runs locally on your computer and uses GPT models to help you code.

**Key features:**
- **Local execution**: Runs in your terminal, no IDE required
- **File operations**: Reads, edits, and creates files
- **Command execution**: Runs shell commands safely
- **OpenAI models**: Uses GPT-4.1, GPT-5, and other OpenAI models
- **Open source**: MIT licensed, fully transparent
- **VS Code integration**: Can be used inside your editor

**How to use it:**
```bash
# Install Codex CLI
npm install -g @openai/codex

# Start it
codex

# Or use specific modes
codex --full-auto    # Let it run commands without approval
codex --suggest      # Only suggest changes, don't apply
```

**Pricing:** Uses your OpenAI API key (GPT-4.1 costs about $2/1M input tokens).

**Best for:** OpenAI ecosystem users, developers who prefer terminal workflows, and teams that want an open-source option.

---

### Gemini CLI

🔗 [https://github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)

**What is it?**
Gemini CLI is Google's open-source AI agent that brings the power of Gemini directly into your terminal. It's built around Gemini's massive 1M+ token context window.

**Key features:**
- **Huge context**: Can read and understand massive codebases at once
- **Google Search grounding**: Searches the web for up-to-date information
- **File operations**: Read, write, and edit files
- **Shell access**: Run commands and scripts
- **Extensions**: Add custom tools and integrations
- **Multi-modal**: Understands images, PDFs, and other file types

**How to use it:**
```bash
# Install Gemini CLI
npm install -g @anthropic-ai/gemini-cli  # (Note: check official docs for exact package)

# Or use via npx
npx @anthropic-ai/gemini-cli

# Start it
gemini

# Ask complex questions
> Analyze the entire codebase and find performance bottlenecks
> Read this PDF and generate a summary
```

**Pricing:** Free tier available, paid plans for heavy usage.

**Best for:** Large codebase refactoring, research-heavy tasks, multi-modal workflows, and Google Cloud users.

---

### Other Agentic Tools

#### Qwen Code
🔗 [https://qwen.ai/qwencode](https://qwen.ai/qwencode)

Open-source terminal agent optimized for Qwen models. Great for users who want to run models locally or use Alibaba's ecosystem.

#### OpenCode
🔗 [https://opencode.ai/](https://opencode.ai/)

An open-source AI coding agent that works with any model. Supports Claude, GPT, Gemini, and custom providers. Completely free with some included models.

#### Cursor
🔗 [https://cursor.sh/](https://cursor.sh/)

A VS Code fork with deep AI integration. Not purely CLI-based, but one of the most popular agentic coding tools. Features include codebase-wide understanding, multi-file editing, and "Agent Mode."

#### Aider
🔗 [https://aider.chat/](https://aider.chat/)

A popular open-source CLI tool for pair programming with AI. Works with any LLM and integrates tightly with Git for version control.

#### Cline (VS Code Extension)
🔗 VS Code Marketplace

An open-source agentic coding assistant that lives inside VS Code. It can read files, run commands, and complete complex tasks autonomously.

---

## 🔧 How to Combine Everything

Here's a real-world example of how you might combine these tools:

### Scenario: A Development Team Building a SaaS Product

**Goal:** Build features quickly, control costs, maintain privacy for sensitive code.

#### Architecture

```
Developer (Cursor / VS Code)
        ↓
Claude Code / Codex CLI (Agentic Tool)
        ↓
MLPal Gateway or CCPG (Gateway)
        ↓
┌──────────────────────────────────────────────┐
│                                              │
│  ├── Anthropic API (Claude Opus 4.7)        │ ← Hard tasks
│  ├── OpenAI API (GPT-4.1)                   │ ← Code review
│  ├── DeepSeek API (DeepSeek R1)             │ ← Simple edits
│  └── Ollama (Llama 3.1 local)              │ ← Sensitive data
│                                              │
└──────────────────────────────────────────────┘
```

#### Setup Steps

**1. Install local inference (Ollama):**
```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull a model for sensitive work
ollama pull llama3.1:70b

# Run it
ollama serve
```

**2. Install a gateway (MLPal):**
```bash
# Clone and start MLPal
git clone https://github.com/mlpal-ai/mlpal-gateway
cd mlpal-gateway
cp .env.oss.example .env
# Edit .env with your provider keys
docker compose up
```

**3. Install an agentic tool (Claude Code):**
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Point it to your gateway
export ANTHROPIC_BASE_URL=http://localhost:8000
export ANTHROPIC_API_KEY=mlpal_sk_...

# Start coding!
claude
```

#### Result
- Simple tasks go through cheap models (DeepSeek) → saves money
- Complex reasoning uses Claude Opus → best quality
- Sensitive code uses local Llama → zero data leakage
- Everything is tracked through the gateway → full cost visibility
- Developer uses Claude Code → fastest workflow

---

## 📚 FAQ & Glossary

### What is an "inference server"?
A program that takes an AI model and makes it generate responses. It's the "engine" that runs the AI.

### What is a "gateway"?
A middleman that routes your requests to the right AI provider. It can also handle cost tracking, caching, and load balancing.

### What is "quantization"?
Making AI models smaller by reducing the precision of their numbers. A 4-bit model is much smaller and faster than a 16-bit model, with only a tiny quality loss.

### What is an "agentic tool"?
An AI that can take actions — not just answer questions. It can read files, edit code, run commands, and complete multi-step tasks.

### What is "MCP" (Model Context Protocol)?
A standard way for AI agents to connect to external tools (databases, APIs, file systems). Think of it as "USB for AI agents."

### What is a "token"?
A unit of text. Roughly 4 characters or 0.75 words in English. AI models charge by the token.

### Should I use local models or cloud APIs?
- **Local models**: Better for privacy, no recurring costs, work offline. Requires good hardware.
- **Cloud APIs**: Better for convenience, no hardware needed, always up-to-date. Costs money per use.
- **Hybrid**: Use local for sensitive work, cloud for hard tasks. Best of both worlds!

### Which inference server should I choose?
- **Beginner**: Ollama (easiest)
- **Power user**: llama.cpp (most flexible)
- **Production**: vLLM (most efficient)
- **Low power**: BitNet (most efficient)

### Which gateway should I choose?
- **Simple setup**: LiteLLM (Python library)
- **Claude Code users**: CCPG (desktop app)
- **Enterprise**: MLPal Gateway (full features)
- **Zero maintenance**: Cloudflare AI Gateway (managed)

### Which agentic tool should I choose?
- **Most powerful**: Claude Code
- **OpenAI ecosystem**: Codex CLI
- **Large codebases**: Gemini CLI
- **Free/open source**: Aider, OpenCode
- **IDE users**: Cursor, Cline

---

## 🔗 Resources

### Official Documentation
- [Anthropic Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code)
- [OpenAI Codex CLI Docs](https://learn.chatgpt.com/docs/codex/cli)
- [Gemini CLI Docs](https://docs.cloud.google.com/gemini/docs/codeassist/gemini-cli)
- [Ollama Documentation](https://ollama.com/docs)
- [vLLM Documentation](https://docs.vllm.ai/)
- [LiteLLM Documentation](https://docs.litellm.ai/)
- [Cloudflare AI Gateway Docs](https://developers.cloudflare.com/ai-gateway/)

### Community
- [r/LocalLLaMA](https://reddit.com/r/LocalLLaMA) — Largest community for running local models
- [r/ClaudeAI](https://reddit.com/r/ClaudeAI) — Claude Code discussions
- [Hugging Face](https://huggingface.co/) — Thousands of free models
- [GitHub Topics: LLM Inference](https://github.com/topics/llm-inference)

### Model Libraries
- [Hugging Face Models](https://huggingface.co/models)
- [Ollama Library](https://ollama.com/library)
- [GGUF Models on Hugging Face](https://huggingface.co/models?search=gguf)

---

## 📝 Contributing

Found a mistake? Have suggestions? This guide is a living document. Feel free to:
- Open an issue with corrections or suggestions
- Submit a pull request with improvements
- Share your own stack configurations

---

## ⚖️ License

This README is released under the MIT License. Feel free to use, modify, and share it however you want.
