# pi-gbrain Implementation Plan

> For agentic workers: use executing-plans with test-driven-development. This is a bounded extraction, not a framework rewrite.

**Goal:** Ship the standalone clean Pi package described in [design.md](design.md).
**Architecture:** Thin extension entrypoint composes a bounded CLI adapter and per-instance memory/lifecycle handlers.
**Tech Stack:** TypeScript, Node APIs, Bun tests, host-provided Pi API.
**Spec:** docs/design.md (approved scope).

## Global constraints
Fresh history; portable paths; explicit opt-in; no real brain access in tests; never modify the original installation. No secrets or personal data in any commit. Keep source compact; split only by responsibility.

## Review focus
Disabled load must have no I/O; failed CLI must not leak stderr or crash on stdin; session changes must not retain another session's memory; invalid settings must not silently broaden memory access; packaged files must exclude local configuration.

## Task 1: Walking skeleton and privacy-safe config
- [ ] Add manifest, TypeScript check, synthetic test helper and public entrypoint tests first; run them RED.
- [ ] Implement configuration and the minimal load -> opted-in CLI -> prompt path; assert disabled load performs zero I/O and opted-in load injects the fake CLI's context.
- [ ] Verify GREEN and checkpoint to the private remote.

## Task 2: Lifecycle, failure handling and portability
- [ ] Add behavioral RED tests for lifecycle payloads, explicit-file-only fallback, missing CLI, malformed context, timeout, early stdin close, independent instances and session cache reset.
- [ ] Implement bounded child-process handling, per-instance cache and documented hooks/guard; no forced visibility, personal path or implicit MEMORY.md fallback.
- [ ] Verify GREEN, typecheck, and checkpoint.

## Task 3: Distribution and acceptance
- [ ] Write installation, env opt-ins, privacy/limitations, license, and clean examples.
- [ ] Inspect package file list and scan full new history and working tree with redaction enabled.
- [ ] Independent architecture and black-box product verification; return concrete findings to the implementer and resolve bounded defects.
- [ ] Re-run final verification; publish only the clean standalone repository. Keep evidence outside the package.

## Ledger
Bootstrap: approved clean extraction; no historical import. Original extension remains untouched. Optional broad refactors and hard security sandboxing are out of scope.
