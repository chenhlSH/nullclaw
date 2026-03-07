# Gateway API

Default gateway endpoint: `http://127.0.0.1:3000`

## Endpoints

| Endpoint | Method | Auth | Description |
|---|---|---|---|
| `/health` | GET | None | Health check |
| `/pair` | POST | `X-Pairing-Code` | Exchange one-time pairing code for bearer token |
| `/webhook` | POST | `Authorization: Bearer <token>` | Send message payload: `{"message":"..."}` |
| `/whatsapp` | GET | Query params | Meta webhook verification |
| `/whatsapp` | POST | Meta signature | WhatsApp inbound webhook |

## Quick Examples

### 1) Health check

```bash
curl http://127.0.0.1:3000/health
```

### 2) Pair and get token

```bash
curl -X POST \
  -H "X-Pairing-Code: 123456" \
  http://127.0.0.1:3000/pair
```

Expected: bearer token response (exact JSON shape may vary by version).

### 3) Send webhook message

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"message":"hello from webhook"}' \
  http://127.0.0.1:3000/webhook
```

## Security Guidance

1. Keep `gateway.require_pairing = true`.
2. Keep gateway on loopback (`127.0.0.1`) and expose externally through tunnel/proxy.
3. Treat bearer tokens as secrets; do not commit or log them.
