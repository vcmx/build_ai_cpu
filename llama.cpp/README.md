# Connect llama.cpp to Claude Code (via LiteLLM)

This guide walks through running a local model with `llama.cpp` and wiring it up to [Claude Code](https://docs.claude.com/en/docs/claude-code/overview), Anthropic's CLI coding assistant.

## Why this is needed

Claude Code sends requests in **Anthropic's API format** (`/v1/messages`). `llama.cpp`'s server only understands the **OpenAI-compatible format** (`/v1/chat/completions`). These schemas aren't compatible, so Claude Code can't talk to `llama-server` directly.

The fix is a small proxy — [LiteLLM](https://github.com/BerriAI/litellm) — that sits in between and translates one format into the other:

```
Claude Code  →  LiteLLM proxy (Anthropic format)  →  llama-server (OpenAI format)
```

> **Heads up:** Claude Code is tuned for Claude's specific tool-calling behavior. Local models — especially smaller ones — may not follow Claude Code's tool-use protocol reliably. Expect basic chat to work well; complex multi-step agentic tasks may be hit or miss depending on the model.

## Prerequisites

- [llama.cpp](https://github.com/ggml-org/llama.cpp) built and working, with a `.gguf` model file
- Python 3.10–3.12 recommended (very new Python versions, e.g. 3.14, have caused dependency issues with LiteLLM — see Troubleshooting)
- Node.js (required by Claude Code)
- [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) installed (`npm install -g @anthropic-ai/claude-code`)

## 1. Install LiteLLM

```bash
pip install 'litellm[proxy]'
```

If you hit a `fastapi`/`get_flat_dependant` import error, see [Troubleshooting](#troubleshooting).

## 2. Start the llama.cpp server

```bash
llama-server -m your-model.gguf --alias "your-model" --host 127.0.0.1 --port 8001 -c 32768 --flash-attn on --n-gpu-layers 99
```

Notes:
- `-c` is the context size. Claude Code sends a fairly large system prompt (tool definitions, etc.) — **use at least 16K–32K context**, or you'll hit `ContextWindowExceededError`. Increase further if your model/hardware supports it.
- `--flash-attn` requires an explicit value (`on`, `off`, or `auto`) on newer llama.cpp builds.
- Add `--api-key <your-key>` if you want to require authentication (recommended once you're forwarding traffic through a proxy).

## 3. Configure LiteLLM

Create `litellm_config.yaml`:

```yaml
model_list:
  - model_name: your-model
    litellm_params:
      model: openai/your-model
      api_base: http://localhost:8001/v1
      api_key: your-llama-server-api-key   # match --api-key above, or any string if none set
```

`model_name` is the identifier Claude Code will request — it must match what you set in step 5.

## 4. Start the LiteLLM proxy

```bash
litellm --config litellm_config.yaml --port 8317
```

Keep this running in its own terminal, separate from `llama-server`. **They must use different ports.**

### Sanity check before connecting Claude Code

```bash
curl http://localhost:8317/v1/messages \
  -H "Content-Type: application/json" \
  -H "x-api-key: anything" \
  -d '{"model":"your-model","max_tokens":50,"messages":[{"role":"user","content":"say hi"}]}'
```

You should get back real model output. If not, fix this before moving on — Claude Code's errors are harder to diagnose than a raw curl failure.

## 5. Point Claude Code at the proxy

Create `.claude/settings.local.json` inside your project folder (this scopes the config to just that folder and won't be committed to git):

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:8317",
    "ANTHROPIC_API_KEY": "anything",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "your-model",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "your-model",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "your-model"
  }
}
```

Then, from that project folder:

```bash
claude
```

If Claude Code still prompts for an Anthropic login on first run (a known quirk on fresh installs), export the same variables in your shell once and retry:

```bash
ANTHROPIC_BASE_URL=http://localhost:8317 ANTHROPIC_API_KEY=anything claude
```

Subsequent launches from that folder should pick up `settings.local.json` automatically.

## Optional: one-click startup script (Windows)

Save as `start_llm_stack.bat` in your model folder, editing the variables at the top to match your setup:

```bat
@echo off
set MODEL_PATH=your-model.gguf
set MODEL_ALIAS=your-model
set LLAMA_PORT=8001
set LITELLM_PORT=8317
set CONTEXT_SIZE=32768
set LITELLM_CONFIG=litellm_config.yaml

echo Starting llama.cpp server on port %LLAMA_PORT% ...
start "llama-server" cmd /k "llama-server -m %MODEL_PATH% --alias %MODEL_ALIAS% -c %CONTEXT_SIZE% --host 127.0.0.1 --port %LLAMA_PORT% --flash-attn on --n-gpu-layers 99"

timeout /t 8 /nobreak >nul

echo Starting LiteLLM proxy on port %LITELLM_PORT% ...
start "litellm-proxy" cmd /k "litellm --config %LITELLM_CONFIG% --port %LITELLM_PORT%"

echo Both servers launching in separate windows.
echo Once both are up, run "claude" in your project folder.
pause
```

## Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `ImportError: cannot import name 'get_flat_dependant' from 'fastapi.dependencies.utils'` | LiteLLM's proxy code expects an older FastAPI internal API | `pip install "fastapi==0.115.6" --force-reinstall`, or use a dedicated virtualenv for LiteLLM |
| `unknown value for --flash-attn: '--n-gpu-layers'` | Newer llama.cpp requires an explicit value for `--flash-attn` | Use `--flash-attn on` (or `off`/`auto`), not a bare flag |
| `litellm.ContextWindowExceededError` | llama-server's `-c` context size is smaller than Claude Code's request | Restart llama-server with a larger `-c` value (16K–32K minimum) |
| Claude Code prompts for login despite `settings.local.json` | Known first-run bug when no `~/.claude.json` exists yet | Export vars in shell for the first run only; later runs read `settings.local.json` normally |
| Missing expression after unary operator `--` (PowerShell) | Using bash-style `\` line continuation in PowerShell | Use backtick `` ` `` for line continuation, or put the command on one line |

## Notes on model choice

Larger, more instruction-tuned local models generally follow Claude Code's tool-calling conventions better than small (≤9B) models. If you see garbled output, ignored instructions, or repeated tool-call failures even after the plumbing above works correctly, that's likely a model-capability limitation rather than a configuration issue.
