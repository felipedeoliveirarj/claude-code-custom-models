# Skill: Trocar o Modelo do Claude Code no Antigravity

## Contexto

O **Antigravity** é um VS Code com branding customizado. Ele tem dois arquivos de configuração separados:

| Arquivo | Afeta |
|---|---|
| `%USERPROFILE%\.claude\settings.json` | Claude Code via terminal/CLI |
| `%APPDATA%\Roaming\Antigravity\User\settings.json` | Extensão Claude Code dentro do Antigravity |

**Para trocar o modelo dentro do Antigravity, o arquivo correto é o segundo.**

---

## Armadilhas Conhecidas

### 1. Formato errado de `environmentVariables`
A extensão Claude Code (`anthropic.claude-code`) exige **array**, não objeto:

```json
// ❌ ERRADO (formato do Kimi)
"claudeCode.environmentVariables": {
    "ANTHROPIC_BASE_URL": "...",
    "ANTHROPIC_AUTH_TOKEN": "..."
}

// ✅ CORRETO
"claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "..." },
    { "name": "ANTHROPIC_AUTH_TOKEN", "value": "..." }
]
```

### 2. Chave errada
- Kimi usa: `kimi.environmentVariables` (objeto)
- Claude Code usa: `claudeCode.environmentVariables` (array)

### 3. Depois de salvar, é necessário reiniciar o Antigravity completamente (fechar e abrir)

---

## Template de settings.json (Antigravity)

```json
{
    "roo-cline.debug": false,
    "roo-cline.allowedCommands": ["git log", "git diff", "git show"],
    "roo-cline.deniedCommands": [],
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

---

## Modelos Configurados

### MiniMax M2.7 (padrão atual)
```json
{ "name": "ANTHROPIC_BASE_URL", "value": "https://api.minimax.io/anthropic" },
{ "name": "ANTHROPIC_AUTH_TOKEN", "value": "SUA_CHAVE_MINIMAX" },
{ "name": "ANTHROPIC_MODEL", "value": "MiniMax-M2.7" }
```
- Endpoint compatível com Anthropic Messages API
- **Sem acesso à internet**

### Kimi K2 via BytePlus ARK (alternativa)
O endpoint BytePlus `/api/coding` **não é compatível** com o Claude Code.
Use o Kimi Code como extensão separada (já instalada: `moonshot-ai.kimi-code`),
configurado via `kimi.environmentVariables` no mesmo `settings.json`.

### Claude Anthropic (original)
Remover todas as variáveis de ambiente — o Claude Code autentica via conta Anthropic.

---

## Como Alternar Modelos via PowerShell

Adicione no `$PROFILE` do PowerShell para iniciar pelo terminal:

```powershell
# Claude Code com MiniMax
function cc-mini {
    $env:ANTHROPIC_BASE_URL = "https://api.minimax.io/anthropic"
    $env:ANTHROPIC_AUTH_TOKEN = "SUA_CHAVE_MINIMAX"
    claude
}

# Claude Code com Kimi (requer endpoint compatível)
function cc-kimi {
    $env:ANTHROPIC_BASE_URL = "https://ark.ap-southeast.byteplus.com/api/v3"
    $env:ANTHROPIC_AUTH_TOKEN = "SUA_CHAVE_BYTEPLUS"
    claude
}
```

---

## Diagnóstico Rápido

Se o ícone do Claude Code sumir após editar o settings.json:
1. Verifique se `claudeCode.environmentVariables` é **array** `[...]` e não objeto `{...}`
2. Verifique se o JSON é válido (sem vírgulas extras, chaves fechadas)
3. Reinicie o Antigravity completamente

Logs do Antigravity: `%APPDATA%\Roaming\Antigravity\logs\` → pasta com data mais recente → `cloudcode.log`
