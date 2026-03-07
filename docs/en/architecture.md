# Architecture

NullClaw uses a vtable-driven pluggable architecture. Most capabilities are extended by implementing interfaces and registering factories.

## Core Design

- Subsystems are abstracted via interfaces using `ptr: *anyopaque + vtable`.
- Runtime implementation is selected through factories.
- Provider/channel/tool/memory swaps should not require core orchestration rewrites.

## Subsystems and Extension Points

| Subsystem | Interface | Built-in implementations (partial) | Extension approach |
|---|---|---|---|
| AI Models | `Provider` | OpenRouter, Anthropic, OpenAI, Ollama, Groq, and more | Add provider implementation + register |
| Channels | `Channel` | CLI, Telegram, Signal, Discord, Slack, Matrix, WhatsApp, Nostr, and more | Add channel implementation + register |
| Memory | `Memory` | SQLite (hybrid retrieval), Markdown | Add memory backend |
| Tools | `Tool` | shell, file_read, file_write, http_request, browser_open, and more | Add tool implementation |
| Observability | `Observer` | Noop, Log, File, Multi | Add observer backend |
| Runtime | `RuntimeAdapter` | Native, Docker, WASM | Add runtime adapter |
| Security | `Sandbox` | Landlock, Firejail, Bubblewrap, Docker(auto) | Add sandbox backend |
| Tunnel | `Tunnel` | None, Cloudflare, Tailscale, ngrok, Custom | Add tunnel provider |
| Peripheral | `Peripheral` | Serial, Arduino, RPi GPIO, STM32/Nucleo | Add hardware driver |

## Memory Stack

| Layer | Implementation |
|---|---|
| Vector retrieval | Embeddings as BLOB in SQLite, cosine similarity search |
| Keyword retrieval | SQLite FTS5 with BM25 |
| Hybrid merge | Weighted vector + keyword merge |
| Embeddings | `EmbeddingProvider` vtable (OpenAI/custom/noop) |
| Data hygiene | Automatic archive and purge |
| Snapshots | Full export/import migration path |

## Practical Constraints

1. Prefer extension through implementations, not invasive core rewrites.
2. Keep subsystem boundaries strict (avoid cross-subsystem internals coupling).
3. For high-risk paths (`security/runtime/gateway/tools`), include boundary/failure-path validation.
