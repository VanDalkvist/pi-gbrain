# pi-gbrain

A small Pi extension integrating the GBrain CLI. Keep the package portable and privacy-conscious.

- Never commit credentials, personal memory, local machine paths, real user fixtures, logs, or transcripts.
- Memory injection and lifecycle hooks require explicit opt-in; importing or loading the default extension must not read memory or spawn GBrain.
- Keep mutable state local to each extension instance. Use the supported Pi extension API.
- Tests use synthetic temporary fixtures and fake CLI processes only, never a real brain.
- Keep changes focused. Run tests and typechecking before claiming completion.
- Use public GitHub noreply author metadata. Do not alter another installation or its settings.
