# Claude Code · Custom Models

Practical guide for using **alternative models** with Claude Code — inside VS Code, Antigravity, or any fork.

By default, Claude Code authenticates with Anthropic and uses their models (Sonnet, Opus). But you can point it to any model that implements the **Anthropic Messages API**, such as MiniMax, DeepSeek, or any compatible proxy.

---

## What is Antigravity?

[Antigravity](https://antigravity.dev) is a custom-branded VS Code, focused on AI-powered development. It comes with pre-installed extensions like Claude Code, Kimi Code, and Roo Code.

---

## How Model Swapping Works

Claude Code uses three environment variables to connect:

| Variable | Description |
|---|---|
| `ANTHROPIC_BASE_URL` | API endpoint (must be compatible with Anthropic Messages API) |
| `ANTHROPIC_AUTH_TOKEN` | Your API key from the chosen provider |
| `ANTHROPIC_MODEL` | Name of the model to use |

---

## Configuration in Antigravity (VS Code)

> ⚠️ Antigravity has **two separate configuration files**. To affect the Claude Code extension inside the editor, the correct file is:
> ```
> %APPDATA%\Roaming\Antigravity\User\settings.json
> ```
> (Not `~/.claude/settings.json`, which only affects the CLI)

### `settings.json` Template

```json
{
    "claudeCode.preferredLocation": "panel",
    "claudeCode.environmentVariables": [
        { "name": "ANTHROPIC_BASE_URL", "value": "YOUR_MODEL_URL_HERE" },
        { "name": "ANTHROPIC_AUTH_TOKEN", "value": "YOUR_API_KEY_HERE" },
        { "name": "ANTHROPIC_MODEL", "value": "YOUR_MODEL_NAME_HERE" },
        { "name": "API_TIMEOUT_MS", "value": "3000000" },
        { "name": "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC", "value": "1" }
    ]
}
```

> ✅ After saving, **completely restart Antigravity** for the variables to take effect.

---

## Tested Models

### MiniMax M2.7
```json
{ "name": "ANTHROPIC_BASE_URL", "value": "https://api.minimax.io/anthropic" },
{ "name": "ANTHROPIC_AUTH_TOKEN", "value": "sk-cp-..." },
{ "name": "ANTHROPIC_MODEL", "value": "MiniMax-M2.7" }
```
- ✅ Compatible with Anthropic Messages API
- ❌ No internet access / web search

### Kimi K2 (via separate extension)
The BytePlus Kimi endpoint (`/api/coding`) is **not compatible** with Claude Code.  
Use the **Kimi Code** extension (`moonshot-ai.kimi-code`) that comes pre-installed in Antigravity, configured via `kimi.environmentVariables`.

### Claude Anthropic (original)
Remove all environment variables — Claude Code will authenticate via your Anthropic account normally.

---

## Common Pitfalls

### ❌ Wrong `environmentVariables` format
The Claude Code extension requires an **array**, not an object:

```json
// WRONG (Kimi format)
"claudeCode.environmentVariables": {
    "ANTHROPIC_BASE_URL": "..."
}

// CORRECT
"claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "..." }
]
```

### ❌ Editing the wrong file
- `~/.claude/settings.json` → affects the **CLI** (`claude` in terminal)  
- `%APPDATA%\Roaming\Antigravity\User\settings.json` → affects the **extension in editor**

### ❌ Claude Code icon disappeared after editing
1. Check if `claudeCode.environmentVariables` is an **array** `[...]`
2. Check if the JSON is valid (no trailing commas)
3. Restart Antigravity completely

---

## Diagnostics

Extension logs:
```
%APPDATA%\Roaming\Antigravity\logs\<date-folder>\cloudcode.log
```

---

## Switching Models via PowerShell (CLI)

Add to your `$PROFILE`:

```powershell
function cc-mini {
    $env:ANTHROPIC_BASE_URL = "https://api.minimax.io/anthropic"
    $env:ANTHROPIC_AUTH_TOKEN = "YOUR_KEY"
    claude
}
```

---

## Contributing

PRs welcome with other tested models, compatible endpoints, or configuration tips.

---

Made with ☕ by [@felipedeoliveirarj](https://github.com/felipedeoliveirarj)
