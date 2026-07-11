# Troubleshooting

## Permission error during install

The video transcript explicitly mentions a permission error during the `free-claude-code` install and recommends installing the official Claude Code CLI first, then rerunning the installer. [web:34]

Example fix:

```bash
sudo npm install -g @anthropic-ai/claude-code
```

## Claude Code connects but model calls fail with 404

NVIDIA's official docs say this can happen when Claude Code still uses built-in aliases like `haiku`, `sonnet`, or `opus`, which NIM does not serve under Anthropic model IDs. The fix is to map those aliases to the active NIM model name with the documented environment variables. [page:1]

## Wrong expectation: this is not free Anthropic Claude

The video clearly states that this method does not provide free access to Anthropic's own Claude models. It allows Claude Code to run against compatible alternative models through NVIDIA NIM. [web:34]

## Admin UI URL differs from examples

The upstream repo says the local server prints the correct Admin UI address in the terminal and that the port may differ from `8082`. Use the URL shown by your running server instead of assuming the example port. [page:2]

## Tool use problems

NVIDIA's direct integration docs warn that Claude Code requires tool calling. If the chosen NIM model does not support tool use well enough, the coding workflow may break or degrade. [page:1]
