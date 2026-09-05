# Open WebUI — Self-Hosted ChatGPT Alternative on Railway (One-Click Deploy)

Self-host Open WebUI on Railway — private multi-model AI chat for OpenAI, Claude, and Ollama with built-in RAG.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/open-web-ui?referralCode=zxcgoT)

## Contents

- [What Is Open WebUI?](#what-is-open-webui)
- [What This Railway Template Deploys](#what-this-railway-template-deploys)
- [Why Self-Host Open WebUI Instead of ChatGPT Plus](#why-self-host-open-webui-instead-of-chatgpt-plus)
- [Deploy to Railway](#deploy-to-railway)
- [Self-host with Docker Compose](#self-host-with-docker-compose)
- [Configuration](#configuration)
- [What You Can Build With This Open WebUI Template](#what-you-can-build-with-this-open-webui-template)
- [Troubleshooting Open WebUI](#troubleshooting-open-webui)
- [FAQ](#faq)

## What Is Open WebUI?

Open WebUI is an open-source, self-hosted web interface for large language models. It offers a familiar chat experience over any combination of OpenAI, Anthropic Claude, and locally-run Ollama models, and adds multi-user accounts, role-based access control, a built-in RAG engine for chatting with your own documents, and a tool runner.

## What This Railway Template Deploys

| Service | Image | Purpose |
|---|---|---|
| Open WebUI | `ghcr.io/open-webui/open-webui:main` | Multi-model AI chat interface with user management, RAG engine and tool runner, listening on port 8080 |

**Persistent volume:** `/app/backend/data` — chat history, user accounts, uploaded documents, model configuration and RAG vector data. Without a mounted volume, all of this is lost on redeploy.

## Why Self-Host Open WebUI Instead of ChatGPT Plus

Single container on Railway's Hobby plan — you pay only for compute plus whatever your chosen LLM provider charges. Self-hosting replaces a per-seat ChatGPT Plus subscription; bring-your-own-key means usage is billed at API rates instead of a flat monthly seat fee.

| | Open WebUI (self-hosted) | ChatGPT Plus | Claude Pro | LibreChat |
|---|---|---|---|---|
| Pricing model | Railway compute + API usage | Flat monthly fee per seat | Flat monthly fee per seat | Self-hosted, infrastructure only |
| Control | Fully self-hosted | OpenAI-hosted; models limited to OpenAI | Anthropic-hosted | Fully self-hosted |
| Where alternatives win | — | No setup, official support, first-party model access | No setup, official support | Comparable open-source alternative with its own feature set |

## Deploy to Railway

1. Click **Deploy on Railway** below.
2. Set at least one LLM backend variable: `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `OLLAMA_BASE_URL`.
3. Set `WEBUI_SECRET_KEY` to a stable random value (e.g. `openssl rand -hex 32`) so sessions survive restarts.
4. Deploy and open the generated Railway URL.
5. Create your admin account, then set `ENABLE_SIGNUP=false` to close public registration.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/open-web-ui?referralCode=zxcgoT)

## Self-host with Docker Compose

```bash
git clone https://github.com/sahilrupani/open-webui-railway-template.git
cd open-webui-railway-template
cp .env.example .env
docker compose up -d
```

Open `http://localhost:8080` (or the port defined in your `.env`) once the container is up.

## Configuration

| Variable | Required | Description |
|---|---|---|
| `WEBUI_SECRET_KEY` | Yes | Session signing secret. Generate 32+ random characters with `openssl rand -hex 32` |
| `OPENAI_API_KEY` | No | OpenAI API key for GPT model access. At least one LLM backend is required |
| `ANTHROPIC_API_KEY` | No | Anthropic Claude API key |
| `OLLAMA_BASE_URL` | No | Ollama service URL for local LLM inference, e.g. `http://ollama:11434` |
| `DEFAULT_MODELS` | No | Comma-separated default model IDs |
| `ENABLE_SIGNUP` | No | Set to `false` to disable self-registration after creating your admin account |
| `WEBUI_AUTH` | No | Set to `false` to disable authentication entirely (only for trusted private networks) |
| `ENABLE_RAG_WEB_SEARCH` | No | Set to `true` to enable web search in RAG |

## What You Can Build With This Open WebUI Template

- A private, self-hosted ChatGPT alternative for a team, with per-user accounts and role-based access
- One chat interface in front of several providers at once — OpenAI, Anthropic Claude, and local Ollama models
- Retrieval-augmented generation over internal documents using the built-in RAG engine
- Running fully offline inference by pointing `OLLAMA_BASE_URL` at a local Ollama instance
- Keeping conversation history and uploaded documents on infrastructure you control rather than a vendor's

## Troubleshooting Open WebUI

**Logged out on every redeploy, or sessions silently invalidated** — `WEBUI_SECRET_KEY` is unset or changes between deploys, so session cookies can no longer be verified. Set it to a fixed random value.

**Chat history, accounts and uploaded documents disappear after a redeploy** — No persistent volume mounted at `/app/backend/data`. Everything Open WebUI stores lives there.

**No models appear in the model picker** — No LLM backend is configured. Set `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `OLLAMA_BASE_URL` — at least one is required.

**Anyone with the URL can register an account** — Self-registration is open by default. Set `ENABLE_SIGNUP=false` after creating your admin account.

## FAQ

### Do I need an OpenAI or Anthropic API key to use this template?

No, but you need at least one LLM backend configured — `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `OLLAMA_BASE_URL` for local/self-hosted models via Ollama.

### Will I lose my chats if the Railway service redeploys?

Only if no persistent volume is mounted at `/app/backend/data`. With the volume attached, chat history, accounts, uploads, and RAG vector data persist across redeploys.

### How do I stop random people from creating accounts on my instance?

Create your own admin account first, then set `ENABLE_SIGNUP=false` to close public registration.

### Can I run this fully offline with local models only?

Yes — set `OLLAMA_BASE_URL` to point at a local Ollama instance and skip the OpenAI/Anthropic keys entirely.

### Where do I find licensing information?

This template deploys the upstream Open WebUI project. Refer to the [upstream repository](https://github.com/open-webui/open-webui) for license terms.

---

*This is a community-maintained Railway template for [Open WebUI](https://github.com/open-webui/open-webui). Not affiliated with the Open WebUI project, Railway, OpenAI, or Anthropic.*