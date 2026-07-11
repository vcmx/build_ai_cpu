# Setup
(the orginal repo is at https://github.com/Alishahryar1/free-claude-code)

### 1. Install CLAUDE

Open a standard Windows PowerShell window (you don't even need to run it as Administrator) and paste the following network command to initiate the secure download:

```bash
irm https://claude.ai/install.ps1 | iex
```

### 2. Install the bridge

To host our custom backend proxy and process the data payloads going back and forth, we need a fast, modern environment setup. For this project, we will use uv the blazing-fast Python package and environment installer written in Rust alongside Python 3.14. The following downloads the necessary bridge server files.


```bash
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
uv self update
uv python install 3.14
```

### 3. Fix permission issues if installation fails

A permission error may appear during installation and recommends manually installing the official Claude Code CLI first with elevated permissions.

### 4. Set-up the local bridge server

We will use uv to install this project directly as a global command-line tool. Open your PowerShell terminal and execute the following uv command to fetch, build, and install the proxy server straight from the GitHub source:

```bash
uv tool install --force git+https://github.com/Alishahryar1/free-claude-code.git
```

Once the installation finishes, you can start the proxy server right from any terminal window by running: 

```bash
fcc-server
```

As soon as the engine successfully boots up, look closely at your PowerShell console output. The local network links will be printed cleanly on your screen: 

```bash
Server URL: http://127.0.0.1:8082
Admin UI: http://127.0.0.1:8082/admin
```

### 5. Create an NVIDIA NIM API key

Now that your local machine has the runtime requirements installed, we need to secure the "brains" of our operation. Instead of using Anthropic's paid infrastructure, we will use NVIDIA NIM (NVIDIA Inference Microservices), which lets us access incredibly fast, open-source LLMs hosted on NVIDIA's enterprise GPUs completely for free.Device Drivers

Important: Copy the generated key (nvapi-...) immediately and save it in a temporary text file. For security reasons, NVIDIA will only show you this key once, and it will be hidden as soon as you close the popup window.Sound & Video Cards

Relevant links:

- [NVIDIA Build](https://build.nvidia.com/explore/discover)
- [NVIDIA API keys](https://build.nvidia.com/settings/api-keys)

### 6. Configure CLAUDE settings and apply NVIDIA NIM

To make the official Claude Code CLI communicate with our local proxy instead of hitting Anthropic’s paid servers, we need to declare the environment variables. While you can type these variables manually every time you launch the terminal, it is much easier to save them directly inside your Claude Global Settings file so they load automatically.

Claude Code stores its system configuration inside your Windows user directory.AI Tools, Chatbots & Virtual Assistants

Press Win + R, type %USERPROFILE%\.claude and press Enter. (Alternatively, navigate to C:\Users\YourUsername\.claude\).

Inside this folder, look for a file named settings.json. If it doesn't exist, create a new text file and rename it exactly to settings.json.

Open the file using Notepad or VS Code.

Add the Proxy Redirection Rules: Paste the following JSON block into the file. If you already have settings configured, append the "env" object inside your root brackets. 

```bash
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:8082",
    "ANTHROPIC_AUTH_TOKEN": "freecc",
    "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1"
  }
}
```
### 7. Launch Claude Code through the bridge
start the fcc server
```bash
fcc-server
```
Then run the Claude launcher from the bridge, trust the folder, and test with a simple prompt.

```bash
fcc-claude
```

Inside Claude Code, you can use `/model` to choose among models exposed by the bridge. The upstream repo explicitly states that the native Claude Code model picker works with FCC gateway models.

## Path B: Direct Claude Code to NIM

NVIDIA's official documentation says Claude Code can use NIM as its backend through the Anthropic-compatible `/v1/messages` endpoint, provided the deployed model supports tool calling. [page:1]

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
