# Usage and Operations

This page focuses on day-to-day commands, service mode, and troubleshooting.

## First-Run Flow

1. Initialize:

```bash
nullclaw onboard --interactive
```

2. Send a test message:

```bash
nullclaw agent -m "hello nullclaw"
```

3. Start long-running gateway:

```bash
nullclaw gateway
```

## Command Quick Reference

| Command | Purpose |
|---|---|
| `nullclaw onboard --api-key sk-... --provider openrouter` | Quick setup for provider and API key |
| `nullclaw onboard --interactive` | Full interactive setup |
| `nullclaw onboard --channels-only` | Reconfigure channels and allowlists only |
| `nullclaw agent -m "..."` | Single-message mode |
| `nullclaw agent` | Interactive mode |
| `nullclaw gateway` | Start long-running runtime (default `127.0.0.1:3000`) |
| `nullclaw service install` | Install background service |
| `nullclaw service start` | Start background service |
| `nullclaw service status` | Check service status |
| `nullclaw service stop` | Stop background service |
| `nullclaw service uninstall` | Uninstall background service |
| `nullclaw doctor` | Run diagnostics |
| `nullclaw status` | Show global status |
| `nullclaw channel status` | Show channel health |
| `nullclaw channel start telegram` | Start a specific channel |
| `nullclaw migrate openclaw --dry-run` | Dry-run OpenClaw migration |
| `nullclaw migrate openclaw` | Execute OpenClaw migration |

## Service Mode Recommendations

For long-running deployments:

```bash
nullclaw service install
nullclaw service start
nullclaw service status
```

After significant config changes, restart service:

```bash
nullclaw service stop
nullclaw service start
```

## Gateway and Pairing

- Default gateway: `127.0.0.1:3000`
- Recommended: keep `gateway.require_pairing = true`
- For public access, prefer tunnel/reverse proxy over direct public bind

Health check:

```bash
curl http://127.0.0.1:3000/health
```

## FAQ

### 1) Startup fails with config error

Steps:

1. Run `nullclaw doctor` for exact error details.
2. Compare with `config.example.json` for key names and nesting.
3. Validate JSON syntax (commas, quotes, braces).

### 2) Model calls fail (401/403)

Common causes:

- API key invalid/expired.
- Provider mismatch (for example, wrong key for selected provider).
- Invalid model route format/string.

Checks:

```bash
nullclaw status
```

Then re-run onboarding:

```bash
nullclaw onboard --interactive
```

### 3) Channel receives no messages

Check:

- `channels.<name>.accounts.*` token/webhook/account settings.
- `allow_from` accidentally set to empty array.
- `nullclaw channel status` health output.

### 4) Gateway starts but is unreachable externally

Common causes:

- Still bound to `127.0.0.1`.
- Tunnel/reverse proxy not configured.
- Firewall port not opened.

## Post-Change Checklist

After config edits:

```bash
nullclaw doctor
nullclaw status
nullclaw channel status
nullclaw agent -m "self-check"
```

For gateway scenarios:

```bash
nullclaw gateway
curl http://127.0.0.1:3000/health
```
