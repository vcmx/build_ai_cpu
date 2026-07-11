# Video to Repo Mapping

This file translates the video into repo tasks.

## Mapped steps

| Video step | Repo task |
|---|---|
| Install Node.js first. [web:34] | Document prerequisite checks in `SETUP.md`. [code_file:46] |
| Run the `free-claude-code` installer. [web:34][page:2] | Add bridge install commands for Linux/macOS and Windows. [code_file:46] |
| If permissions fail, install Claude Code manually and rerun installer. [web:34] | Add a permission-fix section with a global npm install. [code_file:46] |
| Start the local FCC server. [web:34][page:2] | Add `fcc-server` launch step and admin UI note. [code_file:46] |
| Generate NVIDIA NIM API key. [web:34][page:2] | Add NVIDIA Build links and provider setup instructions. [code_file:46] |
| Paste key, validate, and apply. [web:34][page:2] | Document Admin UI configuration. [code_file:46] |
| Launch Claude through the bridge and test it. [web:34][page:2] | Add `fcc-claude` launch instructions. [code_file:46] |
| Optional direct NIM setup. [page:1] | Add official environment-variable path. [code_file:46] |
