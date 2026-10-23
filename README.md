# MCP Security Sanitizer

A small MCP server that provides local, regex-based tools for redacting common
secrets and personally identifiable information (PII).

> Regex matching only recognizes the patterns implemented here. It cannot
> guarantee detection of every secret or PII value. Validate the patterns
> against your requirements before relying on this service.

## Tools

| Tool | Behavior |
| --- | --- |
| `mask_api_keys_and_secrets(text)` | Redacts OpenAI-style `sk-...` keys, bearer tokens, and common named key/secret/password/token assignments. |
| `mask_pii_data(text)` | Redacts email addresses and common North American phone numbers. |
| `audit_text_for_risks(text)` | Counts recognizable secrets, email addresses, and phone numbers. |

## Run locally

Requires Python 3.11+ and [uv](https://docs.astral.sh/uv/).

```bash
uv sync
uv run server.py
```

The server listens on `http://localhost:8000/sse`.

## Deploy on Render

1. Push this repository to GitHub and create a **Web Service** on Render from
   the repository. Render can use the included `render.yaml` configuration.
2. Deploy the service. Render assigns the public URL and port automatically.
3. Connect an MCP client to `https://<your-service>.onrender.com/sse`.

The service is public and has no authentication. Anyone with its URL can call
the sanitizer tools, so do not send real credentials or sensitive personal
data unless you add suitable access controls.

## Development

Sanitization runs locally in Python and does not call an LLM. Add tests for the
secret and PII formats relevant to your environment before relying on the
service for compliance.
