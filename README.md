# pi-gbrain

A small [Pi](https://github.com/earendil-works/pi) extension for [GBrain](https://github.com/garrytan/gbrain): lifecycle hooks and optional ambient memory. One TypeScript file, no runtime dependencies.

## Install

Install and configure the GBrain CLI separately, then:

```sh
pi install git:github.com/VanDalkvist/pi-gbrain
```

**Nothing runs until you opt in.** Set these variables in the environment that launches Pi:

```sh
# Ambient recall only:
PI_GBRAIN_MEMORY=1 pi

# Recall plus GBrain lifecycle hooks:
PI_GBRAIN_MEMORY=1 PI_GBRAIN_HOOKS=1 pi
```

Optional settings:

- `GBRAIN_BIN` — executable path; defaults to `gbrain` on PATH.
- `PI_GBRAIN_ENTITIES` — comma-separated names/slugs; defaults to `people/me`.
- `PI_GBRAIN_MEMORY_FILE` — explicitly selected UTF-8 fallback file. Relative paths resolve against the session working directory; `~/` is supported. Files over 128 KiB are ignored. There is **no automatic MEMORY.md lookup**.

Restart Pi after changing these variables. To remove: `pi remove git:github.com/VanDalkvist/pi-gbrain`.

## Behavior

With memory enabled, the extension warms a 3,000-token `context-pack` in the background. Each new memory snapshot is appended as a persistent custom message, hidden from the chat UI but stored in the session and sent to the model. Unchanged snapshots are not repeated. **The system prompt and previous messages are never rewritten**, preserving Pi's append-only context strategy and leaving existing request prefixes intact. Actual provider cache hits still depend on the provider, retention, and other extensions.

The local retrieval cache lasts ten minutes, per extension instance; it is separate from the provider's prompt cache. Session changes and compaction reset the cache and deduplication, so memory can be reintroduced afterward. Branch navigation also resets deduplication for the destination branch. Earlier memory messages remain in conversation history until normal compaction; a newer snapshot supersedes older memory snapshots. The first turn may have no recalled memory while the background request completes; only an explicitly configured fallback is used then.

With hooks enabled, Pi events call `gbrain hook session-start`, `stop`, `compact`, and `session-end`. Calls send only the session ID and working directory, not conversation messages. **GBrain's hooks can persist data or run other side effects according to your GBrain configuration.** Enable them only if that is intended.

Commands use no shell, have a seven-second timeout and 128 KiB output limit, and fail open without displaying CLI output. Session shutdown waits for the session-end hook, up to that timeout.

## Privacy and limits

Enabling memory sends retrieved context (or the entire configured fallback file) to the model serving the Pi session. Model/provider logs and session storage may retain it. Only enable it in sessions authorized to access that memory; opt-in is not a multi-user access-control boundary. GBrain chooses readable sources and visibility using its own configuration; this extension does not request private widening.

No credentials, personal memory, local-machine paths, transcripts, or inherited installation history are included. Existing Pi/GBrain installations are not changed by this repository.

The extension does not register MCP tools. Configure GBrain MCP separately if you want the agent to search or save memory. Its storage guidance is advisory: **this is not a filesystem security sandbox** and it does not intercept write/edit/bash tools.

## License

MIT © 2026 VanDalkvist
