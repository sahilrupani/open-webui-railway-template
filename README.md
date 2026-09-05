# Deploy and Host Open WebUI on Railway – Private Multi-Model AI Chat

Open WebUI is an open-source, self-hosted web interface for large language models. It offers a familiar chat experience over any combination of OpenAI, Anthropic Claude, and locally-run Ollama models, and adds multi-user accounts, role-based access control, a built-in RAG engine for chatting with your own documents, and a tool runner.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/open-web-ui?referralCode=zxcgoT&utm_medium=integration&utm_source=template&utm_campaign=generic)

## 🚀 Quick Start Deployment Guide

### Step 1: Deploy on Railway
1. Click **Deploy on Railway** above
2. Wait for the service to finish building (~2–3 minutes)

### Step 2: Mount a volume
1. Add a Railway Volume mounted at `/app/backend/data`
2. Chat history, user accounts, uploaded documents and RAG vectors all live there — without it everything resets on redeploy

### Step 3: Set your session secret
1. Generate a value with `openssl rand -hex 32`
2. Set it as `WEBUI_SECRET_KEY`
3. This must stay fixed — if it changes between deploys every user is logged out

### Step 4: Add at least one model provider
1. Set `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or point `OLLAMA_BASE_URL` at an Ollama instance
2. At least one backend is required or the model picker will be empty
3. See the API key guides below
4. Redeploy to apply

### Step 5: Create your admin account
1. Open your Railway URL
2. Sign up — the first account created becomes the administrator
3. Verify a model appears in the picker and send a test message

### Step 6: Close public registration
1. Set `ENABLE_SIGNUP=false` in the Variables tab
2. Redeploy — otherwise anyone with your URL can create an account

## About Hosting Open WebUI

This template deploys Open WebUI as a single container on Railway from the official `ghcr.io/open-webui/open-webui:main` image, listening on port 8080. It exists so you can get a private, multi-model chat interface running behind your own login in minutes, without hand-rolling the Docker setup, volume mounts, or secret management yourself.

A persistent volume at `/app/backend/data` holds chat history, user accounts, uploaded documents, model configuration and RAG vector data — without it, all of that is lost on every redeploy. The deployment gives you a fully self-hosted alternative to ChatGPT Plus or Claude Pro: your conversations and documents stay on infrastructure you control, while you bring your own API keys (or run fully local models via Ollama) instead of paying a flat per-seat subscription.

## Common Use Cases

- **Private team ChatGPT alternative**: a self-hosted chat interface for a team, with per-user accounts and role-based access
- **Multi-provider chat**: one interface in front of several providers at once — OpenAI, Anthropic Claude, and local Ollama models
- **Document Q&A**: retrieval-augmented generation over internal documents using the built-in RAG engine
- **Fully offline inference**: point `OLLAMA_BASE_URL` at a local Ollama instance and run without any external API
- **Data ownership**: keep conversation history and uploaded documents on infrastructure you control rather than a vendor's

## Dependencies for Open WebUI Hosting

### Deployment Dependencies
- [Open WebUI (upstream source)](https://github.com/open-webui/open-webui)
- [Open WebUI documentation](https://docs.openwebui.com/)
- [Anthropic API keys (Claude)](https://platform.claude.com/)
- [OpenAI API keys (GPT)](https://platform.openai.com/)
- [Ollama — for local model inference](https://ollama.com/)

## 🔑 How to Get API Keys for Different AI Providers

### How to Get an Anthropic Claude API Key? (Recommended)
1. Visit the Anthropic Console at https://platform.claude.com/
2. Sign up or log in, then open **API Keys** in the left sidebar
3. Click **Create Key** and give it a name
4. Copy the key — it is shown only once

### How to Get an OpenAI API Key?
1. Go to the OpenAI Platform at https://platform.openai.com/
2. Create an account or sign in, then open **API keys** from the profile menu
3. Click **Create new secret key** and name it
4. Copy the key — it is shown only once

## ⚙️ Configuration

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

## 🐳 Self-Host with Docker Compose

```bash
git clone https://github.com/sahilrupani/open-webui-railway-template
cd open-webui-railway-template
cp .env.example .env
```

Generate a session secret and add it to `.env` as `WEBUI_SECRET_KEY`:

```bash
openssl rand -hex 32
```

Then bring it up:

```bash
docker compose up -d
```

Open `http://localhost:8080` in your browser and sign up — the first account created becomes the administrator.

## ❓ Frequently Asked Questions (FAQ)

### How much does it cost to run Open WebUI on Railway?
One container on Railway's Hobby plan ($5/month base plus usage), plus whatever your model provider charges against your own API key. Self-hosting replaces a per-seat ChatGPT Plus subscription with usage-rate API billing, which is cheaper for light and moderate use and can be more expensive for very heavy use.

### Is my data private and secure?
Yes. Open WebUI is fully self-hosted — conversations, accounts and uploaded documents stay in your own volume on Railway. Your prompts still reach whichever model provider you configure, so choose Ollama for fully local inference.

### Can I use several providers at once?
Yes. Set OpenAI, Anthropic and Ollama credentials together and switch between their models from the picker inside a single chat interface.

### Can I run it fully offline with no external API?
Yes — point `OLLAMA_BASE_URL` at an Ollama instance and use only local models. No prompt data leaves your infrastructure.

### Can I migrate off Railway later?
Yes. This repo ships a compose file that runs the same image anywhere Docker does; copy the `/app/backend/data` volume across and your history and accounts come with it.

### Why am I logged out on every redeploy, or why do sessions get silently invalidated?
`WEBUI_SECRET_KEY` is unset or changes between deploys, so session cookies can no longer be verified. Set it to a fixed random value.

### Why do chat history, accounts and uploaded documents disappear after a redeploy?
No persistent volume is mounted at `/app/backend/data`. Everything Open WebUI stores lives there — mount a Railway Volume at that path.

### Why does no model appear in the model picker?
No LLM backend is configured. Set `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `OLLAMA_BASE_URL` — at least one is required.

### Why can anyone with the URL register an account?
Self-registration is open by default. Set `ENABLE_SIGNUP=false` after creating your admin account.

## 🛠️ Support & Issues

For problems with this template, open an issue at [https://github.com/sahilrupani/open-webui-railway-template/issues](https://github.com/sahilrupani/open-webui-railway-template/issues) and include a description of the problem, steps to reproduce it, and relevant deploy/runtime logs.

---

*This is a community-maintained Railway template built on top of [Open WebUI](https://github.com/open-webui/open-webui). It is not affiliated with, endorsed by, or supported by the Open WebUI project, Railway, or any AI model provider.*