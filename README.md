# Agentic AI with LangGraph, Llama Stack & Langfuse

This project demonstrates a **production-style AI agent system** built with:

- LangGraph (agent orchestration)
- Llama Stack (vLLM inference)
- MCP (Model Context Protocol) tools
- Langfuse observability (Traces, Evals, Feedback)

---

## Architecture Overview

User → FastAPI → LangGraph Agent → MCP Tools → LLM → Langfuse

---

## Key Features

### Trace Observability
- Full reasoning chain (input, LLM, tools, output)

### Trace Details
- Inspect tool inputs/outputs and intermediate steps

### Evaluations
- Keyword-based automated testing
- Dataset-driven scoring

### Tool Integration
- Customer and finance data via MCP

### Feedback Loop
- User comments attached to traces

### Chat UI
- Interactive interface for testing the agent

---

## How to Run

### 1. Clone
```bash
git clone https://github.com/uni85/agentic-ai-langfuse-demo
cd agentic-ai-langfuse-demo
```

### 2. Setup
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements.txt
```

### 3. Configure
Create `.env` file from `.env.example`

### 4. Run agent
```bash
cd backend
python 6-langgraph-langfuse-fastapi-chatbot.py
```

### 5. Test
```bash
curl -X POST http://localhost:8002/chat   -H "Content-Type: application/json"   -d '{"message":"who does Thomas Hardy work for?"}'
```

### 6. Run evaluation
```bash
curl -X POST http://localhost:8002/evaluate   -H "Content-Type: application/json"   -d '{"run_name":"test-run"}'
```

## 🧩 Tech Stack

- LangGraph
- Llama Stack (vLLM)
- FastAPI
- Langfuse
- OpenShift / Kubernetes
- MCP

---

## Shout out to redhat team for their amazing 

---

⭐ Star the repo if you found it useful!
