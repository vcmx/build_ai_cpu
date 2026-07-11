# NIM + Claude Code Setup Repo

This repo converts the steps from the referenced video (https://www.youtube.com/watch?v=-arGuuGpeHM) nto a GitHub-style setup guide for running Claude Code with NVIDIA NIM. The video shows a workflow built around installing Node.js, installing the `free-claude-code` bridge, starting its local server, generating an NVIDIA NIM API key, validating the provider in the local admin panel, and then launching Claude Code through the bridge. [web:34][page:2]

It is important to note that this workflow does **not** give direct free access to Anthropic-hosted Claude models. Instead, it routes Claude Code to compatible alternative models through NVIDIA NIM, which matches both the video explanation and the upstream `free-claude-code` repository. [web:34][page:2]

## Repo contents

- `SETUP.md` — end-to-end setup steps.
- `docs/TROUBLESHOOTING.md` — common issues and fixes.
- `scripts/env-example.sh` — example environment exports for direct NIM usage.
- `.github/workflows/markdown-lint.yml` — markdown QA workflow.

## The Key Steps

The indexed transcript for the video shows these core steps: install Node.js, install `free-claude-code`, manually install Claude Code with `sudo` if permissions fail, rerun the bridge installer, start the bridge server, open the local admin UI, create an NVIDIA NIM API key at build.nvidia.com, paste and validate the key, then launch Claude Code through the bridge and test it.

## Two setup paths

This repo includes two practical paths:

1. **Video-faithful bridge setup** using `free-claude-code`, which matches the video most closely. [web:34][page:2]
2. **Direct NIM environment-variable setup** using NVIDIA's official Claude Code integration docs, which show how to point Claude Code at a NIM deployment through the Anthropic-compatible endpoint and how to map Claude aliases to the active NIM model.
