# AI Project

A small FastAPI service that forwards chat requests to an OpenAI-compatible API.

## Requirements

- Python 3.10 or later
- An API key for your AI provider

## Setup

Create and activate a virtual environment, then install the project:

```bash
python -m venv .venv
```

Activate it (`.venv\\Scripts\\activate` on Windows, or `source .venv/bin/activate` on macOS/Linux), then run:

```bash
pip install -e .
```

Copy `.env.example` to `.env` and configure the provider settings:

```dotenv
AI_API_KEY=your-api-key
AI_BASE_URL=https://api.openai.com/v1
AI_MODEL=gpt-4o-mini
```

The application reads these values from the process environment. Load `.env` into your shell with your preferred environment-file tool, or set the variables directly.

## Run

Start the development server:

```bash
uvicorn app.main:app --reload
```

The interactive API documentation is available at `http://127.0.0.1:8000/docs`.

## Endpoints

### `GET /health`

Returns `{"status":"ok"}` when the service is running.

### `POST /chat`

Send a message:

```bash
curl -X POST http://127.0.0.1:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello"}'
```

Example response:

```json
{"reply":"Hello! How can I help?"}
```

If `AI_API_KEY` is missing, `/chat` returns HTTP 503. If the upstream provider request fails, it returns HTTP 502.

## Configuration

| Variable | Default | Description |
| --- | --- | --- |
| `AI_API_KEY` | *(required)* | API key sent as a Bearer token |
| `AI_BASE_URL` | `https://api.openai.com/v1` | Base URL for an OpenAI-compatible API |
| `AI_MODEL` | `gpt-4o-mini` | Model identifier sent to the provider |
