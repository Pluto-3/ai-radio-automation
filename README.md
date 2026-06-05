# AI Radio Automation

An automated content pipeline that generates broadcast-ready radio scripts from live news — no human intervention required after setup.

## What it does

Every hour, the system:
1. Pulls the latest headlines from BBC News
2. Passes each story to a local AI model
3. Outputs a polished, broadcast-ready radio script per story — saved and ready to air

## How it works

Built on **n8n** — an open-source automation platform — wired together with a locally running AI model via **Ollama**. No data leaves your infrastructure. Everything runs on your own machine.

```
Schedule (hourly)
  → BBC News RSS Feed
    → AI Script Generation (Mistral via Ollama)
      → Timestamped script files saved to disk
```

## Example output

```
Good morning, you're tuned in to your daily briefing.

Breaking news — UK Prime Minister confirms emergency economic 
measures following overnight market turbulence. The Chancellor 
is expected to address Parliament later today with a full 
statement. Markets are watching closely as the pound steadies 
against the dollar. We'll bring you updates as they develop. 
Stay tuned.
```

## Stack

| Layer | Tool |
|---|---|
| Automation | n8n |
| AI Model | Mistral (via Ollama) |
| News Source | BBC News RSS |
| Runtime | Docker |

## Setup

### Prerequisites
- Docker
- Ollama with `mistral:latest` pulled

### 1. Configure Ollama to accept external connections

```bash
bash fix-ollama.sh
```

### 2. Start n8n

```bash
docker run -d \
  --name n8n \
  --add-host=host.docker.internal:host-gateway \
  -p 5678:5678 \
  -v $(pwd)/n8n:/home/node/.n8n \
  -e N8N_SECURE_COOKIE=false \
  -e NODE_FUNCTION_ALLOW_BUILTIN=fs,path \
  docker.n8n.io/n8nio/n8n
```

### 3. Import the workflow

Open `http://localhost:5678`, import `ai-radio-workflow.json`, and activate it.

## What's next

This pipeline is the foundation. Planned extensions:

- **Text-to-Speech** — convert scripts to audio automatically
- **Multi-source feeds** — pull from multiple news sources simultaneously
- **Live streaming integration** — pipe directly into a broadcast platform
- **Custom voice & tone** — adjust AI persona per station or format
