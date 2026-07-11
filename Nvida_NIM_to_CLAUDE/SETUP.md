# Setup

## Path A: Follow the video workflow

### 1. Install prerequisites

The video transcript explicitly starts by installing Node.js because it is required for the setup. The `free-claude-code` project also states that its installer provisions uv and Python 3.14, but you should install Node.js first so `npm` is available for Claude Code or Codex when needed. [web:34][page:2]

Suggested checks:

```bash
node --version
npm --version
python3 --version
```

### 2. Install the bridge

The video uses the `free-claude-code` installer. On Linux/macOS, the upstream repo documents this installer command: [page:2]

```bash
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh
```

On Windows PowerShell, the upstream repo documents: [page:2]

```powershell
irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1" | iex
```

### 3. Fix permission issues if installation fails

The video transcript states that a permission error may appear during installation and recommends manually installing the official Claude Code CLI first with elevated permissions, then rerunning the `free-claude-code` installer. [web:34]

A practical sequence is:

```bash
# install Claude Code first if needed
sudo npm install -g @anthropic-ai/claude-code

# then rerun the bridge installer
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh
```

### 4. Start the local bridge server

The video transcript says to start the local server and open the URL shown in the terminal. The current upstream repo documents the command as `fcc-server` and says it exposes a local-only Admin UI such as `http://127.0.0.1:8082/admin`. [web:34][page:2]

```bash
fcc-server
```

### 5. Create an NVIDIA NIM API key

The video directs users to NVIDIA Build, where they sign in, generate an API key, and copy it back into the local admin panel. The upstream project also documents creating the key at the NVIDIA API key page and then entering it into `NVIDIA_NIM_API_KEY` in the Admin UI. [web:34][page:2]

Relevant links:

- [NVIDIA Build](https://build.nvidia.com/explore/discover)
- [NVIDIA API keys](https://build.nvidia.com/settings/api-keys)

### 6. Validate and apply NVIDIA NIM

According to the video transcript, the key is pasted into the NVIDIA NIM field, then validated and applied in the admin panel. The upstream repo describes the same process and notes that you can leave `MODEL` at the default or select another provider-prefixed model. [web:34][page:2]

### 7. Launch Claude Code through the bridge

The video transcript says to open a terminal, run the Claude launcher from the bridge, trust the folder, and test with a simple prompt. The current upstream repo documents the launcher as `fcc-claude`. [web:34][page:2]

```bash
fcc-claude
```

Inside Claude Code, you can use `/model` to choose among models exposed by the bridge. The upstream repo explicitly states that the native Claude Code model picker works with FCC gateway models. [page:2]

## Path B: Direct Claude Code to NIM

NVIDIA's official documentation says Claude Code can use NIM as its backend through the Anthropic-compatible `/v1/messages` endpoint, provided the deployed model supports tool calling. [page:1]

### 1. Start or obtain a NIM deployment

You need a chat-capable NIM deployment and the active model name. NVIDIA's docs warn that Claude Code requires tool calling, so the selected model must support tool use. [page:1]

### 2. Export the environment variables

Use environment variables to point Claude Code at the NIM endpoint. NVIDIA's documented example is: [page:1]

```bash
export ANTHROPIC_BASE_URL="http://${NIM_ENDPOINT}:${NIM_SERVER_PORT}"
export ANTHROPIC_API_KEY="not-used"
export ANTHROPIC_CUSTOM_MODEL_OPTION="${MODEL_NAME}"
claude
```

NVIDIA also explains that `NIM_SERVER_PORT` is `8000` by default and that `ANTHROPIC_API_KEY` can be any non-empty string because NIM does not validate it. [page:1]

### 3. Map built-in Claude aliases

NVIDIA's troubleshooting section explains that Claude Code may still use built-in aliases such as `haiku`, `sonnet`, or `opus`, which can cause model 404 errors if they still resolve to Anthropic model IDs. The documented fix is to map those aliases to the same NIM model name. [page:1]

```bash
export ANTHROPIC_API_KEY="not-used"
export ANTHROPIC_BASE_URL="http://${NIM_ENDPOINT}:${NIM_SERVER_PORT}"
export ANTHROPIC_CUSTOM_MODEL_OPTION="${MODEL_NAME}"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="${MODEL_NAME}"
export ANTHROPIC_DEFAULT_OPUS_MODEL="${MODEL_NAME}"
export ANTHROPIC_DEFAULT_SONNET_MODEL="${MODEL_NAME}"
export CLAUDE_CODE_SUBAGENT_MODEL="${MODEL_NAME}"
```

### 4. Launch Claude Code

After exporting the variables, start Claude Code normally. NVIDIA's docs present this as the official direct integration path. [page:1]
