# Claude Code · Custom Models

Guia prático para usar **modelos alternativos** no Claude Code — dentro do VS Code, Antigravity ou qualquer fork.

Por padrão, o Claude Code autentica com a Anthropic e usa os modelos deles (Sonnet, Opus). Mas é possível apontar para qualquer modelo que implemente a **Anthropic Messages API**, como MiniMax, DeepSeek, ou qualquer proxy compatível.

---

## O que é o Antigravity?

O [Antigravity](https://antigravity.dev) é um VS Code com branding customizado, voltado para desenvolvimento com IA. Ele vem com extensões pré-instaladas como Claude Code, Kimi Code e Roo Code.

---

## Como funciona a troca de modelo

O Claude Code usa três variáveis de ambiente para se conectar:

| Variável | Descrição |
|---|---|
| `ANTHROPIC_BASE_URL` | Endpoint da API (deve ser compatível com Anthropic Messages API) |
| `ANTHROPIC_AUTH_TOKEN` | Sua API key do provedor escolhido |
| `ANTHROPIC_MODEL` | Nome do modelo a usar |

---

## Configuração no Antigravity (VS Code)

> ⚠️ O Antigravity tem **dois arquivos de configuração separados**. Para afetar a extensão Claude Code dentro do editor, o arquivo correto é:
> ```
> %APPDATA%\Roaming\Antigravity\User\settings.json
> ```
> (Não o `~/.claude/settings.json`, que afeta apenas o CLI)

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

> ✅ Após salvar, **reinicie o Antigravity completamente** para as variáveis entrarem em vigor.

---

## Modelos Testados

### MiniMax M2.7
```json
{ "name": "ANTHROPIC_BASE_URL", "value": "https://api.minimax.io/anthropic" },
{ "name": "ANTHROPIC_AUTH_TOKEN", "value": "sk-cp-..." },
{ "name": "ANTHROPIC_MODEL", "value": "MiniMax-M2.7" }
```
- ✅ Compatível com Anthropic Messages API
- ❌ Sem acesso à internet / web search

### Kimi K2 (via extensão separada)
O endpoint BytePlus do Kimi (`/api/coding`) **não é compatível** com o Claude Code.  
Use a extensão **Kimi Code** (`moonshot-ai.kimi-code`) que vem pré-instalada no Antigravity, configurada via `kimi.environmentVariables`.

### Claude Anthropic (original)
Remova todas as variáveis de ambiente — o Claude Code vai autenticar via conta Anthropic normalmente.

---

## Armadilhas Comuns

### ❌ Formato errado de `environmentVariables`
A extensão Claude Code exige **array**, não objeto:

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

### ❌ Editando o arquivo errado
- `~/.claude/settings.json` → afeta o **CLI** (`claude` no terminal)  
- `%APPDATA%\Roaming\Antigravity\User\settings.json` → afeta a **extensão no editor**

### ❌ Ícone do Claude Code sumiu após editar
1. Verifique se `claudeCode.environmentVariables` é um **array** `[...]`
2. Verifique se o JSON é válido (sem vírgulas extras)
3. Reinicie o Antigravity completamente

---

## Diagnóstico

Logs da extensão:
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

PRs bem-vindos com outros modelos testados, endpoints compatíveis ou dicas de configuração.

---

Feito com ☕ por [@felipedeoliveirarj](https://github.com/felipedeoliveirarj)
