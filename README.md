# Claude Code Â· Custom Models

Guia prÃ¡tico para usar **modelos alternativos** no Claude Code â€” dentro do VS Code, Antigravity ou qualquer fork.

Por padrÃ£o, o Claude Code autentica com a Anthropic e usa os modelos deles (Sonnet, Opus). Mas Ã© possÃ­vel apontar para qualquer modelo que implemente a **Anthropic Messages API**, como MiniMax, DeepSeek, ou qualquer proxy compatÃ­vel.

---

## O que Ã© o Antigravity?

O [Antigravity](https://antigravity.dev) Ã© um VS Code com branding customizado, voltado para desenvolvimento com IA. Ele vem com extensÃµes prÃ©-instaladas como Claude Code, Kimi Code e Roo Code.

---

## Como funciona a troca de modelo

O Claude Code usa trÃªs variÃ¡veis de ambiente para se conectar:

| VariÃ¡vel | DescriÃ§Ã£o |
|---|---|
| `ANTHROPIC_BASE_URL` | Endpoint da API (deve ser compatÃ­vel com Anthropic Messages API) |
| `ANTHROPIC_AUTH_TOKEN` | Sua API key do provedor escolhido |
| `ANTHROPIC_MODEL` | Nome do modelo a usar |

---

## ConfiguraÃ§Ã£o no Antigravity (VS Code)

> âš ï¸ O Antigravity tem **dois arquivos de configuraÃ§Ã£o separados**. Para afetar a extensÃ£o Claude Code dentro do editor, o arquivo correto Ã©:
> ```
> %APPDATA%\Roaming\Antigravity\User\settings.json
> ```
> (NÃ£o o `~/.claude/settings.json`, que afeta apenas o CLI)

### Template de `settings.json`

```json
{
    "claudeCode.preferredLocation": "panel",
    "claudeCode.environmentVariables": [
        { "name": "ANTHROPIC_BASE_URL", "value": "URL_DO_MODELO_AQUI" },
        { "name": "ANTHROPIC_AUTH_TOKEN", "value": "SUA_API_KEY_AQUI" },
        { "name": "ANTHROPIC_MODEL", "value": "NOME_DO_MODELO_AQUI" },
        { "name": "API_TIMEOUT_MS", "value": "3000000" },
        { "name": "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC", "value": "1" }
    ]
}
```

> âœ… ApÃ³s salvar, **reinicie o Antigravity completamente** para as variÃ¡veis entrarem em vigor.

---

## Modelos Testados

### MiniMax M2.7
```json
{ "name": "ANTHROPIC_BASE_URL", "value": "https://api.minimax.io/anthropic" },
{ "name": "ANTHROPIC_AUTH_TOKEN", "value": "sk-cp-..." },
{ "name": "ANTHROPIC_MODEL", "value": "MiniMax-M2.7" }
```
- âœ… CompatÃ­vel com Anthropic Messages API
- âŒ Sem acesso Ã  internet / web search

### Kimi K2 (via extensÃ£o separada)
O endpoint BytePlus do Kimi (`/api/coding`) **nÃ£o Ã© compatÃ­vel** com o Claude Code.  
Use a extensÃ£o **Kimi Code** (`moonshot-ai.kimi-code`) que vem prÃ©-instalada no Antigravity, configurada via `kimi.environmentVariables`.

### Claude Anthropic (original)
Remova todas as variÃ¡veis de ambiente â€” o Claude Code vai autenticar via conta Anthropic normalmente.

---

## Armadilhas Comuns

### âŒ Formato errado de `environmentVariables`
A extensÃ£o Claude Code exige **array**, nÃ£o objeto:

```json
// ERRADO (formato do Kimi)
"claudeCode.environmentVariables": {
    "ANTHROPIC_BASE_URL": "..."
}

// CORRETO
"claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "..." }
]
```

### âŒ Editando o arquivo errado
- `~/.claude/settings.json` â†’ afeta o **CLI** (`claude` no terminal)  
- `%APPDATA%\Roaming\Antigravity\User\settings.json` â†’ afeta a **extensÃ£o no editor**

### âŒ Ãcone do Claude Code sumiu apÃ³s editar
1. Verifique se `claudeCode.environmentVariables` Ã© um **array** `[...]`
2. Verifique se o JSON Ã© vÃ¡lido (sem vÃ­rgulas extras)
3. Reinicie o Antigravity completamente

---

## DiagnÃ³stico

Logs da extensÃ£o:
```
%APPDATA%\Roaming\Antigravity\logs\<pasta-com-data>\cloudcode.log
```

---

## Alternar modelos via PowerShell (CLI)

Adicione no seu `$PROFILE`:

```powershell
function cc-mini {
    $env:ANTHROPIC_BASE_URL = "https://api.minimax.io/anthropic"
    $env:ANTHROPIC_AUTH_TOKEN = "SUA_CHAVE"
    claude
}
```

---

## Contribuindo

PRs bem-vindos com outros modelos testados, endpoints compatÃ­veis ou dicas de configuraÃ§Ã£o.

---

Feito com â˜• por [@felipedeoliveirarj](https://github.com/felipedeoliveirarj)