# CLI Commands Cheatsheet
### Agentic AI with LangGraph, Llama Stack & Langfuse

Keep this open alongside the [Red Hat workshop](https://github.com/rh-aiservices-bu/agentic-workshop). All commands assume you are in the project root unless noted.

---

## Setup

```bash
# Clone the workshop repo
git clone https://github.com/rh-aiservices-bu/agentic-workshop
cd agentic-workshop

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate          # Mac / Linux
.venv\Scripts\activate             # Windows

# Install dependencies
pip install -r backend/requirements.txt

# Copy and edit environment variables
cp .env.example .env
```

---

## Run the Agent

```bash
# Start the FastAPI + LangGraph agent server
cd backend
python 6-langgraph-langfuse-fastapi-chatbot.py

# Server runs on http://localhost:8002
```

---

## Chat with the Agent

```bash
# Basic question
curl -X POST http://localhost:8002/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "who does Thomas Hardy work for?"}'

# Finance query
curl -X POST http://localhost:8002/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "what is the account balance for Thomas Hardy?"}'

# Pretty-print JSON response (requires jq)
curl -s -X POST http://localhost:8002/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "who does Thomas Hardy work for?"}' | jq .
```

---

## Run Evaluations

```bash
# Trigger a named evaluation run
curl -X POST http://localhost:8002/evaluate \
  -H "Content-Type: application/json" \
  -d '{"run_name": "test-run"}'

# Run with a custom name (useful for comparing runs in Langfuse)
curl -X POST http://localhost:8002/evaluate \
  -H "Content-Type: application/json" \
  -d '{"run_name": "my-experiment-v2"}'
```

---

## Health & Debug

```bash
# Check the server is up
curl http://localhost:8002/health

# View live server logs (if running in background)
tail -f backend/agent.log

# Check which Python environment is active
which python
python --version
```

---

## Langfuse (Self-hosted only)

```bash
# Start Langfuse locally with Docker Compose
docker compose up -d

# Check Langfuse is running
curl http://localhost:3000

# Stop Langfuse
docker compose down
```

---

## OpenShift / Kubernetes Deploy

```bash
# Log in to your OpenShift cluster
oc login --token=<your-token> --server=<your-server>

# Create a new project
oc new-project agentic-ai

# Apply manifests
oc apply -f k8s/

# Check pod status
oc get pods

# Stream logs from the agent pod
oc logs -f deployment/agentic-agent

# Expose the service
oc expose svc/agentic-agent
```

---

## Useful One-liners

```bash
# Watch Langfuse trace count update in real time (requires jq + watch)
watch -n 5 'curl -s http://localhost:3000/api/public/traces \
  -u $LANGFUSE_PUBLIC_KEY:$LANGFUSE_SECRET_KEY | jq ".total"'

# Deactivate virtual environment when done
deactivate
```
