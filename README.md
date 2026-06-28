# Agentic AI with LangGraph, Llama Stack & Langfuse

A production-style AI agent system demonstrating end-to-end observability, evaluation, and feedback using open-source tooling on OpenShift / Kubernetes.

> **Based on the Red Hat Agentic AI Workshop** → [rh-aiservices-bu/agentic-workshop](https://github.com/rh-aiservices-bu/agentic-workshop)

---

## Architecture

```
User → FastAPI → LangGraph Agent → MCP Tools → LLM (vLLM / Llama Stack)
                                                       ↓
                                                   Langfuse
                                         (Traces · Evals · Feedback)
```

---

## Features

### 🔍 Trace Observability
Full reasoning chain captured — inputs, LLM calls, tool invocations, and final outputs.

![Trace Overview](docs/trace-overview.png)

### 🔎 Trace Details
Inspect every intermediate step: tool inputs/outputs, latency, and token usage.

![Trace Details](docs/trace-details.png)

### 🛠️ Tool Integration
Customer and finance data accessed via MCP (Model Context Protocol) servers.

![Tools Usage](docs/tools-usage.png)

### ✅ Evaluations
Keyword-based automated testing and dataset-driven scoring — all surfaced in Langfuse.

![Eval Datasets](docs/eval-datasets.png)
![Eval Test Case](docs/eval-test-case.png)

### 💬 Feedback Loop
User comments attached directly to traces for human-in-the-loop review.

![Feedback Comment](docs/feedback-comment.png)

### 🖥️ Chat UI
Interactive interface for testing the agent end-to-end.

![Chat UI](docs/chat-ui.png)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Agent orchestration | [LangGraph](https://github.com/langchain-ai/langgraph) |
| LLM inference | [Llama Stack](https://github.com/meta-llama/llama-stack) + vLLM |
| Tool protocol | [MCP (Model Context Protocol)](https://modelcontextprotocol.io) |
| Observability | [Langfuse](https://langfuse.com) |
| API server | [FastAPI](https://fastapi.tiangolo.com) |
| Deployment | OpenShift / Kubernetes |

---

## Prerequisites

- Python 3.11+
- A running Llama Stack / vLLM inference endpoint
- A Langfuse account ([cloud](https://cloud.langfuse.com) or [self-hosted](https://langfuse.com/docs/deployment/self-host))
- Access to an OpenShift or Kubernetes cluster (optional, for production deploy)

---

## Getting Started

### 1. Clone

```bash
git clone https://github.com/uni85/agentic-ai-langfuse-demo
cd agentic-ai-langfuse-demo
```

### 2. Set up Python environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements.txt
```

### 3. Configure environment

```bash
cp .env.example .env
# Edit .env and fill in your values
```

Key variables:

```
LANGFUSE_PUBLIC_KEY=pk-...
LANGFUSE_SECRET_KEY=sk-...
LANGFUSE_HOST=https://cloud.langfuse.com   # or your self-hosted URL
LLAMA_STACK_URL=http://localhost:8321
MCP_SERVER_URL=http://localhost:8001
```

### 4. Run the agent

```bash
cd backend
python 6-langgraph-langfuse-fastapi-chatbot.py
```

The FastAPI server starts on `http://localhost:8002`.

### 5. Chat with the agent

```bash
curl -X POST http://localhost:8002/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "who does Thomas Hardy work for?"}'
```

### 6. Run evaluations

```bash
curl -X POST http://localhost:8002/evaluate \
  -H "Content-Type: application/json" \
  -d '{"run_name": "test-run"}'
```

---

## Project Structure

```
├── backend/
│   ├── 6-langgraph-langfuse-fastapi-chatbot.py   # Main agent + API server
│   ├── requirements.txt
│   └── mcp/                                       # MCP tool servers
├── docs/                                          # Screenshots
├── .env.example
└── README.md
```

---

## Credits

Built on top of the [Red Hat Agentic AI Workshop](https://github.com/rh-aiservices-bu/agentic-workshop) — Shout out to the Red Hat AI Services team for the foundational work.

---
