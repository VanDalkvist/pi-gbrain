# pi-gbrain: standalone clean extraction

## Goal
Publish a portable Pi extension based on the existing GBrain lifecycle integration, without personal configuration, fixtures, memory, or inherited Git history. The existing installation and its source remain unchanged.

## Approach
A fresh, small TypeScript package with an explicit Pi extension manifest, GBrain CLI adapter, instance-local cache, synthetic tests, and MIT license. No runtime dependencies beyond Node and the host Pi API. The CLI remains separately installed; this package does not configure MCP or provide memory tools itself.

An alternative history rewrite is unnecessary: the new repository starts from a clean snapshot. A settings migration is explicitly out of scope.

## Privacy and behavior
Loading the package alone reads no memory and launches no GBrain process. Ambient recall is explicitly enabled with PI_GBRAIN_MEMORY=1. Lifecycle hooks are separately enabled with PI_GBRAIN_HOOKS=1. GBRAIN_BIN defaults to gbrain on PATH. Entities are configurable, defaulting to the generic people/me convention. A local memory fallback is read only from an explicitly configured PI_GBRAIN_MEMORY_FILE; never implicitly from cwd or a fixed workspace path. Memory is sent to the session's model only when enabled, and README must say so. No source-wide visibility policy is forced onto users.

Preserve a short storage-contract advisory and best-effort direct-write guard only in the enabled memory integration; do not claim the lexical guard is a security sandbox. Use documented host lifecycle events; no private systemPromptOptions mutation. Bound subprocess time/output, fail open without logging child output, and clear stale/cross-session cache. No automatic ingestion setup or model calls in tests.

## Acceptance
Pi discovers the package; disabled load is inert; explicit memory enable injects only synthetic configured context; hooks receive expected minimal payloads; missing executable, invalid JSON, timeout, and broken stdin do not crash Pi; independent instances do not share memory. Tests, typecheck, package allowlist inspection, and secret/PII scans pass before public visibility. Repository history contains only new sanitized commits and public noreply author metadata.
