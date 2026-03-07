# Configuration

NullClaw is compatible with OpenClaw config structure and uses `snake_case` keys.

## Config File Path

- macOS/Linux: `~/.nullclaw/config.json`
- Windows: `%USERPROFILE%\\.nullclaw\\config.json`

Recommended first step:

```bash
nullclaw onboard --interactive
```

This generates your initial config file.

## Minimal Working Config

The example below is enough to run local CLI mode (replace API key):

```json
{
  "models": {
    "providers": {
      "openrouter": {
        "api_key": "YOUR_OPENROUTER_API_KEY"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "openrouter/anthropic/claude-sonnet-4"
      }
    }
  },
  "channels": {
    "cli": true
  },
  "memory": {
    "backend": "sqlite",
    "auto_save": true
  },
  "gateway": {
    "host": "127.0.0.1",
    "port": 3000,
    "require_pairing": true
  },
  "autonomy": {
    "level": "supervised",
    "workspace_only": true,
    "max_actions_per_hour": 20
  },
  "security": {
    "sandbox": {
      "backend": "auto"
    },
    "audit": {
      "enabled": true
    }
  }
}
```

## Core Sections

### `models.providers`

- Defines LLM provider connection parameters and API keys.
- Common providers: `openrouter`, `openai`, `anthropic`, `groq`.

Example:

```json
{
  "models": {
    "providers": {
      "openrouter": { "api_key": "sk-or-..." },
      "anthropic": { "api_key": "sk-ant-..." },
      "openai": { "api_key": "sk-..." }
    }
  }
}
```

### `agents.defaults.model.primary`

- Sets default model route, typically `provider/vendor/model`.
- Example: `openrouter/anthropic/claude-sonnet-4`

### `channels`

- Channel config lives under `channels.<name>`.
- Multi-account channels typically use an `accounts` wrapper.

Telegram example:

```json
{
  "channels": {
    "telegram": {
      "accounts": {
        "main": {
          "bot_token": "123456:ABCDEF",
          "allow_from": ["YOUR_TELEGRAM_USER_ID"]
        }
      }
    }
  }
}
```

Rules:

- `allow_from: []` means deny all inbound messages.
- `allow_from: ["*"]` means allow all sources (use only when you accept the risk).

### `memory`

- `backend`: start with `sqlite`.
- `auto_save`: persists conversation memory automatically.
- For hybrid retrieval and embedding settings, see root `config.example.json`.

### `gateway`

Recommended defaults:

- `host = "127.0.0.1"`
- `require_pairing = true`

Avoid direct public exposure. Use tunnel when external access is required.

### `autonomy`

- `level`: start with `supervised`.
- `workspace_only`: keep `true` to limit file access scope.
- `max_actions_per_hour`: keep conservative limits first.

### `security`

- `sandbox.backend = "auto"`: auto-selects an available sandbox backend.
- `audit.enabled = true`: recommended for traceability.

### Advanced: Web Search + Full Shell (high risk)

Use only in controlled environments:

```json
{
  "http_request": {
    "enabled": true,
    "search_base_url": "https://searx.example.com",
    "search_provider": "auto",
    "search_fallback_providers": ["jina", "duckduckgo"]
  },
  "autonomy": {
    "level": "full",
    "allowed_commands": ["*"],
    "allowed_paths": ["*"],
    "require_approval_for_medium_risk": false,
    "block_high_risk_commands": false
  }
}
```

Notes:

- `search_base_url` must be a valid URL, otherwise startup validation fails.
- `allowed_commands: ["*"]` and `allowed_paths: ["*"]` significantly widen execution scope.

## Validate After Config Changes

After each config change:

```bash
nullclaw doctor
nullclaw status
nullclaw channel status
```

If gateway/channel changed, also run:

```bash
nullclaw gateway
```
